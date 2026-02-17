---
title: "Firmware Structure & Patterns"
weight: 40
bookCollapseSection: true
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Firmware Structure & Patterns

Making code predictable. Embedded firmware runs without an operating system to manage execution, allocate resources, or recover from errors. The firmware *is* the system — it decides what runs, when, and in what order. Structure matters more here than in application software because there is no safety net: a mishandled interrupt or a missed state transition can lock up the system or produce silent, intermittent failures.

The key patterns in bare-metal firmware — startup sequences, interrupt-driven I/O, and state machines — exist because they solve real problems: how to initialize hardware in the right order, how to respond to events without busy-waiting, and how to manage complex behavior without spaghetti code. These patterns aren't academic; they're the difference between firmware that works reliably and firmware that works until it doesn't.

## What This Section Covers

- **[Startup & Initialization]({{< relref "startup-and-initialization" >}})** — Vector table, startup code, C runtime initialization, and main loop patterns: what happens between power-on and the first line of application code.
- **[Interrupts]({{< relref "interrupts" >}})** — ISR design rules, shared data patterns, and critical sections: the core mechanism for real-time response.
- **[Cortex-M Interrupts and the NVIC]({{< relref "cortex-m-interrupts" >}})** — NVIC configuration, priority grouping, tail-chaining, latency, and jitter on ARM Cortex-M.
- **[State Machines & Event Loops]({{< relref "state-machines-and-event-loops" >}})** — Cooperative scheduling, event-driven design, and superloop vs dispatch patterns: organizing firmware behavior.
