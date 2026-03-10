---
title: "Class B Push-Pull"
weight: 20
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---
{{< katex >}}{{< /katex >}}

# Class B Push-Pull

The efficiency fix for Class A: split the output stage into two transistors, each handling half the signal. One conducts on positive swings, the other on negative swings. Neither conducts at idle. The efficiency jumps dramatically — but a new kind of distortion appears.

## The Circuit

A complementary pair — one NPN and one PNP — configured as emitter followers. Both emitters connect to the load (speaker). The NPN handles the positive half-cycle: when the input goes positive, the NPN turns on and sources current into the load. The PNP handles the negative half-cycle: when the input goes negative, the PNP turns on and sinks current from the load.

At zero crossing — when the signal passes through zero volts — neither transistor is conducting. This is the defining feature of Class B: each device conducts for exactly 180° of the signal cycle, and the conduction angle is exactly half.

## What It Solves

**Efficiency.** The theoretical maximum efficiency of Class B push-pull is approximately **78.5%** (π/4):

$$\eta_{max} = \frac{\pi}{4} \approx 78.5\%$$

Compare this to 25% for Class A with resistive load. The improvement is dramatic because transistors that aren't conducting aren't dissipating power. At idle, with no signal, the amplifier draws essentially zero current. Power dissipation scales with signal level rather than being constant.

**Idle dissipation.** In Class A, the transistor burns maximum power at idle. In Class B, idle power is near zero. This means smaller heatsinks, smaller power supplies, and less thermal stress on components during quiet passages.

**Scalability.** The improved efficiency makes higher power outputs practical. A 50W Class B amplifier dissipates far less heat than a 50W Class A amplifier, making it physically realizable without exotic cooling.

## The New Problem: Crossover Distortion

Both transistors are silicon BJTs. Each requires about 0.6-0.7V of base-emitter voltage (V_BE) before it begins conducting. This means there's a dead zone around zero crossing: for input signals between roughly -0.7V and +0.7V, *neither* transistor is on. The output is zero while the input is non-zero.

This dead zone produces **crossover distortion** — a kink in the output waveform at every zero crossing. The output waveform has a flat spot or notch where the signal passes through zero.

Crossover distortion is particularly nasty:

- **It's worst at low signal levels.** When the signal amplitude is small (quiet music), the dead zone is a large fraction of the total swing. At very low levels, the signal may never exceed the V_BE threshold, producing no output at all.
- **It's high-order harmonics.** The sharp kink at zero crossing generates odd harmonics (3rd, 5th, 7th...) that are musically unpleasant — harsh, gritty, buzzy.
- **It's signal-dependent.** Unlike the gentle second-harmonic distortion of Class A, crossover distortion changes character with signal level, making it harder to mask or tolerate.

For an amplifier intended to reproduce audio faithfully, crossover distortion is unacceptable. A listener can hear it clearly, especially at low volumes.

## Why Not Just Add More Gain?

A common first instinct: wrap the output stage in a feedback loop with enough gain to linearize it. And feedback does help — but it can't fully eliminate the crossover artifact. The dead zone is a discontinuity, and no amount of linear feedback perfectly corrects a discontinuity. The loop gain would need to be infinite at the crossover point, which isn't achievable.

Feedback reduces crossover distortion but doesn't eliminate it. Something more is needed.

## The Engineering Pressure

The solution is conceptually simple: don't let both transistors turn completely off. If each output device is biased slightly into conduction — just enough to eliminate the dead zone — the crossover distortion disappears while preserving most of the efficiency advantage.

This is Class AB: the pragmatic compromise between Class A linearity and Class B efficiency.
