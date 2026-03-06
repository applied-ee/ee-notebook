---
title: "Power Amplifier Evolution"
weight: 80
bookCollapseSection: true
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Power Amplifier Evolution

How audio power amplifiers got from "one transistor, lots of heat" to "switching at megahertz, almost no heat."

This section traces the design evolution of audio power amplifiers — not as a textbook taxonomy of classes, but as a chain of engineering problems and solutions. Each stage in the story solves the most pressing limitation of the previous design, and in doing so reveals a new problem that drives the next iteration.

The pattern is remarkably consistent:

1. **Start simple** — a single transistor amplifying a signal into a load.
2. **Hit a wall** — efficiency, distortion, stability, or some combination.
3. **Solve the immediate problem** — split the output stage, add bias, add feedback, switch to PWM.
4. **Discover what the solution broke** — crossover distortion, thermal runaway, switching noise.
5. **Repeat.**

This is how real circuit design works. Nobody sat down and invented the Class-AB amplifier from first principles. Engineers started with what they had, ran into limitations, and iterated. Understanding that progression builds better intuition than memorizing a table of amplifier classes.

## Pages

- **[Single-Transistor Class A]({{< relref "single-transistor-class-a" >}})** — Where it all starts: one transistor, full-time conduction, and a brutal efficiency problem.

- **[Class B Push-Pull]({{< relref "class-b-push-pull" >}})** — Split the job between two transistors. Efficiency jumps, but now there's a dead zone.

- **[Class AB Output Stage]({{< relref "class-ab-output-stage" >}})** — Bias the output transistors slightly on to kill crossover distortion — then wrestle with thermal stability.

- **[The Complete Linear Architecture]({{< relref "complete-linear-architecture" >}})** — Differential pair, voltage amplifier stage, driver, output: the multi-stage topology that dominates discrete analog amp design.

- **[The Fork]({{< relref "the-fork" >}})** — Two divergent paths: refine linear amplifiers to perfection, or abandon analog output entirely.

- **[Refining Linear Amplifiers]({{< relref "refining-linear-amplifiers" >}})** — Better bias, better feedback, better devices — pushing Class AB toward vanishing distortion.

- **[Class D Switching]({{< relref "class-d-switching" >}})** — Encode audio as pulse widths, switch the output stage, filter to recover the signal. Efficiency above 90%.

- **[Lessons and Canonical Structure]({{< relref "lessons-and-canonical-structure" >}})** — What the evolutionary arc teaches about amplifier design: recurring themes and the canonical linear topology.
