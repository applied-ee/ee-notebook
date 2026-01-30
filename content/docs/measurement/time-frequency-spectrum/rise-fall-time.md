---
title: "What's the Rise/Fall Time?"
weight: 20
---

# What's the Rise/Fall Time?

Edge speed matters. Rise and fall times affect signal integrity, EMI emissions, logic timing margins, and crosstalk. Fast edges are what create high-frequency content in digital signals — a 10 MHz clock with 1 ns edges has frequency content into the GHz range. Measuring edges accurately requires knowing when your scope and probe are the bottleneck.

## Oscilloscope: Rise/Fall Time Measurement

**Tool:** Oscilloscope, 10x probe with spring-tip ground
**When:** Characterizing edge speed on digital signals, switch nodes, clock outputs, or driver outputs

### Procedure

1. Use a 10x probe with the shortest possible ground connection (spring-tip ground). Ground lead inductance adds ringing that corrupts edge measurements
2. Probe the signal and get a clean, stable trigger on the edge of interest
3. Set timebase to zoom in on the edge — the edge should occupy at least 3–5 horizontal divisions for accurate measurement
4. Use the scope's auto-measurement for rise time (10% to 90% of amplitude is the standard definition)
5. Measure both rise and fall times — they may differ, which is informative

### Measurement Standard

Rise time is measured from **10% to 90%** of the final voltage (industry standard). Some scopes also offer 20%-80% measurement, which gives a shorter number for the same edge. Make sure you know which definition your scope is using, especially when comparing to datasheet specs.

### What You Learn

- Actual edge speed vs. datasheet spec
- Whether rise and fall times are matched (asymmetry indicates driver imbalance)
- Whether the edge is faster than expected (layout issue, no series termination) or slower (excessive capacitive loading, weak driver)

### Is Your Scope the Bottleneck?

The displayed rise time combines the signal and the measurement system:

**t_displayed² = t_signal² + t_scope² + t_probe²**

Or approximately:

**t_system = 0.35 / BW_system**

| Your scope/probe BW | System rise time | Can accurately measure edges down to |
|---------------------|-----------------|-------------------------------------|
| 50 MHz | 7 ns | ~20 ns (displayed ≈ signal if signal >> system) |
| 100 MHz | 3.5 ns | ~10 ns |
| 200 MHz | 1.75 ns | ~5 ns |
| 500 MHz | 0.7 ns | ~2 ns |
| 1 GHz | 0.35 ns | ~1 ns |

**Rule of thumb:** If the displayed rise time is less than 2× your system rise time, the scope is significantly affecting the measurement. Upgrade probe/scope for accurate results.

### De-Embedding the Measurement System

If you know the scope system's rise time, you can estimate the true signal rise time:

**t_signal = sqrt(t_displayed² - t_system²)**

Example: 100 MHz scope (3.5 ns system rise time) shows 5 ns edge:

t_signal = sqrt(25 - 12.25) = sqrt(12.75) ≈ 3.6 ns

The true edge is significantly faster than what the scope displays.

### Gotchas

- Ground lead ringing looks like overshoot and can confuse the 10%/90% measurement thresholds. Use spring-tip ground for any rise time measurement
- Probe loading slows down edges. If the source impedance is high, the probe capacitance forms an RC filter that rounds the edge. An active probe (~1 pF) gives truer results than a passive probe (~15 pF) on high-Z nodes
- Sample rate matters too. Nyquist says you need at least 2 samples per cycle of the highest frequency component, but for accurate edge reconstruction you want at least 5–10 samples on the edge. For a 1 ns rise time, that's 5–10 GS/s
- Sin(x)/x interpolation helps reconstruct edges between samples but can't recover information that wasn't captured. If the sample rate is marginal, the interpolated edge shape may be misleading

## Relating Rise Time to Signal Integrity

### EMI: Faster Edges = More High-Frequency Energy

The -3 dB "knee" frequency of a digital signal is:

**f_knee ≈ 0.5 / t_rise**

| Rise time | Knee frequency | EMI concern |
|-----------|---------------|-------------|
| 100 ns | 5 MHz | Low — AM radio band |
| 10 ns | 50 MHz | Moderate — FM/VHF band |
| 1 ns | 500 MHz | High — UHF, cellular bands |
| 200 ps | 2.5 GHz | Very high — WiFi, Bluetooth bands |

Slowing edges (with series resistors or ferrite beads) is a primary EMI reduction technique. But don't slow edges so much that timing margins are violated.

### Crosstalk

Crosstalk between adjacent traces is proportional to dV/dt — the rate of voltage change. Faster edges couple more energy into neighboring signals. If you see unexpected glitches on adjacent signals coinciding with fast edges on a driven signal, the rise time is too fast for the trace spacing.

### Gotchas

- Datasheet rise times are specified under particular load conditions. Your circuit may have different loading, giving different actual rise times. This is expected — the important thing is whether the rise time works for your signal integrity requirements
- Series termination resistors intentionally slow edges and reduce ringing. Measure rise time at the receiver end (after the transmission line), not at the driver output, to see what the receiving device actually sees
