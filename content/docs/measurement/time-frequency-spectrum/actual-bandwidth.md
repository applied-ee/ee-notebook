---
title: "What's the Actual Bandwidth Here?"
weight: 40
---

# What's the Actual Bandwidth Here?

Measuring the real bandwidth of amplifiers, filters, and signal paths — not what the datasheet claims, but what the built circuit actually delivers. Bandwidth changes with component tolerances, parasitic capacitance, loading, and layout. The datasheet says 10 MHz; the board says maybe.

## Oscilloscope + Signal Generator: Swept Frequency Response

**Tool:** Function generator (with frequency sweep or manual stepping) + oscilloscope
**When:** Measuring the -3 dB bandwidth of an amplifier, filter, or signal path

### Procedure

1. Connect the function generator to the circuit input
2. Connect scope CH1 to the input, CH2 to the output
3. Set the generator to a sine wave at a low frequency well within the expected passband
4. Measure the output amplitude (Vpp or Vrms) — this is your reference level (0 dB)
5. Increase the frequency in steps (1x, 2x, 5x, 10x sequence works well: 100 Hz, 200 Hz, 500 Hz, 1 kHz, 2 kHz, 5 kHz...)
6. At each step, record the output amplitude
7. Continue until the output has dropped well below the reference (at least -10 dB past the -3 dB point)
8. The -3 dB frequency is where the output amplitude drops to 70.7% of the reference (or equivalently, -3 dB down)

### Plotting the Response

Plot amplitude (in dB) vs. frequency (on a log scale):

**Gain_dB = 20 × log10(V_out / V_out_ref)**

| Gain relative to passband | dB |
|---------------------------|-----|
| 100% (passband) | 0 dB |
| 70.7% (-3 dB point) | -3 dB |
| 50% | -6 dB |
| 10% | -20 dB |
| 1% | -40 dB |

### What You Learn

- The actual -3 dB bandwidth of the circuit as built
- The rolloff slope (first-order = -20 dB/decade, second-order = -40 dB/decade, etc.)
- Whether there are peaks or dips in the passband (indicating resonance or impedance mismatch)

### Gotchas

- Keep the input amplitude constant across the sweep. Some function generators' amplitude varies with frequency. Monitor CH1 (input) and adjust if needed, or use the ratio (CH2/CH1) rather than absolute CH2 amplitude
- Don't overdrive the circuit at low frequencies and then wonder why it clips. Set the input amplitude so the circuit operates in its linear range at all frequencies
- At high frequencies, cable capacitance and probe loading affect the measurement. What you measure as "circuit bandwidth" may actually include the measurement system's rolloff. Use short cables and calibrated probes
- Source impedance matters. The generator's output impedance (typically 50 Ohm) forms a divider with the circuit's input impedance. If the circuit expects a high-impedance source, the generator may load it differently than the actual source

## Oscilloscope: Step Response Method

**Tool:** Function generator (square wave) + oscilloscope
**When:** Quick bandwidth estimate from a single measurement — no frequency sweep needed

### How It Works

A square wave's sharp edge contains all frequencies. The circuit's bandwidth determines how much of that edge it can pass. The output's rise time is directly related to bandwidth:

**BW ≈ 0.35 / t_rise**

### Procedure

1. Apply a square wave at a frequency low enough that the output fully settles between edges (period >> 5 × rise time)
2. Measure the output's 10%-90% rise time
3. Calculate: **BW ≈ 0.35 / t_rise_output**
4. If the input edge isn't infinitely fast, subtract the input's contribution:

**t_circuit = sqrt(t_out² - t_in²)**

**BW ≈ 0.35 / t_circuit**

### What You Learn

- Bandwidth from a single measurement — faster than a full frequency sweep
- Approximate but useful for quick checks and first-order verification

### Gotchas

- This assumes a single-pole (first-order) rolloff. Higher-order systems have a different relationship between rise time and bandwidth — the 0.35 constant changes (0.34–0.45 depending on the filter type and order)
- The input square wave must be fast enough that it's not the bottleneck. If the generator's edge is comparable to the circuit's rise time, you're measuring the combined response
- Overshoot on the step response indicates peaking in the frequency response — the system isn't a clean first-order rolloff. In this case, the 0.35/t_rise estimate is approximate

## Comparing to Datasheet

### Common Reasons Measured BW < Datasheet BW

| Reason | How much bandwidth loss | How to verify |
|--------|------------------------|---------------|
| Parasitic capacitance (PCB, socket, wiring) | 10–50% | Calculate expected rolloff from parasitics |
| Output loading (scope probe, cable, downstream circuit) | Varies — can be significant at high Z | Remove load and re-measure |
| Component tolerance (feedback resistors, filter caps) | ±10–20% on BW | Measure actual component values |
| Power supply inadequacy | Can be severe (slew rate limited) | Check supply for droop under dynamic load |
| Layout issues (long traces, no ground plane) | Highly variable | Compare to reference design layout |

### Common Reasons Measured BW > Expected

| Reason | Concern |
|--------|---------|
| Peaking before rolloff | Marginal stability — the system is close to oscillating. There may be a resonant peak that extends the apparent -3 dB frequency |
| Missing filter component | A filter stage that should limit bandwidth isn't working (open cap, wrong value) |
| Lower-than-expected parasitic capacitance | Good news — the layout is better than expected |

### Gotchas

- Datasheets specify bandwidth under specific conditions (gain setting, load, supply voltage, temperature). Make sure your test conditions match before concluding the part is underperforming
- "Gain-bandwidth product" for op-amps means bandwidth depends on closed-loop gain. At gain of 10, bandwidth is 1/10th of the GBW product. At gain of 1, bandwidth equals GBW. Check your gain setting
- Unity-gain bandwidth and -3 dB bandwidth are different things for amplifiers with peaking. A peaking amplifier can have gain > 1 at frequencies above its "bandwidth" — which is confusing but correct
