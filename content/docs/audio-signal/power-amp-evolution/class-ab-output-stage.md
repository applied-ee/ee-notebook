---
title: "Class AB Output Stage"
weight: 30
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Class AB Output Stage

The fix for crossover distortion: bias the push-pull output transistors slightly into conduction so the dead zone disappears. This turns out to work extremely well — but introduces a new set of problems related to bias stability and thermal behavior.

## The Circuit

Same complementary push-pull topology as Class B, but with a bias network inserted between the bases of the NPN and PNP output transistors. This network holds the bases apart by approximately 2 × V_BE (about 1.2-1.4V), ensuring both devices are slightly on at idle.

Common bias methods:

- **Diode bias:** Two silicon diodes in series between the bases, thermally coupled to the output transistors. Each diode drops ~0.65V, providing ~1.3V of bias spread.
- **V_BE multiplier (rubber diode):** A transistor with a resistive voltage divider between collector and emitter, configured to produce an adjustable voltage drop. More flexible than diodes — the bias voltage can be trimmed precisely.

The quiescent current through the output devices is typically 10-50mA — enough to keep both transistors in their active region through the crossover point, but far less than the full signal current.

## What It Solves

**Crossover distortion is gone.** With both transistors conducting at idle, there's no dead zone at zero crossing. The output waveform transitions smoothly from NPN conduction to PNP conduction without the kink. Small signals are amplified cleanly because both devices are active.

**Efficiency stays high.** The quiescent current is small — milliamps, not amps. At full power, the efficiency is nearly identical to Class B (approaching 78.5% theoretical). At idle, power dissipation is modest: P_idle = V_CC × I_Q, where I_Q is the small bias current.

In practice, Class AB amplifiers achieve 50-65% efficiency at full power, which is a massive improvement over Class A.

## The New Problems

### Bias Stability

The bias voltage must be precise. Too little bias and crossover distortion returns. Too much and the quiescent current rises, wasting power and heating the output transistors.

The problem: V_BE has a negative temperature coefficient of about -2mV/°C. As the output transistors warm up (which they do under signal), their V_BE drops. If the bias voltage stays constant, the quiescent current increases. Higher current means more heating, which means lower V_BE, which means even more current.

### Thermal Runaway

This is the positive feedback loop that can destroy output transistors:

1. Signal drives output transistors → they heat up
2. V_BE drops with temperature → quiescent current increases
3. Higher current → more heating
4. More heating → even lower V_BE → even more current
5. Transistor exceeds its safe operating area → failure

Thermal runaway is not a theoretical concern. It destroys amplifiers. It's the primary failure mode of poorly designed Class AB output stages.

## Engineering Solutions

These problems are all solvable — and the solutions are well established.

### Emitter Resistors (R_E)

Small resistors (0.1-0.47Ω) in series with each output transistor's emitter. These provide local negative feedback: as current increases, the voltage drop across R_E increases, reducing V_BE and opposing further current increase.

Emitter resistors are the single most important protection against thermal runaway. They also help balance current sharing when multiple output devices are paralleled for higher power.

The tradeoff: R_E wastes a small amount of power and slightly reduces the maximum output swing. Typical values are chosen so the voltage drop at full current is 50-200mV — a minor sacrifice.

### Temperature-Tracking Bias

The V_BE multiplier transistor is thermally coupled to the output heatsink (bolted to it, or mounted on the same metal surface). As the output transistors heat up, the bias transistor heats up too, and its V_BE drops by the same -2mV/°C. This reduces the bias voltage, which reduces the quiescent current — exactly the correction needed.

This thermal tracking turns the positive feedback loop into a stable system. It's elegant: the same physical effect that causes the problem (V_BE temperature dependence) is used to solve it.

### Negative Feedback

Wrapping the entire amplifier in a global negative feedback loop reduces distortion from all sources — including any residual crossover artifacts, transistor nonlinearity, and bias imperfections. Typical feedback factors of 20-40dB reduce distortion by one to two orders of magnitude.

Feedback also stabilizes the gain against component variations, supply voltage changes, and temperature drift. It's the single most powerful tool in the linear amplifier designer's kit.

## Where This Stands

Class AB with proper biasing, emitter degeneration, and thermal tracking is a solved problem. The output stage works. But it's just the output stage — it still needs:

- A clean signal to drive it (high impedance, well-controlled voltage swing)
- Stable gain (not dependent on transistor β or temperature)
- Very low distortion (beyond what the output stage alone provides)

These requirements point toward a multi-stage architecture with a differential input, a voltage amplifier stage, and global feedback. That's the complete linear amplifier — the next step in the evolution.
