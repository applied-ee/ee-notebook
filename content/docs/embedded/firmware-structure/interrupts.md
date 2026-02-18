---
title: "Interrupts"
weight: 20
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Interrupts

Interrupts are the fundamental mechanism for real-time response in embedded systems. Instead of constantly polling a peripheral to see if something happened, the hardware itself signals the CPU: "stop what you are doing and handle this." The CPU saves its current context, jumps to a handler function, executes it, restores context, and resumes where it left off. This sounds straightforward, but the details — priority levels, nesting, shared data, and latency — are where most embedded bugs come from.

For ARM Cortex-M specifics — NVIC configuration, priority grouping, tail-chaining, and latency characteristics — see [Cortex-M Interrupts and the NVIC]({{< relref "cortex-m-interrupts" >}}).

## How Interrupts Work

When a peripheral (timer overflow, UART byte received, GPIO edge detected) asserts its interrupt line, the CPU finishes the current instruction, saves a set of registers to the stack, fetches the handler address from a vector table, and begins executing the ISR. When the ISR returns, the CPU restores the saved registers and resumes the interrupted code.

The details of context save vary by architecture. Some processors (like ARM Cortex-M) handle it entirely in hardware — the CPU pushes a fixed set of registers automatically, and ISR functions in C look like normal functions. Others (like AVR) rely on the compiler to generate explicit save/restore sequences, requiring special macros or attributes (`ISR()`, `__attribute__((interrupt))`) to get the right code generation.

On architectures with an interrupt controller (like the Cortex-M NVIC), higher-priority interrupts can preempt lower-priority ones already in progress, and the hardware fetches handler addresses directly from the vector table without software dispatch. Simpler architectures may use a single interrupt vector with software polling to determine the source.

## ISR Design Rules

The overriding principle: **keep ISRs short**. The ISR is executing at elevated priority, blocking all equal- and lower-priority interrupts. Every cycle spent in the ISR is a cycle that other interrupt sources must wait.

Practical rules I have collected (and am still learning to apply consistently):

- **Do the minimum work** — Read the data register, copy the value to a buffer, clear the interrupt flag, set a flag for the main loop. That is it.
- **Never call blocking functions** — No `delay_ms()`, no busy-wait loops, no `printf()`. These belong in main loop context.
- **Never allocate memory** — `malloc()` is not reentrant on most embedded C libraries. Calling it from an ISR risks heap corruption.
- **Clear the interrupt source flag** — If the peripheral's interrupt flag is not cleared, the ISR returns and the interrupt controller immediately re-enters it. This creates an interrupt storm: the main loop never runs, the system appears locked up, and the watchdog (if present) eventually resets the device.
- **Keep shared data access atomic** — If the ISR writes a variable that the main loop reads, consider whether the access is atomic on the target platform. Atomicity depends on the bus width and architecture — always verify for the specific target.

## Shared Data and volatile

When an ISR sets a flag that the main loop checks, the variable must be declared `volatile`. Without it, the compiler may optimize the main-loop read into a single load that is never refreshed — the classic "flag never seen" bug:

```c
volatile uint8_t data_ready = 0;

void USART1_IRQHandler(void) {
    rx_buffer[rx_head++] = USART1->DR;
    data_ready = 1;
}

int main(void) {
    while (1) {
        if (data_ready) {    // without volatile, compiler may hoist this check
            process_data();
            data_ready = 0;
        }
    }
}
```

But `volatile` only prevents compiler reordering and caching — it does not make multi-byte accesses atomic. Writing a `uint64_t` from an ISR and reading it in `main()` can produce a torn read (half old value, half new) even with `volatile`. For multi-byte shared data, disable interrupts briefly around the access or use a lock-free structure like a ring buffer. See the discussion of race conditions in [State Machines & Event Loops]({{< relref "state-machines-and-event-loops" >}}) for how event queues help with this.

## Disabling Interrupts

The simplest way to protect shared data is to disable interrupts:

```c
__disable_irq();
// critical section — read/modify/write shared data
__enable_irq();
```

This is a blunt instrument. While interrupts are disabled, every pending interrupt is delayed. The disabled window adds directly to worst-case interrupt latency for the entire system. Best practice is to keep critical sections as short as possible — ideally just a few instructions — and measure the actual disabled duration with a logic analyzer.

Some architectures offer finer-grained masking — for example, Cortex-M3+ provides `BASEPRI` to mask only interrupts below a given priority. See [Cortex-M Interrupts and the NVIC]({{< relref "cortex-m-interrupts" >}}) for details.

## Tips

- Keep ISRs as short as possible — set a flag, copy data to a buffer, clear the interrupt source, and return
- Always clear the peripheral's interrupt flag before returning from the ISR to prevent interrupt storms

## Caveats

- **Forgetting to clear the interrupt flag causes an interrupt storm** — The ISR runs, returns, and immediately re-enters because the peripheral still asserts the interrupt. The main loop starves. The system appears frozen. Always clear the source flag in the ISR
- **volatile does not mean atomic** — Declaring a shared variable `volatile` prevents the compiler from caching it in a register, but it does not prevent torn reads or writes for types wider than the bus width. A 16-bit variable on an 8-bit AVR requires interrupt protection even if it is `volatile`

## In Practice

- A system that appears frozen with high CPU utilization likely has an interrupt storm — verify interrupt flags are being cleared
- Shared variables that occasionally show corrupted values may have atomicity issues — verify access is protected or naturally atomic
