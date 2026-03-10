---
title: "The Complete Linear Architecture"
weight: 40
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---
{{< katex >}}{{< /katex >}}

# The Complete Linear Architecture

The Class AB output stage is a good power delivery mechanism — but it can't stand alone. It needs a front end to provide gain, reject noise, and enable feedback. The result is the canonical multi-stage linear amplifier: a topology so successful that nearly every discrete audio power amplifier built since the 1970s follows this pattern.

## The Architecture

Four stages, each with a distinct job:

### 1. Differential Input Stage

A long-tailed pair (two matched transistors with a shared tail current source) forms the input. One base receives the audio signal; the other receives a fraction of the output via the feedback network.

What it provides:
- **Common-mode rejection** — power supply noise, ground bounce, and other common-mode disturbances are rejected
- **Feedback summing point** — the difference between input and fed-back output becomes the error signal
- **High input impedance** — doesn't load the signal source

The differential pair is the reason the amplifier can use negative feedback effectively. Without it, you'd need a separate summing circuit.

### 2. Voltage Amplifier Stage (VAS)

A single transistor (often a common-emitter stage) takes the small differential output from the input pair and amplifies it to the full voltage swing needed to drive the output stage. This is where most of the open-loop voltage gain lives — typically 1,000 to 10,000× (60-80dB).

The VAS is usually compensated with a Miller capacitor (a small capacitor from collector to base) to ensure stability when the feedback loop is closed. This dominant-pole compensation deliberately limits the open-loop bandwidth to prevent oscillation.

### 3. Driver Stage

An intermediate buffer (often an emitter follower or a complementary pair) that provides current gain between the VAS and the output transistors. The output transistors may need 100mA or more of base drive at full power; the VAS can't supply this directly.

In some designs, the driver stage is integrated with the output stage as a Darlington or Sziklai (complementary feedback) pair.

### 4. Class AB Output Stage

The complementary push-pull output with bias network, emitter resistors, and thermal tracking — as covered in the previous page. This stage provides the current to drive the speaker but contributes minimal voltage gain.

## How It Works Together

The elegance is in the feedback loop:

1. Audio input arrives at one leg of the differential pair
2. The output is sampled by a resistive divider and fed back to the other leg
3. The differential pair subtracts: error = input - (output × feedback fraction)
4. The VAS amplifies the error signal
5. The driver and output stage deliver current to the load
6. The feedback loop forces the output to track the input

The closed-loop gain is set almost entirely by the feedback resistors:

$$A_{CL} = \frac{R_f + R_g}{R_g}$$

where R_f is the feedback resistor from output to inverting input, and R_g is the resistor from inverting input to ground. This is independent of transistor parameters, temperature, and supply voltage — as long as the open-loop gain is much larger than the closed-loop gain.

## What It Solves

**Stable, predictable gain.** The open-loop gain is high and sloppy; the closed-loop gain is precise and stable. This is the fundamental bargain of negative feedback.

**Very low distortion.** All sources of distortion within the loop — transistor nonlinearity, crossover artifacts, VAS clipping — are reduced by the loop gain. A well-designed amplifier achieves THD below 0.01% at rated power.

**Scalable power.** The architecture separates voltage gain (VAS) from current delivery (output stage). You can scale the output stage — bigger transistors, more parallel devices, higher supply rails — without changing the front end.

**Robust design.** Temperature changes, component aging, and transistor variations are all absorbed by the feedback loop. The amplifier performs consistently over a wide range of conditions.

## The Remaining Limitations

This architecture has dominated for half a century because it works extremely well. But it has inherent limitations that no amount of refinement fully eliminates:

- **Efficiency ceiling.** Class AB is more efficient than Class A, but still dissipates significant power as heat. At full power, practical efficiency is 50-65%. The rest is heat. A 100W amplifier needs a power supply capable of delivering 150-200W, and a heatsink rated for 50-100W of continuous dissipation.

- **Size and weight.** Large heatsinks, heavy power transformers, and substantial power supply capacitors make high-power linear amplifiers bulky and heavy.

- **Power supply demands.** The amplifier needs clean, well-regulated supply rails. Ripple on the supply appears as hum in the output (attenuated by the power supply rejection ratio, but not eliminated).

These aren't design flaws — they're fundamental consequences of using transistors as continuously variable current sources. The output devices are always in their linear region, always dissipating the difference between the supply voltage and the output voltage as heat.

This sets up a fork in the road: you can keep refining the linear approach, or you can ask a radical question — what if the output transistors weren't in their linear region at all?
