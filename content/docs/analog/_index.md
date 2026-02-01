---
title: "🎛️ Analog Electronics"
weight: 2
bookCollapseSection: true
---

# Analog Electronics

Human-engineered behavior built on continuous signals.

Analog electronics is about shaping voltages and currents in a continuous domain — working with the grain of physics rather than forcing it into discrete states. This is where transistors, op-amps, diodes, amplifiers, filters, and voltage regulators come together to produce useful behavior.

Every analog design is a balance of competing tradeoffs: gain versus bandwidth, noise versus impedance, stability versus responsiveness, simplicity versus performance. There are no perfect solutions — only acceptable compromises.

Entries here range from single-component behavior to complete circuit topologies, with an emphasis on intuition, biasing, and practical design over abstract theory. The [Fundamentals]({{< relref "/docs/fundamentals" >}}) section defines the underlying requirements; Analog Electronics explores how real circuits are built on top of them.

## Sections

- **[Active Devices]({{< relref "active-devices" >}})** — Diodes, BJTs, and MOSFETs: how analog behavior is created from semiconductor junctions.
  *What this section covers:* Diodes · BJTs · MOSFETs

- **[Amplifiers & Gain Stages]({{< relref "amplifiers-gain-stages" >}})** — Single-transistor stages, op-amps, and multistage designs: turning small signals into usable ones.
  *What this section covers:* Single-Transistor Amplifiers · Op-Amps · Multistage Amplifiers

- **[Biasing & Operating Points]({{< relref "biasing-operating-points" >}})** — DC biasing, operating regions, and temperature drift: making circuits behave predictably.
  *What this section covers:* DC Biasing · Operating Regions · Temperature & Drift

- **[Filters & Frequency Behavior]({{< relref "filters-frequency-behavior" >}})** — RC/RL filters, active filters, and frequency response: shaping signals in time and frequency.
  *What this section covers:* RC & RL Filters · Active Filters · Frequency Response

- **[Power & Regulation]({{< relref "power-and-regulation" >}})** — Linear regulators, switching regulators, references, and decoupling: supplying clean, stable energy.
  *What this section covers:* Linear Regulators · Switching Regulators · Reference Voltages · Decoupling & Bypassing

- **[Noise, Stability & Reality]({{< relref "noise-stability-reality" >}})** — Noise sources, oscillation, and layout parasitics: where designs succeed or fail.
  *What this section covers:* Noise Sources · Feedback & Loop Intuition · Stability & Oscillation · Layout & Parasitics
