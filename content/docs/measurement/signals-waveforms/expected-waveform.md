---
title: "Does It Match the Expected Waveform?"
weight: 20
---

# Does It Match the Expected Waveform?

You've confirmed the signal is present — now does it look right? Comparing the actual waveform to what the datasheet, schematic, or simulation predicts. The differences between "expected" and "actual" are where the debugging information lives.

## Oscilloscope: Waveform Comparison

**Tool:** Oscilloscope, DC-coupled, 10x probe
**When:** Comparing an observed signal to an expected shape, amplitude, frequency, or DC offset

### What to Check

| Parameter | How to check | What you're comparing to |
|-----------|-------------|------------------------|
| Amplitude (peak-to-peak) | Cursor measurement or auto-measure | Datasheet spec, schematic annotation, simulation |
| DC offset | DC-coupled measurement, read DC level from cursor or measure function | Expected bias point (e.g., Vcc/2 for AC-coupled amplifier stages) |
| Frequency / period | Period measurement or frequency counter | Clock spec, oscillator datasheet, RC time constant calculation |
| Waveshape | Visual comparison | Sine, square, triangle, sawtooth, pulse — does it match the expected type? |
| Duty cycle | Pulse-width measurement or duty cycle auto-measure | 50% for symmetric clocks, specific ratio for PWM |
| Symmetry | Compare positive and negative half-cycles | Push-pull stages should be symmetric; asymmetry suggests bias or matching problems |

### Procedure

1. Set up the scope to display the signal cleanly — stable trigger, appropriate vertical and horizontal scale
2. Use the scope's automatic measurements (Vpp, frequency, duty cycle, rise time) to get numbers
3. Compare each parameter to the expected value from the schematic, datasheet, or simulation
4. Note discrepancies — these are your clues

### What You Learn

- Whether the circuit is behaving as designed
- Which specific parameter is wrong — amplitude too low suggests gain issue; frequency off suggests timing/component value issue; wrong waveshape suggests the circuit topology isn't doing what you expect

### Gotchas

- Auto-measurements can be misleading on noisy or complex signals. The scope may measure the wrong edges, trigger on noise, or include ringing in the amplitude measurement. Always sanity-check auto-measurements against what you see visually
- Make sure probe compensation is correct — an uncompensated probe changes the apparent waveshape (drooping or overshooting square wave edges), which you might mistake for a circuit problem
- DC offset measurement requires DC coupling. If you're AC-coupled, you lose the offset information entirely

## Reference Waveform Comparison

**Tool:** Oscilloscope with reference waveform storage, or a second known-good unit
**When:** You have a working unit and a failing unit, or a saved "golden" waveform

### Procedure

1. Capture the waveform from the known-good circuit and save it as a reference trace (most scopes have REF waveform storage)
2. Probe the same node on the suspect circuit
3. Display both on screen — the live trace and the reference
4. Look for differences: amplitude, timing, waveshape, noise

### What You Learn

- Exactly how the suspect unit differs from a known-good one
- Subtle differences that are hard to spot without a direct comparison — slight frequency shift, marginally reduced amplitude, different ringing pattern

### Gotchas

- Make sure test conditions are identical: same input signal, same load, same supply voltage, same temperature
- Component tolerances mean that two "good" units won't be identical. Know the expected variation before declaring a difference a fault
- Reference waveforms captured at different scope settings (different sample rate, bandwidth limit, etc.) aren't directly comparable

## Comparing to Simulation

**Tool:** Oscilloscope + SPICE simulation output
**When:** The circuit has no known-good reference, and you need to compare to design intent

### Procedure

1. Simulate the circuit in SPICE (LTspice, ngspice, etc.) with realistic component values
2. Note the expected waveform parameters: amplitude, frequency, DC bias, waveshape
3. Measure the actual circuit with the scope
4. Compare — differences indicate either a circuit fault or a simulation that doesn't match reality

### What You Learn

- Whether the physical circuit matches the design intent
- Where the simulation's assumptions diverge from reality (ideal vs. real components, parasitic effects, layout issues)

### Gotchas

- Simulations use ideal models unless you explicitly include parasitics. The real circuit has stray capacitance, trace inductance, ESR, and leakage that the simulation doesn't know about
- Component values in the simulation are nominal. Real components are within tolerance — a 10% difference between sim and measurement may just be component variation, not a fault
- Simulations don't include probe loading. If the probe changes the circuit behavior (high-Z nodes, oscillators), the measurement won't match the simulation even if the circuit is perfect
