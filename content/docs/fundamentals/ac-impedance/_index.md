---
title: "AC & Impedance"
weight: 31
bookCollapseSection: true
---

# AC & Impedance

The bridge from DC analysis to AC reality. Everything in the Fundamentals section up to this point works cleanly at DC — resistors have fixed resistance, Ohm's law is V = IR, and series/parallel combinations are straightforward arithmetic. But the moment signals change over time, frequency enters the picture and everything gets more interesting.

This section covers what happens when voltage and current aren't in lockstep anymore: reactance, complex impedance, frequency-dependent component behavior, and resonance. These ideas are prerequisites for filters, matching networks, transmission lines, and most of what happens in the Analog and RF sections.

## Pages

- **[Impedance & Reactance]({{< relref "impedance-and-reactance" >}})** — From R to Z: why resistance isn't enough when signals change, how capacitors and inductors create frequency-dependent opposition, and what complex impedance actually means at the bench.

- **[Frequency-Dependent Behavior]({{< relref "frequency-dependent-behavior" >}})** — How real resistors, capacitors, and inductors deviate from their ideal models as frequency changes — ESR, ESL, self-resonant frequency, and reading impedance curves.

- **[Resonance & Q Factor]({{< relref "resonance-and-q" >}})** — Where capacitive and inductive reactance cancel, what quality factor measures, and why every real circuit has resonances — some intentional, some not.
