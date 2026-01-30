---
title: "Is the Signal Present at This Node?"
weight: 10
---

# Is the Signal Present at This Node?

The first question when debugging a signal path. Before worrying about shape, amplitude, or distortion, confirm the signal exists where it should. A missing signal is a different class of problem than a wrong signal — it points to power issues, broken connections, or a stage that isn't running at all.

## Oscilloscope: Visual Confirmation

**Tool:** Oscilloscope, DC-coupled, 10x probe
**When:** You need to see whether a signal is present and get a quick read on its character

### Procedure

1. DC-couple the channel to see both the signal and its DC offset
2. Set vertical scale to the expected amplitude — if you don't know, start at 1V/div and adjust
3. Set timebase based on the expected frequency:
   - Audio (20 Hz–20 kHz): 1 ms/div to 10 ms/div
   - Digital logic (kHz–MHz): 1 µs/div to 100 µs/div
   - RF or high-speed digital: 10 ns/div to 100 ns/div
4. Probe the node of interest
5. Set trigger to edge mode on the signal channel, adjust trigger level to get a stable display

### What You Learn

- Signal present vs. absent — the most basic question
- Approximate amplitude, frequency, and waveshape at a glance
- DC offset — the signal may be riding on an unexpected DC level
- Whether the signal is continuous, intermittent, or bursty

### Gotchas

- If you see nothing, check your trigger. Auto trigger (free-running) will show whatever is there, even if it's just noise. Normal trigger will show a blank screen if the trigger condition isn't met — which can look like "no signal" when the signal is actually present but at the wrong level or frequency
- AC coupling hides DC offsets and blocks very low-frequency signals. Use DC coupling for the initial check
- If the expected signal is in the millivolt range, make sure you're not looking at probe noise. Disconnect the probe tip and see the noise floor; reconnect and see if the signal is clearly above it

## DMM: AC Voltage Check

**Tool:** DMM, V~ (AC Volts) mode
**When:** Quick yes/no — is there an AC signal here? No scope needed

### Procedure

1. Set DMM to AC Volts (V~)
2. Probe the node vs. ground
3. Any non-zero AC reading confirms signal presence

### What You Learn

- Whether an AC signal is present — expressed as RMS voltage
- Rough amplitude for comparison between nodes (input vs. output of a stage)

### Gotchas

- DMM AC mode has limited bandwidth (typically a few hundred Hz to 100 kHz for average-responding meters, up to ~1 MHz for true RMS). Signals above the meter's bandwidth read low or zero — a 10 MHz clock reads nothing on most DMMs
- The reading is RMS, which doesn't tell you waveshape. A 1V RMS sine wave and a 1V RMS square wave look identical on the meter
- AC-coupled measurement misses the DC component. If the signal is a small AC ripple on a large DC offset, the DMM sees only the AC part — which is useful for ripple checks but misleading if you expect the full signal level
- Some DMMs read AC+DC simultaneously; most don't. Check your meter's mode

## Speaker/Headphone: Audio Signal Tracing

**Tool:** Small speaker, headphone, or audio amplifier probe
**When:** Tracing audio signals through an audio signal chain without a scope

### Procedure

1. Connect a small speaker or headphone (through a DC-blocking capacitor, ~1–10 µF, to avoid DC current through the speaker)
2. Touch the probe to each stage in the audio chain: input jack, preamp output, tone control, power amp input
3. Listen for the signal at each stage — you'll hear where it disappears or degrades

### What You Learn

- Exactly where in the audio chain the signal stops
- Qualitative assessment of distortion, hum, noise — your ear is remarkably good at detecting these

### Gotchas

- Always use a DC-blocking capacitor. Many circuit nodes have a DC bias that could damage a speaker or headphone, or at minimum cause a loud pop
- Volume may vary dramatically between stages. A power amp output can damage headphones or your hearing. Start with the speaker away from your ear and bring it closer
- This technique is audio-frequency only. For RF or digital signals, use a scope
