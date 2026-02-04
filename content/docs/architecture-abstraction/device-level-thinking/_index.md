---
title: "Device-Level Thinking"
weight: 50
bookCollapseSection: true
---

# Device-Level Thinking

## What This Section Covers

- **[What Defines a Device Boundary]({{< relref "what-defines-a-device-boundary" >}})** — How to identify where one device ends and another begins, and why the boundary matters for reasoning and debugging.

- **[Cross-Subsystem Interactions]({{< relref "cross-subsystem-interactions" >}})** — How subsystems within a device affect each other through shared power, ground, thermal paths, and timing — even when the schematic says they're independent.

- **[Sequencing, Bring-Up, and Reset Domains]({{< relref "sequencing-bring-up-and-reset-domains" >}})** — The temporal structure of a device: power sequencing, reset dependencies, and the assumptions that must hold before each subsystem can operate.

- **["Works Alone" vs "Works Integrated"]({{< relref "works-alone-vs-works-integrated" >}})** — Why a subsystem that passes every bench test can still fail when combined with the rest of the device, and what to look for at integration time.
