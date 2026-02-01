---
title: "EE Notebook"
type: docs
---

# EE Notebook

A working reference for electronics and electrical engineering — concepts, procedures, gotchas, and patterns organized for practical use. The goal is a single resource that covers the full landscape from fundamentals through design and repair, organized so that any topic can scale from a concise overview into the depth that professional engineering demands. The emphasis is on selecting the right components and patterns, understanding why circuits behave the way they do, and building the judgment to design and fix real hardware.

## How It's Organized

The sections follow a rough layering. Nature's constraints come first. Human-engineered behavior is built on top. The design process turns ideas into hardware. Practice — measurement and debugging — runs through everything.

**The physics layer** — rules you can't negotiate with:

- **[Fundamentals]({{< relref "/docs/fundamentals" >}})** — Ohm's law, Kirchhoff's laws, power and energy, passive component behavior, circuit analysis, and the unit system. The universal constraints that every circuit obeys whether you remember them or not.

**The engineering layer** — designed behavior built on top of those constraints:

- **[Analog Electronics]({{< relref "/docs/analog" >}})** — Transistors, op-amps, amplifiers, filters, regulators. Continuous-signal design where everything is a tradeoff between gain, bandwidth, noise, and stability.
- **[Digital Electronics]({{< relref "/docs/digital" >}})** — Logic gates, flip-flops, timing, buses, state machines. Discrete abstraction layered over analog reality — until edges get fast enough that the analog reality shows through.
- **[Embedded Systems]({{< relref "/docs/embedded" >}})** — MCU and MPU architecture, single-board computers, peripherals, firmware, toolchains. Where hardware meets software and timing constraints define what's possible.
- **[Audio & Signal Processing]({{< relref "/docs/audio-signal" >}})** — Filters, FFT, DACs, ADCs, sampling theory. The information layer — capturing, transforming, and reproducing signals.
- **[Radio & RF]({{< relref "/docs/radio-rf" >}})** — Antennas, impedance matching, transmission lines, propagation. Where wavelength matters and low-frequency intuition breaks down.

**The design process layer** — turning ideas into working hardware:

- **[Design & Development]({{< relref "/docs/design-development" >}})** — From requirements through schematic, layout, prototyping, and validation. The workflow and discipline of turning a concept into working hardware.

**The practice layer** — verifying and fixing the things you build:

- **[Measurement & Test]({{< relref "/docs/measurement" >}})** — Organized by what you're trying to measure, not which instrument you pick up. How you find out what's actually happening.
- **[Debugging, Failure & Repair]({{< relref "/docs/debugging" >}})** — Troubleshooting methods, failure modes, rework techniques. What you do when reality disagrees with your model.
