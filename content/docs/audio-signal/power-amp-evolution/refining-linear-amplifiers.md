---
title: "Refining Linear Amplifiers"
weight: 60
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Refining Linear Amplifiers

Once the basic multi-stage Class AB architecture was established, decades of refinement pushed linear amplifier performance to extraordinary levels. The efficiency problem was accepted as fundamental; everything else was optimized.

## Better Input Stages

### Current Mirror Loading

Replacing the simple resistor load on the differential pair with a current mirror doubles the gain of the input stage and improves balance. The mirror forces equal collector currents in both halves, converting the full differential signal into a single-ended output without losing half the signal.

### Cascoding

Adding a cascode transistor above the differential pair (or above the VAS) increases the output impedance and gain of the stage dramatically. A cascode also reduces the Miller effect in the input transistors, improving high-frequency performance and reducing distortion at the input.

### Input JFET Devices

Some designs use JFETs for the input differential pair. JFETs have extremely high input impedance, very low input bias current, and — importantly — a square-law transfer characteristic rather than exponential. This produces lower distortion in the input stage itself, before feedback has a chance to correct anything.

## Better Voltage Amplifier Stages

### Cascoded VAS

A cascode on the VAS transistor raises the open-loop gain from 60-80dB to 100dB or more. Higher open-loop gain means more loop gain when feedback is applied, which means lower distortion.

### Current Source Loading

Active current source loads on the VAS (instead of a resistor) increase both gain and output swing. The VAS can swing closer to the supply rails, maximizing the signal range available to drive the output stage.

### Two-Stage VAS

Some high-performance designs cascade two gain stages (with appropriate compensation) to achieve extremely high open-loop gain. This requires careful stability analysis — more gain means more phase shift to manage.

## Better Output Stages

### MOSFET Outputs

Power MOSFETs have a positive temperature coefficient of R_DS(on) at high currents — as they heat up, their resistance increases, naturally limiting current. This makes them inherently resistant to thermal runaway, unlike BJTs.

MOSFETs also have very high input impedance (gate is insulated), so they don't load the driver stage. The tradeoff: MOSFETs have higher transfer nonlinearity than BJTs, producing more crossover distortion for a given bias level. Feedback compensates, but the open-loop linearity is worse.

### Triple Darlington / Sziklai Outputs

Adding a third transistor to the output cascade (before the driver and output transistor) increases the current gain enormously, reducing the drive requirements on the VAS. This makes higher-power designs feasible without compromising the VAS linearity.

### Output Triples

A related technique: the pre-driver, driver, and output transistor form a three-stage cascade where each transistor can be optimized for its specific role — the pre-driver for linearity, the driver for voltage swing, and the output device for current capacity and SOA (safe operating area).

## Better Feedback

### Error Correction

Some designs add a local error-correction loop around the output stage. This is distinct from the global feedback loop: it specifically targets output stage nonlinearity (especially crossover distortion) and corrects it before the signal reaches the load. The result is lower distortion than global feedback alone can achieve.

### Feedback Compensation Techniques

The simple Miller capacitor (dominant-pole compensation) works but limits the bandwidth and slew rate. More sophisticated compensation — two-pole compensation, input-stage compensation, transitional Miller compensation — allows higher loop gain at audio frequencies while maintaining stability.

## Where It Lands

A well-designed modern Class AB amplifier achieves:

- **THD+N < 0.001%** at rated power across the audio band
- **Frequency response flat within ±0.1dB** from 20Hz to 20kHz
- **Signal-to-noise ratio > 110dB** (A-weighted)
- **Output impedance < 0.1Ω** (high damping factor for speaker control)

These specifications exceed the resolving capability of human hearing by a comfortable margin. The amplifier, as a signal processing element in the audio chain, is essentially transparent.

The limitation remains efficiency. A 200W linear amplifier still needs a 400W power supply and substantial heatsinking. For applications where size, weight, and heat matter — car audio, portable systems, powered speakers, subwoofers — this limitation eventually pushed the industry toward switching amplifiers.
