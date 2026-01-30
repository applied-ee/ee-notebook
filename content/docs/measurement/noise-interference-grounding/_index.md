---
title: "Noise, Interference & Grounding"
weight: 90
bookCollapseSection: true
---

# Noise, Interference & Grounding

Tracking down unwanted signals. EMI, ground loops, crosstalk, coupling. The measurements you make when the circuit "should work" but something invisible is ruining it.

## What This Section Covers

This section is about finding and fixing the invisible signals you didn't put there — the ones ruining your circuit.

Measurement intent — the questions you're trying to answer at the bench:

- **[Where is this noise coming from?]({{< relref "noise-source" >}})** — Identifying noise sources: switching converters, digital clocks, motor drives, external interference. Near-field probing, spectrum analysis, and process of elimination.
- **[Is there a ground loop?]({{< relref "ground-loop" >}})** — Hum, buzz, and ground-referenced interference. Identifying ground loops between instruments, between boards, or between a DUT and test equipment.
- **[Is this conducted or radiated?]({{< relref "conducted-radiated" >}})** — Distinguishing coupling paths. Does the noise travel on wires or through the air? Shielding experiments, common-mode vs. differential-mode, and ferrite tests.
- **[Does shielding / rerouting fix it?]({{< relref "shielding-rerouting" >}})** — Empirical fixes and verification. Testing whether a shield, filter, rerouted trace, or grounding change actually solves the problem — and measuring the before/after difference.
