---
title: "Is the Gain Structure Correct Through the Signal Chain?"
weight: 30
---

# Is the Gain Structure Correct Through the Signal Chain?

Level at each stage. Every multi-stage signal chain has a gain structure — the planned signal level at each node from input to output. When it's wrong, you get clipping in one stage and noise domination in another. Checking the gain structure means measuring the signal level at each point and confirming there's enough headroom above and enough distance from the noise floor below.

## What Gain Structure Means

**Concept:** Signal level, headroom, and noise floor at each stage

At every stage in the chain, three numbers matter:

| Parameter | What it is | Why it matters |
|-----------|-----------|----------------|
| Signal level | The RMS or peak voltage of the signal at this node | Must be above the noise floor for adequate SNR |
| Headroom | Distance from signal peak to the stage's clipping point | Must be positive to avoid clipping |
| Noise floor | Residual noise at this node (no signal) | Sets the minimum useful signal level |

The dynamic range at each stage = headroom + SNR. The overall chain's dynamic range is limited by the weakest stage.

### The Gain Structure Rule

- Each stage should receive a signal well above its noise floor
- Each stage should have enough headroom to handle peaks without clipping
- No stage should be "running hot" (near clipping) or "running cold" (signal barely above noise)

## Oscilloscope: Stage-by-Stage Level Check

**Tool:** Oscilloscope, 2 channels, DC-coupled
**When:** Tracing signal level through a multi-stage chain to find where clipping or noise problems originate

### Procedure

1. Apply a known test signal at the circuit input (sine wave at a standard level — e.g., line level for audio, or the expected sensor output level)
2. Measure the signal amplitude (Vpp or Vrms) at each stage's output:
   - Input connector / buffer output
   - Each gain stage output
   - Filter outputs
   - Final output stage
3. Record the actual gain of each stage: **Gain = V_out / V_in** (or in dB: 20 × log10(V_out / V_in))
4. Compare measured gain to design intent (schematic, datasheet)
5. At each stage, note the clipping point by increasing input until distortion appears

### Building the Level Diagram

A level diagram shows signal level at each node:

```
Input    →  Preamp  →  Filter  →  Power Amp  →  Output
-20 dBV     0 dBV     -3 dBV     +20 dBV       +20 dBV
            (+20 dB)  (-3 dB)    (+23 dB)
```

At each stage, note:
- Actual signal level
- Stage gain
- Maximum output before clipping
- Headroom = clip point − signal peak

### What You Learn

- Where in the chain the signal is too hot (clipping) or too cold (buried in noise)
- Whether each stage's gain matches the design
- The weakest link in the dynamic range chain

### Gotchas

- Use the same measurement bandwidth for all stages. Noise measurements at different bandwidths aren't comparable
- AC-coupled stages introduce low-frequency rolloff. If your test signal is near the coupling capacitor's cutoff frequency, the level will be reduced. Use a test frequency well within the passband
- Some stages have gain that depends on frequency (EQ, tone controls, filters). Measure at a frequency in the flat part of the response, or at a specified reference frequency

## DMM: Quick Level Check

**Tool:** DMM, V~ (AC Volts, RMS)
**When:** Quick stage-by-stage level check without setting up a scope

### Procedure

1. Apply a known input signal (sine wave at the reference frequency and level)
2. Set DMM to AC Volts (true RMS if available)
3. Measure the AC voltage at each stage's output
4. Calculate gain between stages

### What You Learn

- Signal level at each node as an RMS voltage
- Quick identification of stages with no output (open coupling cap, dead device) or unexpected gain

### Gotchas

- DMM bandwidth limits apply — most DMMs are accurate only to a few hundred kHz. For audio work this is fine; for RF or video signal chains, use a scope
- RMS reading on a clipped signal gives a misleadingly high value — the meter can't tell you the signal is clipping. Use a scope to check waveform shape
- True RMS meters give correct readings on any waveshape; average-responding meters are only accurate on sine waves. If the signal isn't sinusoidal, use a true RMS meter

## Finding Clipping Points

**Tool:** Oscilloscope + variable-level signal generator
**When:** Determining the maximum output swing (headroom) of each stage

### Procedure

1. Feed a sine wave into the stage input
2. Watch the output on the scope
3. Gradually increase the input amplitude until the output waveform begins to clip (flat tops/bottoms)
4. Record the output amplitude at the onset of clipping — this is the stage's maximum output swing
5. The headroom at the nominal operating level = clip point − nominal signal level

### Common Clipping Points

| Stage type | Typical clip point | Notes |
|-----------|-------------------|-------|
| Op-amp (±15V supply) | ±13V (1–2V from rail) | Rail-to-rail op-amps clip closer to the supply rail |
| Op-amp (single 5V supply) | ~0.2V to ~4.5V | Depends on rail-to-rail capability |
| Discrete transistor stage | Depends on bias point and supply | Clip levels are set by the design |
| ADC input | Full-scale voltage (e.g., 0–3.3V) | Clipping here means lost data — no recovery |
| DAC output | Full-scale voltage | Usually not the limiting stage |

### Gotchas

- In a multi-stage chain, the first stage to clip determines the chain's maximum level. If a preamp clips at 1Vpp output and the next stage has 10× gain, the power amp never sees more than 10Vpp — even though it could handle 30Vpp. The preamp is the bottleneck
- Some stages soft-clip (gradual compression) rather than hard-clip (flat top). Soft clipping introduces harmonic distortion before the signal visually clips on the scope. Use THD measurement to find the onset of distortion, not just visual clipping
- ADC clipping is particularly bad — once the signal exceeds the ADC's input range, the data is lost and can't be recovered by downstream processing. Always ensure headroom at the ADC input
