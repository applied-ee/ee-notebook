---
title: "Interfacing Playbook: Modern ↔ Legacy"
weight: 80
bookCollapseSection: true
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Interfacing Playbook: Modern ↔ Legacy

This is the capstone for the Retro & Legacy section. Everything else in this section — [era constraints]({{< relref "../design-constraints-of-earlier-eras" >}}), [electrical assumptions]({{< relref "../electrical-assumptions" >}}), [legacy signaling]({{< relref "../legacy-signaling-and-interfaces" >}}), [aging behavior]({{< relref "../aging-drift-and-failure-modes" >}}) — leads here: actually connecting a modern microcontroller, SBC, or USB device to an older system without destroying either one.

This page is the systematic playbook: how to characterize an unknown interface, identify what can damage modern hardware, select the right protection and conversion topology, and bring it up safely on the bench.

- **[Domain Identification Checklist]({{< relref "domain-identification-checklist" >}})** — Voltage/current ranges, impedance, grounding model, isolation requirements, and fault scenarios to characterize before designing any interface.
- **[What Can Kill an MCU?]({{< relref "what-can-kill-an-mcu" >}})** — Overvoltage, negative voltage, ground offset, inductive kick, and backfeeding conditions that destroy CMOS I/O pins.
- **[Common Adapter Building Blocks]({{< relref "adapter-building-blocks" >}})** — Level shifting, buffering, galvanic isolation, protection components, and protocol conversion patterns.
- **[Grounding & Shielding Patterns]({{< relref "grounding-and-shielding" >}})** — Single-point grounding, shield termination, and chassis bonding strategies for legacy interfaces.
- **[Worked Mini-Patterns]({{< relref "worked-mini-patterns" >}})** — Conceptual interface topologies for MCU ↔ RS-232, 4–20 mA, relay/contact, and unbalanced audio.
- **[Bench Bring-Up Checklist]({{< relref "bench-bring-up-checklist" >}})** — Step-by-step procedure for first connection, power-up sequence, and functional test of a modern-to-legacy interface.
