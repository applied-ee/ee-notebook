---
title: "Cortex-M Interrupts and the NVIC"
weight: 25
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Cortex-M Interrupts and the NVIC

This page covers interrupt handling specifics for the ARM Cortex-M architecture — the NVIC, hardware context save, priority grouping, tail-chaining, and latency characteristics. For general interrupt concepts, ISR design rules, and shared-data patterns, see {{< relref "interrupts" >}}.

## Cortex-M Context Save

When a Cortex-M takes an exception, the hardware automatically pushes eight registers onto the current stack: R0-R3, R12, LR, PC, and xPSR. This context save is done entirely in hardware — the compiler does not need to generate any special prologue or epilogue for ISR functions. C functions used as interrupt handlers look like normal functions.

This contrasts with architectures like AVR, where the compiler must generate explicit register save/restore sequences. On AVR, the `ISR()` macro or `__attribute__((interrupt))` tells the compiler to emit the right code generation for saving and restoring the full register set.

## The NVIC

The Nested Vectored Interrupt Controller is the hardware block that manages all external interrupts (and some internal exceptions) on Cortex-M. Every interrupt source gets a slot in the NVIC with:

- **Enable/disable** — Each interrupt can be individually enabled or disabled
- **Pending flag** — An interrupt can be "pending" (requested) even if it is disabled; when enabled, it fires immediately
- **Priority level** — A configurable numeric priority (lower number = higher priority on Cortex-M)

"Nested" means a higher-priority interrupt can preempt a lower-priority ISR that is already running. "Vectored" means the hardware fetches the handler address from the vector table directly, without software dispatch. This combination makes Cortex-M interrupt entry very fast compared to architectures that use a single interrupt handler and require software to determine the source.

### Tail-Chaining

If a second interrupt becomes pending while the CPU is finishing an ISR, the NVIC skips the context restore and immediately enters the next handler. This "tail-chaining" eliminates the overhead of popping and re-pushing the same registers and reduces back-to-back interrupt latency from ~24 cycles to ~6 cycles on Cortex-M3/M4.

## Priority Levels and Grouping

Cortex-M priority is encoded in a field that is typically 3-4 bits wide (8 or 16 levels), though the exact number depends on the implementation — many chips implement only the top 3 or 4 bits of the 8-bit priority field, making the effective range 0-7 or 0-15.

Priority grouping divides this field into two parts:

- **Preemption priority** — Determines whether one interrupt can preempt another. Only a numerically lower preemption priority can preempt.
- **Sub-priority** — Breaks ties when two interrupts with the same preemption priority are pending simultaneously. The one with the lower sub-priority runs first, but it will not preempt the other.

Setting the priority grouping wrong is a common and confusing bug. If all bits are assigned to sub-priority, no interrupt can preempt any other — the system behaves as if nesting is disabled. The CMSIS function `NVIC_SetPriorityGrouping()` controls this, and it should be configured once at startup before enabling any interrupts.

I am still building intuition for how to assign priorities in practice. The general guidance is: give the highest priority (lowest number) to interrupts with the hardest real-time deadlines (motor commutation, safety-critical timers), and lower priority to everything else (UART receive, background timers). But the number of levels is small, and getting the assignment wrong can lead to priority inversion or missed deadlines that only appear under load.

## Interrupt Latency on Cortex-M

Interrupt latency is the time from the hardware event (e.g., pin edge) to the first instruction of the ISR executing. On Cortex-M3/M4, this is deterministic: 12 clock cycles in the best case. That includes the exception entry sequence (stacking, vector fetch, pipeline refill).

In practice, latency is longer than 12 cycles because of:

- **Higher-priority ISR already running** — The pending interrupt waits until the higher-priority handler finishes
- **Disabled-interrupt regions** — Code that disables interrupts (via `__disable_irq()` or `PRIMASK`) adds the disabled window duration directly to the latency
- **Multi-cycle instructions** — The CPU must finish the current instruction before taking the exception. Most Cortex-M instructions are 1 cycle, but load/store multiples and divides can take many cycles
- **Flash wait states** — Vector fetch goes through the flash interface; additional wait states add cycles
- **Bus contention** — DMA and other bus masters competing for the same bus as the vector fetch

## Jitter

Jitter is the variation in interrupt latency from one occurrence to the next. Even if the average latency is acceptable, high jitter means some events are handled faster than others. For periodic tasks — sampling an ADC at a fixed rate, generating a precise PWM update — jitter directly translates to timing error.

Sources of jitter include everything listed under latency, plus: variable-length instruction sequences in higher-priority ISRs, cache misses (on M7), and flash accelerator behavior. Measuring jitter requires toggling a GPIO pin at the start of the ISR and capturing the timing on an oscilloscope or logic analyzer. I have found that jitter is often a bigger problem in practice than absolute latency — a system with 2 µs average latency and 10 µs jitter is harder to work with than one with 5 µs average and 0.5 µs jitter.

## BASEPRI

On Cortex-M3 and above, `BASEPRI` offers a finer-grained alternative to disabling all interrupts. It masks interrupts at or below a given priority level without affecting higher-priority ones. This allows protecting shared data from a specific set of ISRs without blocking time-critical higher-priority handlers.

I have not used `BASEPRI` extensively yet, but it seems like the right approach for systems with both hard-real-time and background interrupts.

## Tips

- Clear pending interrupt flags before enabling interrupts in the NVIC to avoid spurious immediate entry
- Configure priority grouping once at startup before enabling any interrupts
- Use tail-chaining awareness when profiling back-to-back interrupt performance — if you see ~6-cycle transitions, the NVIC is doing its job
- Consider `BASEPRI` instead of global interrupt disable when you need to protect data from specific ISRs without blocking higher-priority handlers

## Caveats

- **Priority number 0 is the highest priority, not the lowest** — This is backwards from most people's intuition. On Cortex-M, a lower numeric value means a higher urgency level. Setting a non-critical interrupt to priority 0 means nothing else can preempt it
- **Interrupt nesting depends on priority grouping configuration** — If all priority bits are assigned to sub-priority (grouping set to 0 preemption bits), no ISR can preempt any other, regardless of assigned priority numbers
- **Enabling an interrupt before its peripheral is configured risks an immediate spurious ISR** — If the peripheral's interrupt flag is already set (from a previous configuration or power-on state), enabling the interrupt in the NVIC causes immediate entry into the handler
- **Long ISRs cause jitter in all lower-priority interrupts** — A 100 µs ISR at priority 1 adds up to 100 µs of jitter to every interrupt at priority 2 or lower
- **Aligned 32-bit reads and writes are atomic on Cortex-M** — But 64-bit values or multi-field structures are not. Do not assume atomicity for anything wider than a single word

## In Practice

- Timing jitter in periodic interrupts often traces to long higher-priority ISRs — measure ISR execution time with GPIO toggling
- An ISR that fires immediately when enabled suggests the peripheral's interrupt flag was already set — clear pending flags before enabling
- A system where no interrupts preempt despite different priority numbers likely has priority grouping misconfigured — check `NVIC_SetPriorityGrouping()`
