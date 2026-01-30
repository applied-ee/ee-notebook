---
title: "Is This Clock at the Right Frequency?"
weight: 10
---

# Is This Clock at the Right Frequency?

Frequency measurement is one of the most fundamental checks on any periodic signal. Clocks, oscillators, PWM outputs, data rates — when any of these are off, everything downstream misbehaves. The good news is that frequency is one of the easiest parameters to measure accurately.

## Oscilloscope: Frequency from Period Measurement

**Tool:** Oscilloscope, 10x probe, auto-measurement
**When:** Quick frequency check on any periodic signal you can see

### Procedure

1. Probe the signal and get a stable, triggered display
2. Use the scope's auto-measurement for frequency or period
3. For better accuracy, measure period (scope counts time between edges) and calculate frequency: **f = 1 / T**
4. Measure over multiple cycles if the scope allows — averaging reduces noise in the measurement

### Accuracy

| Scope timebase accuracy | Typical frequency accuracy |
|------------------------|---------------------------|
| Standard crystal (50 ppm) | ±0.005% — good enough for most digital clocks |
| TCXO-equipped scope | ±1–2 ppm — suitable for audio and moderate precision |
| GPS-disciplined or rubidium reference | Sub-ppm — for characterizing oscillators |

### What You Learn

- Whether the clock is at the expected frequency (within the scope's accuracy)
- Whether it's stable or drifting (watch the measurement over time)
- Gross errors: wrong crystal, wrong PLL multiplier, oscillator not running

### Gotchas

- Auto-frequency measurement can be wrong on signals with multiple zero crossings (ringing, noise, distorted waveforms). The scope triggers on extra edges and gives a too-high frequency reading. Clean up the trigger or use manual cursors
- Scope frequency accuracy is limited by the timebase. A 50 ppm timebase error means a 10 MHz reading could be off by 500 Hz — fine for a digital logic check, but not for characterizing a precision oscillator
- Low-frequency signals require long acquisition times for accurate period measurement. A 1 Hz signal needs at least 1 second of capture

## Oscilloscope: Frequency Counter Function

**Tool:** Oscilloscope with built-in hardware counter (many modern scopes have this)
**When:** You need higher accuracy than the standard auto-measurement

### How It Works

Some scopes include a dedicated hardware frequency counter that uses the scope's timebase directly rather than deriving frequency from the sampled waveform. This gives higher accuracy (often 6+ digits) and updates continuously.

### Procedure

1. Enable the frequency counter function (often in the Measure menu, separate from auto-measurements)
2. Set the input channel and trigger level
3. Read the frequency — it typically displays with more digits than the standard auto-measurement

### What You Learn

- Frequency to the accuracy of the scope's timebase
- Real-time frequency tracking as the oscillator warms up or drifts

### Gotchas

- Not all scopes have a hardware counter. Check your scope's specs
- The counter still needs a clean trigger — noisy or low-amplitude signals give erratic counts

## DMM: Frequency Measurement

**Tool:** DMM with frequency function (Hz)
**When:** Quick frequency check without setting up a scope

### Procedure

1. Set DMM to frequency mode (Hz)
2. Probe the signal
3. Read the frequency

### What You Learn

- Signal frequency as a single number — fast and simple

### Gotchas

- DMM frequency bandwidth is limited — typically a few hundred kHz to maybe 10 MHz depending on the meter. Above that, the reading is inaccurate or the meter won't register
- The DMM needs enough signal amplitude to trigger its comparator. Very low-level signals (millivolts) may not register. Check the meter's sensitivity spec for frequency mode
- DMM frequency measurement tells you nothing about waveshape, duty cycle, or jitter — just the fundamental repetition rate

## Checking Specific Oscillator Types

### Crystal Oscillators

- Should be within ±50 ppm of marked frequency at room temperature (typical for standard crystals)
- If way off (> 1000 ppm), the crystal may have been damaged, or the load capacitors are wrong
- If not oscillating at all, check power, enable pin, and load capacitor values. Probing the crystal pins with a scope can stop oscillation due to capacitive loading — use a high-impedance active probe or probe the oscillator's output buffer instead

### RC Oscillators

- Frequency depends on R and C values, which have tolerances of ±1–20%
- A 555 or RC relaxation oscillator at ±10% of expected frequency is probably fine — the components are within tolerance
- If way off, check the R and C values individually

### PLLs (Phase-Locked Loops)

- Output frequency depends on reference frequency, divider ratios, and VCO tuning range
- If the PLL output is wrong, check the reference clock first
- A PLL that can't lock shows an unstable or wildly fluctuating frequency. Check the lock-detect output, loop filter components, and VCO supply

### Gotchas

- Temperature affects all oscillators. Crystal oscillators drift tens of ppm across the operating range; RC oscillators drift hundreds or thousands of ppm. Measure at a stable, known temperature for meaningful comparison to specs
- Startup time matters — some oscillators take milliseconds to seconds to stabilize. Don't judge frequency during the startup transient
