---
title: "When Was This Last Calibrated / Compensated?"
weight: 30
---

# When Was This Last Calibrated / Compensated?

Probe compensation, scope self-cal, DMM calibration status. Some things you should check every session, some periodically, and some only when something seems off. Knowing the difference keeps you from wasting time on unnecessary calibration while also catching the cases where a drifted instrument is lying to you.

## Probe Compensation: Every Session (or Every Scope Change)

**Tool:** Oscilloscope probe + scope's calibration output (1 kHz square wave, typically on front panel)
**When:** Every time you use a different probe, move a probe to a different scope, or at the start of a session

### Procedure

1. Connect the probe to the scope's CAL output (1 kHz square wave, typically ~3Vpp or 5Vpp)
2. Set the scope to show a clean square wave (1 ms/div, 1V/div)
3. Look at the square wave edges:
   - **Correct compensation:** Square corners, flat tops and bottoms
   - **Under-compensated:** Rounded leading edges, the square wave looks like it droops on each transition
   - **Over-compensated:** Overshoot on each transition, the square wave has spikes at the corners
4. Adjust the trimmer capacitor on the probe (small screwdriver, usually accessible near the BNC connector or on the probe body) until the square wave is flat

### Why It Matters

An uncompensated probe has a frequency-dependent attenuation ratio. At DC, the 10:1 ratio is correct. At higher frequencies, the ratio changes — either over-attenuating (under-compensated) or under-attenuating (over-compensated). This distorts any waveform with high-frequency content.

### Gotchas

- Compensation is specific to the probe-scope combination. Moving a probe to a different scope input (even the same scope, different channel) can require re-compensation
- The compensation adjustment range is limited. If you can't get a flat square wave, the probe may be damaged or fundamentally incompatible with the scope's input capacitance
- On switchable 1x/10x probes, compensation only applies to the 10x setting. In 1x mode, there's no compensation network

## Scope Self-Cal: Periodically (Monthly or After Temperature Changes)

**Tool:** Oscilloscope's internal self-calibration routine
**When:** Monthly, after significant temperature changes (moving the scope, turning it on after being cold), or whenever readings seem off

### Procedure

1. Warm up the scope for at least 30 minutes (the manufacturer's recommendation varies, but 20–30 minutes is typical)
2. Disconnect all probes and inputs (or cap the inputs with 50 Ohm terminators if the scope recommends it)
3. Run the self-calibration routine (usually in the Utility or Calibration menu)
4. Wait for it to complete — this can take 5–15 minutes as the scope calibrates all channels, ranges, and timebase settings

### What Self-Cal Adjusts

- DC offset on each channel at each sensitivity setting
- Vertical gain accuracy across ranges
- Timebase accuracy
- Channel-to-channel delay matching
- ADC linearity compensation

### Gotchas

- Self-cal must be done with inputs disconnected. Any signal on the inputs during self-cal corrupts the calibration
- Self-cal is not a substitute for traceable calibration. It corrects for temperature drift and aging within the scope's self-calibration range, but it can't correct for systematic errors outside that range
- If self-cal fails or reports errors, the scope may need service

## DMM Calibration: Annually (or When Required)

**Tool:** Known voltage/resistance references (or send the meter to a calibration lab)
**When:** Annually for critical measurements, or whenever the meter's readings seem inconsistent

### What Calibration Does

A DMM is calibrated by measuring known reference standards and adjusting the meter's internal correction factors to match. This requires:

- A voltage reference more accurate than the meter (at least 4× better)
- A resistance reference (precision resistor decade or individual standards)
- A current reference (precision current source)
- Traceability to a national standard (NIST, NPL, etc.)

### Self-Check with Known References

Between formal calibrations, you can do a sanity check:

1. **Voltage:** Measure a known reference (1.5V battery is crude; a precision voltage reference IC like LM4040 or REF5025 is better). Compare to the reference's spec
2. **Resistance:** Measure precision resistors of known value (1% or better, recently purchased from a reputable supplier). Compare to the marked value
3. **Continuity:** Short the leads together. The reading should be < 0.5 Ohm and the relative/delta function should zero it out

### Gotchas

- Most hobbyists never formally calibrate their meters, and for bench electronics work (checking power rails, confirming component values, debugging), this is fine. The factory calibration of a name-brand meter is typically valid for 1–3 years
- If your meter disagrees with another meter, the question is which one to trust. The one with more recent calibration wins — otherwise, check both against a known reference
- Cheap meters may never have been properly calibrated at the factory. Treat their readings as approximate

## What to Check at the Start of Each Session

A quick pre-flight checklist that catches the most common issues:

| Check | How | Time |
|-------|-----|------|
| Probe compensation | Cal output, adjust if needed | 30 seconds per probe |
| DMM leads — zero check | Short leads, read resistance, verify < 0.5 Ohm | 10 seconds |
| DMM battery | If display is dim or readings are erratic, change battery | 5 seconds to notice |
| Scope vertical position | Verify ground level with probe grounded (ground clip to probe tip) | 10 seconds per channel |
| Scope trigger | Set to Auto, verify you see baseline noise | 5 seconds |

### Gotchas

- Skipping probe compensation is the #1 preventable measurement error for scope users. It takes 30 seconds and catches a problem that ruins all subsequent measurements
- DMM lead zero-check catches corroded probes, broken wires inside leads, and bad banana plug contacts. A lead that reads 2 Ohm adds 2 Ohm to every resistance measurement
