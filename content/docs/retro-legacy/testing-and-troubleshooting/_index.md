---
title: "Testing & Troubleshooting"
weight: 65
bookCollapseSection: true
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Testing & Troubleshooting

Finding the fault in a legacy board is a different discipline than fixing it. [Aging, Drift, and Failure Modes]({{< relref "../aging-drift-and-failure-modes" >}}) covers what goes wrong over time; [Repair, Substitution, and Reverse Engineering]({{< relref "../repair-substitution-and-reverse-engineering" >}}) covers executing the fix. This page sits between them: the diagnostic process of isolating what has actually failed on a specific board.

The scope here is solid-state equipment from roughly the 1960s onward — transistorized and IC-based designs. Tube and valve troubleshooting is a different discipline with different safety concerns (high voltage, high impedance, different failure signatures) and is out of scope. For general debugging methodology that applies to any era, see [Debugging, Failure & Repair]({{< relref "/docs/debugging" >}}).

What makes legacy troubleshooting distinct: no JTAG or firmware debug port, limited or nonexistent test points, often no schematic, components that have drifted from spec over decades, and replacement parts that may no longer exist. The diagnostic approach must account for all of this.

- **[Safe Power-Up Procedures]({{< relref "safe-power-up-procedures" >}})** — Visual inspection, variac bring-up, current-limited supply, and dim bulb tester techniques for powering unknown or dormant boards without causing further damage.
- **[Diagnostic Workflow Without Documentation]({{< relref "diagnostic-workflow-without-documentation" >}})** — Identifying power rails, locating active devices, tracing signal flow, and comparing working vs. failing behavior when no schematic exists.
- **[Signal Tracing]({{< relref "signal-tracing" >}})** — Audio-frequency and RF signal injection, and the half-split strategy for isolating the failing stage in a signal chain.
- **[Thermal & Intermittent Faults]({{< relref "thermal-and-intermittent-faults" >}})** — Cold spray, heat gun, tap test, flex test, and monitoring techniques for faults that appear or disappear with temperature or mechanical stress.
- **[In-Circuit vs. Out-of-Circuit Testing]({{< relref "in-circuit-vs-out-of-circuit-testing" >}})** — When in-circuit measurements are reliable (ESR, diode test, DC voltage), when they mislead (resistance, capacitance), and when to lift a lead.
- **[Common Fault Signatures by Technology]({{< relref "common-fault-signatures" >}})** — Characteristic failure modes of discrete transistor boards, TTL/CMOS logic, linear ICs, and mixed-signal systems.
