---
title: "What's the THD / Noise Floor?"
weight: 10
---

# What's the THD / Noise Floor?

Distortion and noise measurements. In audio and precision analog circuits, the signal itself is the product — so how clean it is matters directly. THD tells you how much harmonic distortion a stage adds; noise floor tells you the quietest signal you can resolve.

## THD: Total Harmonic Distortion

**Concept:** How much of the output is harmonics that weren't in the input

### What THD Means

If you feed a pure sine wave into an amplifier, the output should be a pure sine wave (scaled by the gain). Any harmonics (2×, 3×, 4× the fundamental frequency) are distortion added by the amplifier. THD is the ratio of harmonic energy to fundamental energy:

**THD = sqrt(V2² + V3² + V4² + ...) / V1**

Where V1 is the fundamental amplitude and V2, V3, etc. are the harmonic amplitudes.

| THD | What it means in practice |
|-----|--------------------------|
| < 0.01% (-80 dB) | Excellent — high-fidelity audio, precision instrumentation |
| 0.01–0.1% (-80 to -60 dB) | Good — most listeners can't distinguish from 0.01% |
| 0.1–1% (-60 to -40 dB) | Audible on careful listening — acceptable for some applications |
| 1–10% (-40 to -20 dB) | Clearly audible — guitar amps intentionally run here |
| > 10% | Severe distortion — something is clipping or broken (unless intentional) |

## Oscilloscope FFT: Measuring THD

**Tool:** Oscilloscope with FFT, clean sine wave source
**When:** Quick bench measurement of harmonic distortion

### Procedure

1. Feed a clean sine wave from a low-distortion function generator into the circuit input
2. Capture the output on the scope
3. Run FFT with a Hann or flat-top window
4. Identify the fundamental peak and each harmonic peak (2×, 3×, 4×, 5× fundamental)
5. Read each peak's amplitude in dBV
6. Calculate THD:
   - Convert each harmonic from dBV to voltage: V = 10^(dBV/20)
   - THD = sqrt(V2² + V3² + ...) / V1 × 100%

### What You Learn

- The approximate THD of the circuit under test
- Which harmonics dominate — 2nd harmonic indicates asymmetric distortion, 3rd indicates symmetric clipping

### Gotchas

- Your signal source must be cleaner than the circuit you're measuring. If the generator has 0.1% THD and the circuit adds 0.05% THD, you can't separate them. For THD below 0.1%, you need a dedicated low-distortion oscillator
- The scope's own ADC has distortion (SFDR/SINAD specs). Most bench scopes can reliably measure THD down to about -50 to -60 dB (0.3–0.1%). Below that, the scope's distortion dominates
- FFT windowing matters for amplitude accuracy. Flat-top window gives the most accurate peak amplitudes; Hann window is a reasonable compromise
- Make sure the scope input isn't clipping — a clipped input adds harmonics from the scope, not the circuit

## Noise Floor Measurement

**Tool:** Oscilloscope (AC-coupled, bandwidth-limited) or dedicated audio analyzer
**When:** Measuring the residual noise when no signal is applied

### Oscilloscope Method

1. Connect the circuit output to the scope with no input signal applied (or input shorted to ground through a low impedance)
2. AC-couple the channel to remove DC offset
3. Set bandwidth limit (20 MHz for general noise; 20 kHz or 80 kHz for audio-specific measurements)
4. Set vertical scale to the most sensitive range that shows the noise without clipping
5. Measure the RMS voltage of the noise — use the scope's RMS measurement function

### Noise in dBV and dBu

| Reference | Formula | Common in |
|-----------|---------|-----------|
| dBV | 20 × log10(Vrms / 1V) | Consumer audio, general electronics |
| dBu | 20 × log10(Vrms / 0.775V) | Professional audio |
| dBFS | Relative to ADC full-scale | Digital audio |

### What You Learn

- The noise floor of the circuit — the quietest signal it can resolve
- The signal-to-noise ratio: **SNR = 20 × log10(V_signal_rms / V_noise_rms)**
- Whether the noise is broadband (hiss) or has specific frequency components (hum, buzz, switching noise)

### Gotchas

- Measurement bandwidth directly affects the noise reading. Wider bandwidth = more noise captured = higher reading. Always specify the bandwidth when quoting a noise floor measurement. A "1 mV noise floor" at 20 kHz bandwidth is very different from 1 mV at 20 MHz bandwidth
- The scope's own input noise may be comparable to or higher than the circuit's noise. Check the scope's noise floor (input terminated with 50 Ohm or shorted) and make sure the circuit's noise is well above it
- Grounding matters enormously for noise measurements. Use short ground connections and keep the probe cable away from switching power supplies, motors, and other noise sources

## SNR: Signal-to-Noise Ratio

**Tool:** Same as THD and noise floor measurements
**When:** Quantifying the usable dynamic range of a signal chain

### Procedure

1. Apply a full-scale sine wave at the circuit input (the maximum level before clipping)
2. Measure the output signal amplitude (RMS)
3. Remove the input signal (or short the input)
4. Measure the output noise amplitude (RMS) at the same gain setting
5. Calculate: **SNR (dB) = 20 × log10(V_signal / V_noise)**

### Typical SNR Values

| System | Typical SNR |
|--------|------------|
| Decent op-amp stage | 80–100 dB |
| CD-quality audio (16-bit) | ~96 dB (theoretical) |
| 24-bit audio interface | ~110–120 dB (practical) |
| Cheap audio amplifier | 60–80 dB |
| AM radio | ~40 dB |

### Gotchas

- SNR depends on the signal level. At lower signal levels, SNR decreases proportionally. Always specify the signal level when quoting SNR
- SINAD (Signal-to-Noise-and-Distortion) combines noise and distortion into one number: **SINAD = 20 × log10(V_signal / sqrt(V_noise² + V_distortion²))**. This is more representative of actual signal quality than SNR alone
