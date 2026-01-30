---
title: "Is the Frequency Response Flat Where It Should Be?"
weight: 20
---

# Is the Frequency Response Flat Where It Should Be?

Swept or stepped frequency response measurements. An amplifier that's down 3 dB at 5 kHz, a filter with the wrong cutoff frequency, a speaker crossover that's 6 dB off at the crossover point — frequency response tells you whether the signal chain treats all frequencies the way the design intended.

## Oscilloscope + Signal Generator: Swept Response

**Tool:** Function generator (sine wave, variable frequency) + oscilloscope (2 channels)
**When:** Measuring the amplitude-vs-frequency response of any analog stage

### Procedure

1. Connect the generator to the circuit input, scope CH1 to the input, CH2 to the output
2. Set the generator to a sine wave at a mid-band frequency (1 kHz for audio, or a frequency known to be in the passband)
3. Record the output amplitude — this is your 0 dB reference
4. Step the frequency through the range of interest. For audio: 20 Hz, 50 Hz, 100 Hz, 200 Hz, 500 Hz, 1 kHz, 2 kHz, 5 kHz, 10 kHz, 20 kHz. For wider-bandwidth circuits, extend to MHz
5. At each frequency, record the output amplitude
6. Calculate gain at each point: **Gain (dB) = 20 × log10(V_out / V_out_ref)**
7. Plot on a log-frequency scale

### What a Flat Response Looks Like

| Frequency range | Gain relative to mid-band | Verdict |
|----------------|--------------------------|---------|
| All points within ±0.5 dB | Flat | Excellent for audio, good for general analog |
| All points within ±1 dB | Mostly flat | Acceptable for most applications |
| All points within ±3 dB | Nominally flat | The ±3 dB points define the bandwidth |
| More than ±3 dB variation | Not flat | Intentional (filter/EQ) or a problem |

### What You Learn

- The actual bandwidth of the circuit (where it drops 3 dB)
- Whether there are peaks or dips in the passband (resonance, impedance mismatch, component issues)
- The rolloff slope outside the passband (first-order = 20 dB/decade, second-order = 40 dB/decade)

### Gotchas

- Keep the input amplitude constant. Some generators' output varies with frequency. Monitor CH1 and adjust as needed, or calculate the ratio V_out/V_in at each point
- Don't overdrive the circuit at any frequency. If the mid-band gain is 20 dB and you apply 1Vpp, the output is 10Vpp — make sure that doesn't clip. Set input level so the output stays in the linear range across all frequencies
- Source impedance and load impedance affect the response. Measure with the actual source and load, or specify the test conditions. An amplifier measured with no load may have a different response than one driving a speaker
- At high frequencies, cable capacitance and probe loading affect the measurement. Keep connections short and account for probe rolloff

## Audio Analyzer: Automated Sweep

**Tool:** Dedicated audio analyzer (e.g., QuantAsylum QA403, or PC-based tools like REW, ARTA)
**When:** You need precise, repeatable frequency response measurements with minimal effort

### How It Works

Audio analyzers automate the swept measurement: they generate a sine sweep (or stepped sine, or chirp) and measure the output amplitude at each frequency. The result is a plotted frequency response curve with dB accuracy.

### Advantages Over Manual Scope Measurement

- Automated sweep — measurements at hundreds of frequency points in seconds
- Better amplitude accuracy (typically ±0.1 dB vs. ±3–5% for scope)
- Built-in distortion and noise measurements alongside frequency response
- Calibrated and repeatable

### Gotchas

- Audio analyzers typically cover 20 Hz–20 kHz (some extend to 100 kHz+). For RF or wideband measurements, use a network analyzer or the scope method
- The analyzer's output impedance and input impedance are part of the measurement setup. Know them and account for them

## Speaker and Crossover Measurement

**Tool:** Signal generator + measurement microphone + audio analyzer (or scope)
**When:** Measuring the acoustic frequency response of speakers, or the electrical response of crossover networks

### Electrical Crossover Measurement

1. Feed a swept sine into the crossover input
2. Measure the output of each crossover section (high-pass, low-pass, band-pass)
3. Plot each on the same frequency axis
4. The crossover frequency is where the outputs are equal (typically -3 dB or -6 dB depending on the crossover type)

### Acoustic Measurement (Basic)

1. Position a measurement microphone at a fixed distance from the speaker (1 meter is standard)
2. Feed a swept sine or pink noise into the speaker
3. Record the microphone output and analyze frequency response

### Gotchas

- Acoustic measurements are dominated by the room. Reflections from walls, floor, and ceiling create peaks and dips that have nothing to do with the speaker. Use a near-field measurement (microphone close to the driver cone) to minimize room effects, or measure outdoors / in an anechoic space
- Microphone calibration matters. Cheap microphones have their own frequency response curve that colors the measurement. Use a calibrated measurement mic (e.g., UMIK-1, Dayton EMM-6) with its calibration file loaded

## Phase Response

**Concept:** Frequency response has two parts — amplitude and phase

### Why Phase Matters

- In feedback loops, phase shift determines stability. Too much phase shift at the unity-gain frequency = oscillation
- In audio crossovers, phase alignment between drivers affects the summed response at the crossover frequency
- In sensor signal conditioning, phase shift between channels causes errors in differential or ratio measurements

### Measuring Phase

1. Two-channel scope: CH1 on input, CH2 on output
2. At each frequency, measure the time delay between input and output zero crossings
3. Phase (degrees) = (time_delay / period) × 360°
4. Alternatively, use XY (Lissajous) mode — the shape of the ellipse indicates the phase relationship

### Gotchas

- Phase wraps at ±180°. A measured phase of +170° and -190° are the same thing. Be careful with plotting and interpretation near the ±180° boundary
- At very low frequencies, the period is long and small timing errors become large phase errors. At very high frequencies, propagation delay through cables and probes adds phase offset. Calibrate at mid-band
