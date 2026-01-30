---
title: "What Harmonics Are Present?"
weight: 30
---

# What Harmonics Are Present?

Moving from the time domain to the frequency domain. An FFT turns your oscilloscope into a basic spectrum analyzer, revealing harmonic content, switching artifacts, and unexpected frequency components that are invisible in the time-domain waveform.

## Oscilloscope FFT: Basic Spectrum View

**Tool:** Oscilloscope with FFT math function
**When:** Identifying frequency components in a signal — harmonics, spurs, noise peaks, switching artifacts

### Procedure

1. Capture the signal in the time domain with a stable trigger
2. Set the record length as long as practical — more samples give better frequency resolution
3. Enable FFT (usually under Math or Analysis menu)
4. Set the FFT window type:
   - **Hann** (Hanning): good general-purpose, good frequency resolution, moderate amplitude accuracy
   - **Flat-top**: best amplitude accuracy, wider frequency bins
   - **Rectangular** (none): best frequency resolution but severe spectral leakage — only use for signals that are exactly periodic within the record
5. Set the vertical scale to dBV or dBm for meaningful amplitude readings
6. Adjust the frequency span and center frequency to focus on the region of interest

### Reading the FFT Display

| Feature | Meaning |
|---------|---------|
| Tall peak at fundamental frequency | The primary signal — this should be the strongest component |
| Peaks at 2×, 3×, 4× fundamental | Harmonics — their amplitude relative to the fundamental indicates distortion |
| Peak at an unrelated frequency | Spur — interference, switching noise, or aliasing artifact |
| Elevated noise floor | Broadband noise — thermal, quantization, or EMI |
| Peak at switching frequency (and harmonics) | Switching converter artifacts coupling into the signal |

### What You Learn

- The frequency content of the signal beyond what's visible in the time domain
- Whether harmonics are present and how strong they are relative to the fundamental
- Whether external interference is coupling into the signal

### Gotchas

- FFT frequency resolution = sample rate / number of points. For a 1 MS record at 1 GS/s, resolution is 1 kHz. If you need to distinguish two signals 100 Hz apart, you need a 10 MS+ record
- Spectral leakage from windowing can make a single-frequency signal look like it has sidebands. Use a Hann or flat-top window to minimize this. Rectangular window leakage can mask weak signals near strong ones
- The oscilloscope's noise floor limits what you can see. On most bench scopes, the FFT noise floor is around -50 to -70 dBc. You can't see harmonics below that
- Aliasing: frequency components above the Nyquist frequency (half the sample rate) fold back and appear as false peaks at lower frequencies. Use bandwidth-limited acquisition if you suspect aliasing

## Identifying Specific Artifacts

### Switching Converter Noise

Switching regulators operate at a fixed frequency (typically 100 kHz to 3 MHz) and produce harmonics. In the FFT:

- Look for peaks at the switching frequency and its harmonics (2×, 3×, 4×...)
- These show up on the output rail and can couple into sensitive analog signals
- Compare to the converter datasheet's switching frequency to confirm identification

### Mains Hum

50/60 Hz fundamental with harmonics at 100/120 Hz, 150/180 Hz, etc.:

- Strong 50/60 Hz = magnetic coupling (ground loop, transformer leakage)
- Strong 100/120 Hz = full-wave rectifier ripple (power supply issue)
- Harmonics extending to kHz range = SCR/triac dimmer, nonlinear load, or power supply with high ripple current

### Clock Crosstalk

Digital clocks coupling into analog signals:

- Peak at the clock frequency and its odd harmonics (square waves have primarily odd harmonics)
- Amplitude decreases with frequency as 1/f (−20 dB/decade) for an ideal square wave
- If crosstalk peaks are strong relative to the analog signal, there's a layout or shielding problem

## FFT vs. Dedicated Spectrum Analyzer

| Feature | Oscilloscope FFT | Dedicated spectrum analyzer |
|---------|------------------|-----------------------------|
| Frequency range | DC to scope bandwidth | Varies — often kHz to GHz |
| Dynamic range | 50–70 dB (limited by ADC bits) | 80–120+ dB |
| Frequency resolution | Depends on record length | Very fine — Hz resolution common |
| Speed | One-shot or slow update | Real-time swept or FFT-based |
| Cost | Already have it (if you have a scope) | Separate expensive instrument |
| Best for | Quick look at harmonics, identifying spurs | Precision measurements, EMI compliance, RF characterization |

For most bench debugging, the scope FFT is sufficient. When you need dynamic range beyond ~60 dB (e.g., measuring -80 dBc spurs on a clock), a dedicated spectrum analyzer or SDR-based tool is necessary.

### Gotchas

- Scope FFT update rate is slow — you're looking at a snapshot, not a real-time sweep. Intermittent interference may not appear in a single capture. Use persistence or averaging
- Don't confuse FFT artifacts with real signals. If a peak appears at exactly half the sample rate, it's likely aliasing. If it moves when you change the sample rate, it's definitely an artifact
