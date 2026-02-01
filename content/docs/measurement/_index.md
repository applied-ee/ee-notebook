---
title: "📏 Measurement & Test"
weight: 8
bookCollapseSection: true
---

# Measurement & Test

Organized by what you're trying to measure, not by which instrument you pick up. Each subsection starts with a measurement situation — safety, probing, power rails, signals, noise — and works through the practical questions you face at the bench.

Subsections are ordered prerequisites-first: safety and probing technique before anything else, simple checks (continuity, components) before complex ones (signals, spectrum, protocols), and cross-cutting skills (noise, calibration) at the end.

## Sections

- **[Safety & High Energy]({{< relref "safety-high-energy" >}})** — Mains, high voltage, CAT ratings, and the hazard assessment that comes before every measurement.
  *What this section covers:* Can I Probe Safely? · Voltage & Energy Domain · Isolation & Differential · Instrument Ratings

- **[Probing & Measurement Technique]({{< relref "probing-technique" >}})** — Connecting without changing what you're measuring: loading, bandwidth, and grounding.
  *What this section covers:* Circuit Loading · Probe Bandwidth · Ground Connection · Probe Selection

- **[Continuity & Connections]({{< relref "continuity-connections" >}})** — The simplest checks: joints, shorts, wire mapping, and intermittent faults.
  *What this section covers:* Joint & Contact · Short Between Nets · Pin & Wire Mapping · Intermittent Connection

- **[Component Testing]({{< relref "component-testing" >}})** — Go/no-go checks, actual values, tolerance, and characterizing ESR, leakage, and gain.
  *What this section covers:* Dead or Alive? · Actual Value · Within Tolerance? · ESR, Leakage & Gain

- **[Power Rails & Supplies]({{< relref "power-rails-supplies" >}})** — Supply verification, regulation, ripple, current draw, sequencing, and energy storage health.
  *What this section covers:* Voltage Present? · Voltage Correct Under Load? · Ripple & Noise · Current Draw · Power Sequencing · Bulk Cap & Battery

- **[Signals & Waveforms]({{< relref "signals-waveforms" >}})** — Following signals through the circuit: presence, shape, gain, distortion, and finding where they go wrong.
  *What this section covers:* Signal Present? · Expected Waveform? · Signal Tracing · Distortion & Clipping

- **[Time, Frequency & Spectrum]({{< relref "time-frequency-spectrum" >}})** — Signal characterization in both domains: rise times, jitter, harmonics, bandwidth, and frequency content.
  *What this section covers:* Clock Frequency · Rise & Fall Time · Harmonics · Actual Bandwidth · Jitter

- **[Digital Logic & Protocols]({{< relref "digital-logic-protocols" >}})** — Logic levels, bus health, protocol decode, clock-data alignment, and speed failures.
  *What this section covers:* Logic Levels · Bus Activity · Device Responding? · Clock/Data Alignment · Speed Failures

- **[Audio & Analog Circuits]({{< relref "audio-analog" >}})** — THD, SNR, frequency response, gain structure, and sensor interfaces where the signal is the product.
  *What this section covers:* THD & Noise Floor · Frequency Response · Gain Structure · Sensor Output

- **[Noise, Interference & Grounding]({{< relref "noise-interference-grounding" >}})** — Tracking unwanted signals: EMI, ground loops, crosstalk, coupling, and empirical fixes.
  *What this section covers:* Noise Source · Ground Loop · Conducted & Radiated · Shielding & Rerouting

- **[Calibration & Measurement Confidence]({{< relref "calibration-confidence" >}})** — Trusting your readings: accuracy, artifacts, calibration status, and measurement uncertainty.
  *What this section covers:* Instrument Accuracy · Measurement Artifact · Calibration & Compensation · Measurement Uncertainty

- **[Test Instruments]({{< relref "test-instruments" >}})** — Reference pages for the instruments on the bench: specs, usage notes, and limits.
  *What this section covers:* DMM (Triplett MM520) · Oscilloscope (Owon SDS7102V) · ESR Meter (Peak Atlas ESR70)
