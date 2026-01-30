---
title: "Is My Probe Bandwidth Sufficient?"
weight: 20
---

# Is My Probe Bandwidth Sufficient?

Bandwidth tells you the highest frequency your measurement system can faithfully reproduce. Below the bandwidth limit, signals come through accurately. At the bandwidth limit, the signal is already 30% low (-3 dB). Above it, the signal is increasingly attenuated and distorted. A probe with insufficient bandwidth doesn't just reduce amplitude — it changes waveshape, hides fast edges, and makes ringing disappear.

## What Bandwidth Means

**Concept:** -3 dB frequency

Bandwidth is the frequency at which the output amplitude drops to 70.7% of the input (-3 dB). This is not a cliff — attenuation increases gradually, so signals near bandwidth are already degraded, and signals well below bandwidth are accurate.

| Signal frequency vs. bandwidth | Amplitude error | Waveform fidelity |
|-------------------------------|-----------------|-------------------|
| 1/5th of BW (e.g., 20 MHz signal, 100 MHz BW) | < 2% | Excellent — edges and details preserved |
| 1/3rd of BW | ~5% | Good — minor softening of fast edges |
| At BW (1:1) | 30% | Poor — significant amplitude loss, edges rounded |
| 2x BW | > 60% | Bad — signal barely visible, shape is wrong |

### Rule of Thumb: 5x

For accurate amplitude measurements, you want system bandwidth at least 5x the signal's highest significant frequency component. For digital signals, the highest significant component is related to the rise time, not the clock frequency:

**f_knee ≈ 0.5 / t_rise**

A 10 MHz square wave with 5 ns rise time has frequency content up to ~100 MHz. You need 500 MHz of bandwidth to see it accurately — not 50 MHz.

## Rise Time and Bandwidth

**Relationship:** t_rise ≈ 0.35 / BW (for single-pole, Gaussian response)

This connects two ways of thinking about the same limitation:

| System bandwidth | System rise time |
|-----------------|-----------------|
| 20 MHz | 17.5 ns |
| 100 MHz | 3.5 ns |
| 200 MHz | 1.75 ns |
| 500 MHz | 0.7 ns |
| 1 GHz | 0.35 ns |

If the signal's rise time is faster than the system's rise time, you won't see the true edge. The displayed rise time will be the system's rise time, not the signal's.

### Measuring Rise Time

The displayed rise time combines the signal and the measurement system:

**t_displayed² ≈ t_signal² + t_system²**

If your 100 MHz scope (3.5 ns rise time) shows a 5 ns edge, the actual signal rise time is:

t_signal = sqrt(5² - 3.5²) ≈ 3.6 ns

The scope is significantly slowing down the displayed edge. With a 500 MHz scope (0.7 ns), you'd see the true ~3.6 ns edge.

## System Bandwidth: The Weakest Link

**Concept:** Cascade of probe and scope bandwidths

The measurement system bandwidth is approximately:

**1/BW_system² ≈ 1/BW_probe² + 1/BW_scope²**

Or equivalently, the system bandwidth is limited by the slowest element:

| Probe BW | Scope BW | System BW (approx.) |
|----------|----------|---------------------|
| 200 MHz | 100 MHz | ~89 MHz |
| 200 MHz | 200 MHz | ~141 MHz |
| 500 MHz | 100 MHz | ~98 MHz |
| 100 MHz | 100 MHz | ~71 MHz |

### Key Insight

A 200 MHz probe on a 100 MHz scope gives you about 89 MHz of system bandwidth — the scope is the bottleneck. Conversely, a 100 MHz probe on a 200 MHz scope gives about 89 MHz — the probe is the bottleneck. The system is only as fast as its slowest link, and mismatched components waste capability.

## When Bandwidth Matters vs. Doesn't

### Bandwidth Matters

- Digital signals: SPI, I2C at high speed, UART at high baud rates — fast edges carry high-frequency content regardless of data rate
- Switching power supplies: switch node transitions happen in nanoseconds even though the switching frequency is hundreds of kHz
- Rise/fall time measurements: if you can't see the true edge, you can't measure it
- Ringing and overshoot: high-frequency artifacts from impedance mismatches need bandwidth to be visible
- Clock signals: integrity, jitter, duty cycle all require seeing the true edge shape

### Bandwidth Doesn't Matter (Much)

- DC voltage measurements
- Audio-frequency signals (< 20 kHz) — essentially any probe is fine
- Slow analog signals: temperature sensors, strain gauges, battery voltage
- Low-frequency ripple on power rails (fundamental at switching frequency, typically < 2 MHz)

## Gotchas

- **1x probes have much lower bandwidth than 10x.** A typical 1x probe might be 6–15 MHz; the same probe switched to 10x might be 100–200 MHz. This is because the 10x attenuator forms a compensated divider that extends bandwidth, while 1x passes everything through the cable capacitance
- **Probe compensation affects bandwidth.** An under-compensated or over-compensated probe has degraded frequency response. Always compensate probes (adjust the trimmer cap on the scope's cal output) before trusting bandwidth-critical measurements
- **Sample rate is not bandwidth.** A scope with 1 GS/s sample rate and 100 MHz bandwidth still only shows 100 MHz of signal content. The extra sample rate helps with timebase accuracy and reduces aliasing, but doesn't extend bandwidth
- **Digital filtering can affect displayed bandwidth.** Some scopes apply sin(x)/x interpolation or other filtering that changes the effective displayed bandwidth. Check whether your scope's bandwidth spec is analog bandwidth or "effective" bandwidth after DSP
