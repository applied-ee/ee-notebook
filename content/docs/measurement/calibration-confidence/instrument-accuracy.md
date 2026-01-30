---
title: "Is My Instrument Accurate Enough for This?"
weight: 10
---

# Is My Instrument Accurate Enough for This?

Matching instrument specs to measurement needs. A 3½-digit DMM is perfect for checking a 5V power rail and useless for measuring a 1.000V reference to four digits. Knowing when your instrument is the limiting factor — and when it's more than adequate — prevents both false confidence and unnecessary upgrades.

## DMM: Resolution and Accuracy

**Concept:** Digit count, resolution, and accuracy are different things

### Digits and Resolution

| DMM type | Display counts | Resolution (on 4V range) | Smallest change visible |
|----------|---------------|-------------------------|------------------------|
| 3½-digit | 2000 | 1 mV | 0.001V |
| 4½-digit | 20,000 | 0.1 mV | 0.0001V |
| 5½-digit | 200,000 | 0.01 mV | 0.00001V |
| 6½-digit | 2,000,000 | 0.001 mV | 0.000001V |

Resolution is the smallest increment the display can show. It does not mean the reading is accurate to that digit.

### Accuracy Specification

DMM accuracy is typically stated as: **±(% of reading + counts)**

Example: Klein MM520 on DC Volts: ±(0.5% + 3 counts)

Measuring 5.000V: error = ±(0.025V + 0.003V) = ±0.028V

So the true voltage is somewhere between 4.972V and 5.028V.

### When Accuracy Matters

| Measurement | Required accuracy | 3½-digit DMM sufficient? |
|------------|-------------------|-------------------------|
| Is 5V rail present? (± 10%) | ±0.5V | Yes — easily |
| Is 3.3V rail within spec? (±5%) | ±0.165V | Yes |
| Matching 1% resistors | ±0.5% or better | Marginal — need to verify meter accuracy is better than 0.5% |
| Checking a 2.500V reference to ±0.1% | ±2.5 mV | No — need at least 4½ digits and ±0.05% accuracy |
| Calibrating a DAC to 12-bit accuracy | ±0.6 mV (on 0–2.5V range) | No — need 5½ digits or better |

### What You Learn

- Whether your DMM can meaningfully distinguish "in spec" from "out of spec" for the measurement you need
- How many digits of the display are meaningful (the last digit is always uncertain)

### Gotchas

- DMM accuracy degrades at the extremes of each range. Measuring 0.5V on a 40V range wastes most of the resolution. Use the lowest range that accommodates the expected voltage
- Accuracy specs are typically given at 23°C ±5°C and within 1 year of calibration. Outside these conditions, accuracy degrades. Check the "temperature coefficient" spec for temperature-dependent error
- AC accuracy is much worse than DC accuracy on most DMMs, and degrades rapidly with frequency

## Oscilloscope: When Accuracy and Sample Rate Matter

### Vertical (Amplitude) Accuracy

Most bench oscilloscopes have 3–5% vertical accuracy — vastly worse than a DMM. The scope shows you waveshape; the DMM tells you the actual voltage.

| Measurement | Use scope? | Use DMM? |
|------------|-----------|----------|
| DC voltage to ±1% | No | Yes |
| Waveform shape, edges, ringing | Yes | No |
| Relative amplitude (ratio between two signals) | Yes (same scope, same settings) | N/A |
| Peak-to-peak on a fast signal | Yes (only option) | N/A |

### Sample Rate

**Nyquist rule:** Sample rate must be at least 2× the highest frequency component. In practice, 5–10× is needed for accurate waveform reconstruction.

| Signal type | Minimum sample rate | Recommended sample rate |
|------------|--------------------|-----------------------|
| Audio (20 kHz) | 40 kS/s | 200 kS/s |
| 1 MHz square wave (with ~3 ns edges) | 600 MS/s | 2 GS/s |
| 100 MHz clock | 200 MS/s | 1 GS/s |
| 1 ns rise time | 2 GS/s | 5–10 GS/s |

### Gotchas

- Sample rate per channel may be lower than the headline rate. Many scopes advertise "2 GS/s" but deliver 1 GS/s per channel when using 2 channels, or 500 MS/s per channel on all 4 channels. Check the per-channel rate
- Memory depth limits effective sample rate at long timebases. A scope with 1 MS memory at 1 GS/s can capture only 1 ms. For a 100 ms capture, it drops to 10 MS/s. Check whether your scope reduces sample rate at the timebase you need

## When Your Instrument Isn't Enough

### Signs You've Hit the Instrument's Limit

- The reading fluctuates in the last digit by more than you'd expect from the signal itself
- You're trying to see a difference smaller than the accuracy spec
- The scope displays a clean square wave but you suspect ringing that's below the scope's bandwidth
- FFT noise floor is higher than the signal you're looking for

### Options

| Problem | Solution |
|---------|---------|
| DMM accuracy insufficient | Borrow or buy a 4½/5½-digit DMM, or use a known reference to check |
| Scope bandwidth too low | Use a higher-bandwidth probe or scope; or infer behavior from slower measurements |
| Scope sample rate too low | Reduce timebase (zoom in on less time), accept reduced record length |
| Noise floor too high | Average, use bandwidth limiting, improve probe grounding, or use a lower-noise instrument |

### Gotchas

- Averaging reduces random noise but hides intermittent events. If you're looking for occasional glitches, don't average
- A more expensive instrument isn't always necessary. Understanding and optimizing what you have (proper probing, correct range selection, bandwidth limiting, averaging) can often get you there
