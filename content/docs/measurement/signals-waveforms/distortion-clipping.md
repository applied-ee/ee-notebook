---
title: "Is This Stage Adding Distortion or Clipping?"
weight: 40
---

# Is This Stage Adding Distortion or Clipping?

Waveform integrity through amplifier stages, filters, and buffers. The signal is present and roughly the right shape — but is it clean? Soft clipping, crossover distortion, saturation, slew-rate limiting, and unexpected nonlinearity all show up here.

## Oscilloscope: Visual Distortion Check

**Tool:** Oscilloscope, 2 channels, DC-coupled
**When:** Comparing a stage's input and output to see what the stage is doing to the signal

### Procedure

1. CH1 on the input, CH2 on the output of the suspect stage
2. Trigger on CH1
3. Adjust vertical scales so both waveforms are clearly visible — scale the output by the expected gain
4. Compare waveshapes: the output should be a clean, scaled (and possibly inverted) copy of the input

### What to Look For

| Symptom | What it means |
|---------|--------------|
| Flat tops or bottoms on output | Clipping — signal exceeds the stage's output swing. Hitting supply rail or internal limit |
| Flat tops on positive half only (or negative only) | Asymmetric clipping — one side of the output stage is limiting. Bias issue, failed transistor, or asymmetric supply |
| Notch or step at zero crossing | Crossover distortion — class-B or class-AB output stage with insufficient bias current. Dead zone where neither output transistor conducts |
| Rounded edges on what should be sharp | Slew-rate limiting — the output stage can't change voltage fast enough. The amplifier's slew rate is being exceeded |
| Waveshape changes with amplitude | Nonlinearity — small signals look clean, large signals get distorted. Gain compression, soft clipping, or nonlinear transfer function |
| Output is correct shape but slightly different amplitude than expected | Gain error — component tolerance, loading, or temperature drift. Not distortion per se, but worth noting |
| Ringing or oscillation on edges | Instability — the stage is borderline oscillating. Usually a feedback/compensation issue |

### Clipping Identification

Clipping is the most common amplitude-related distortion. To confirm:

1. Reduce the input signal amplitude
2. If the flat section disappears and the output shape returns to normal, the stage was clipping
3. The clipping level tells you the output voltage swing limit of the stage
4. Compare to the supply voltage — most op-amps clip 1–2V below the rail (unless rail-to-rail output)

### Gotchas

- Don't confuse probe-induced distortion with circuit distortion. If you're probing a high-impedance node, the probe capacitance can round off edges and ring. Use a 10x probe or active probe, and check with the ground spring
- AC coupling on the scope can make a clipped signal look non-clipped (the DC level shifts to center the waveform). Use DC coupling when checking for clipping
- Digital scope aliasing can make a clean signal look distorted if the sample rate is too low. Make sure the scope's sample rate is at least 5x the signal frequency

## Oscilloscope XY Mode: Transfer Curve

**Tool:** Oscilloscope, XY display mode
**When:** Visualizing the input-to-output transfer function of a stage directly

### Procedure

1. CH1 (X-axis) on the stage input
2. CH2 (Y-axis) on the stage output
3. Switch to XY display mode
4. Apply a sine wave or slowly varying signal at the input
5. The display shows output vs. input — the transfer curve

### Interpreting the Display

| Display shape | Meaning |
|--------------|---------|
| Straight diagonal line | Linear transfer — constant gain, no distortion |
| Diagonal line that flattens at the ends | Clipping — the output saturates at the extremes |
| S-curve | Compression/expansion — nonlinear but smooth |
| Flat region near center with diagonal sides | Crossover distortion — dead zone around zero |
| Ellipse (for sine-to-sine) | Phase shift between input and output (not distortion, just delay or filter effect) |
| Kinked or stepped line | Abrupt nonlinearity — threshold effects, switching behavior |

### What You Learn

- The actual transfer function of the stage, visualized directly
- Where in the amplitude range distortion occurs (top, bottom, center, everywhere)
- Whether the nonlinearity is smooth (soft clipping) or abrupt (hard clipping, crossover)

### Gotchas

- XY mode requires both signals to be in the scope's bandwidth and properly scaled. If one channel clips at the scope input, the display is wrong
- The input signal must sweep the full operating range to reveal clipping at the extremes. A small test signal only shows the linear region
- Phase delay through the stage causes the trace to open into an ellipse even if there's no distortion. This is normal for stages with frequency-dependent phase shift. At DC or very low frequencies, the ellipse collapses to a line

## FFT: Harmonic Distortion Check

**Tool:** Oscilloscope with FFT function
**When:** Quantifying distortion as harmonic content — especially for audio and analog signal chains

### Procedure

1. Apply a clean sine wave at the frequency of interest
2. Capture the output of the stage
3. Run FFT on the scope (or export data and FFT in software)
4. Look for harmonics of the fundamental frequency:
   - 2nd harmonic (2× fundamental) — asymmetric distortion
   - 3rd harmonic (3× fundamental) — symmetric distortion (clipping)
   - Higher-order harmonics — harder clipping or crossover distortion

### Interpreting the Spectrum

| Harmonic pattern | Typical cause |
|-----------------|---------------|
| Strong 2nd harmonic | Asymmetric transfer curve — single-ended stage with uneven bias |
| Strong 3rd harmonic | Symmetric clipping — both halves of the waveform are limited equally |
| Many harmonics, slowly declining | Hard clipping — sharp corners in the time domain spread energy across many harmonics |
| Strong odd harmonics (3rd, 5th, 7th) | Crossover distortion — the zero-crossing step creates odd-harmonic content |
| No significant harmonics | Clean — distortion is below the measurement noise floor |

### What You Learn

- Total harmonic distortion (THD) by measuring harmonic amplitudes relative to the fundamental
- Which type of distortion is dominant (2nd harmonic = asymmetric, 3rd = symmetric, high-order = hard clipping)

### Gotchas

- The input sine wave must be clean. Distortion in the test signal shows up in the output and can't be distinguished from distortion added by the stage. Use a good function generator or a sine wave with a bandpass filter
- FFT windowing affects the result. Use a flat-top window for amplitude accuracy or a Hann window for frequency resolution. Rectangular window (no window) causes spectral leakage that looks like distortion
- The scope's own noise and distortion set the measurement floor. On most bench scopes, you can reliably measure THD down to about -50 to -60 dB (0.1–0.3%). Below that, you need a dedicated audio analyzer or a low-noise ADC
