---
title: "Single-Transistor Class A"
weight: 10
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---
{{< katex >}}{{< /katex >}}

# Single-Transistor Class A

The simplest possible audio power amplifier: one transistor conducting all the time, linearly reproducing the input signal. This is where the story starts — and where the efficiency problem first becomes obvious.

## The Circuit

A single NPN BJT in common-emitter configuration, biased at a quiescent point roughly halfway between the supply rails. The load (speaker or resistor) sits in the collector circuit. An input signal swings the base voltage, modulating the collector current and producing an amplified version of the input across the load.

The key feature: the transistor is always conducting. It never turns off during any part of the signal cycle. This is the defining characteristic of Class A operation.

## What Works

**Linearity.** Because the transistor operates in its active region throughout the entire signal swing, the output is a reasonably faithful reproduction of the input. There's no point in the cycle where the device switches off and back on — so there's no crossover distortion, no switching transient, no dead zone. The transfer function is as smooth as the transistor's own characteristics allow.

**Simplicity.** One active device, a bias network, a coupling capacitor, a load. This is the amplifier you build in the first week of an electronics course. There's very little to go wrong from a topology standpoint.

**Low distortion at small signals.** When the signal swing is small relative to the bias point, the transistor operates over a nearly linear segment of its transfer curve. Distortion is primarily second-harmonic (the gentle curvature of the I_C vs V_BE characteristic), which sounds relatively benign in audio applications.

## The Efficiency Problem

Here's where Class A falls apart for power applications.

The transistor is always on, always conducting the quiescent current I_Q. At idle — no signal — the amplifier is burning power:

$$P_{idle} = V_{CC} \times I_Q$$

That's pure waste. The transistor is dissipating energy as heat, doing no useful work. And it gets worse: the maximum theoretical efficiency of a Class A stage with a resistive load is only about **25%**. With an inductive (transformer-coupled) load, you can push this to about **50%** in theory — but real-world circuits with resistive collector loads rarely exceed 20-25%.

What this means in practice: to deliver 10W to a speaker, the amplifier might be dissipating 30-40W as heat. That's a big heatsink. That's a beefy power supply. And the transistor is under thermal stress *all the time*, not just during loud passages.

The efficiency expression for a Class A stage with resistive load:

$$\eta_{max} = \frac{P_{load(max)}}{P_{supply}} = \frac{V_{CC}^2 / 8R_L}{V_{CC}^2 / 2R_L} = 25\%$$

This isn't a problem you can engineer around within the Class A topology. It's fundamental: if the transistor must conduct continuously, it must dissipate power continuously.

## Why It Matters

For small-signal applications — preamplifiers, voltage gain stages, headphone amps driving efficient loads — Class A is perfectly fine. The distortion is low, the circuit is simple, and the wasted power is measured in milliwatts.

But for power amplification — driving loudspeakers at useful volumes — the efficiency problem is disqualifying. You can't scale Class A to 50W or 100W output without absurd heatsinks and power supplies.

## The Engineering Pressure

The question becomes obvious: can we build an amplifier where the output transistor isn't wasting power during the parts of the signal cycle when it isn't needed?

If the signal is positive, maybe only one transistor needs to conduct. If the signal is negative, a different transistor handles it. Each device works only half the time, and neither is burning power while idle.

This is the idea that leads to push-pull and Class B.
