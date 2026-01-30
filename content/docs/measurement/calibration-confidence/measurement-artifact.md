---
title: "Could This Be a Measurement Artifact?"
weight: 20
---

# Could This Be a Measurement Artifact?

Questioning the reading before trusting it. The most dangerous measurements are the ones that look reasonable but are wrong. Probe artifacts, aliasing, ground bounce, and coupling from your hands can all create readings that look like real circuit behavior — and send you down the wrong debugging path.

## Common Artifacts and How to Identify Them

### Ground Lead Ringing

**Symptom:** Overshoot and damped oscillation on fast edges, typically ~100 MHz
**Cause:** Inductance of the ground clip resonating with probe capacitance
**Test:** Switch from alligator clip ground to spring-tip ground. If the ringing disappears or changes frequency, it was the ground lead
**See also:** [Is My Ground Connection Adding Artifacts?]({{< relref "../probing-technique/ground-connection" >}})

### Probe Loading Changing Circuit Behavior

**Symptom:** Circuit works differently when the probe is connected — oscillation starts/stops, frequency shifts, amplitude changes
**Cause:** Probe capacitance (~15 pF for passive 10x) and resistance (10 MOhm) loading the circuit
**Test:** Compare behavior with and without the probe connected. Use an active probe (~1 pF) if available. If the behavior changes with probe connection, the probe is affecting the circuit
**See also:** [Am I Loading the Circuit?]({{< relref "../probing-technique/circuit-loading" >}})

### Aliasing

**Symptom:** A signal appears at the wrong frequency on the scope, or a repetitive signal looks like it has an unexpected modulation
**Cause:** Sample rate too low — signals above half the sample rate fold back to incorrect lower frequencies
**Test:** Change the scope's sample rate or timebase. If the apparent signal frequency changes when you change the sample rate, it's an alias. If the frequency stays constant, it's real
**How to confirm:** Enable bandwidth limiting (reduces high-frequency content before sampling) and see if the artifact disappears

### Ghost Voltages (DMM)

**Symptom:** DMM reads 30–80V AC on a circuit that's off and should be at 0V
**Cause:** Capacitive coupling from nearby live conductors to the high-impedance (10 MOhm) DMM input
**Test:** Switch to LoZ mode (~3 kOhm input) if your meter has it. Ghost voltages collapse to zero; real voltages remain
**See also:** [Can I Touch/Probe This Safely?]({{< relref "../safety-high-energy/can-i-probe-safely" >}})

### Hand Capacitance and Body Coupling

**Symptom:** The signal changes when you move your hand near the probe, the board, or a component
**Cause:** Your body acts as an antenna (picks up 50/60 Hz mains) and as a capacitor (couples to high-impedance nodes)
**Test:** Stabilize the probe mechanically (clip or fixture), step back, and observe whether the signal changes. If it does, your body was coupling into the measurement
**Fix:** Use a probe holder or clip. For sensitive measurements, maintain consistent body position or step away entirely

### Scope Input Clipping

**Symptom:** Waveform appears flat-topped or distorted, but the circuit is clean
**Cause:** Signal exceeds the scope's input range — the ADC clips, and the display shows a flat top that isn't in the signal
**Test:** Increase vertical scale (reduce sensitivity) or switch to 10x probe attenuation. If the flat top disappears, the scope was clipping, not the circuit

### Trigger Coupling Artifact

**Symptom:** Waveform looks like it has jitter, frequency modulation, or is unstable
**Cause:** Trigger is set wrong — triggering on noise, on the wrong edge, or on a signal with multiple crossings per cycle
**Test:** Switch trigger source, adjust trigger level, or use a different trigger mode (e.g., pulse-width trigger instead of edge trigger). If the waveform stabilizes, the trigger was the problem

## The Artifact Checklist

When a measurement looks unexpected, run through this list before trusting it:

1. **Is the probe compensated?** (Uncompensated probe distorts square waves and frequency response)
2. **Is the ground lead short?** (Long ground adds ringing to fast edges)
3. **Is the probe loading the circuit?** (High-Z nodes, oscillators, feedback networks are sensitive)
4. **Is the sample rate adequate?** (At least 5× the highest frequency component)
5. **Is the scope input clipping?** (Check that the signal fits on screen with margin)
6. **Am I coupling noise into the measurement?** (Hand, body, nearby cables)
7. **Is the trigger stable?** (Jittery display = trigger problem, not necessarily signal problem)
8. **Is the DMM on the right mode and range?** (AC vs. DC, auto-range settling, LoZ for ghost voltages)

## The Ultimate Test: Does It Change When You Change the Measurement Setup?

**Principle:** Real signals are properties of the circuit. Artifacts are properties of the measurement.

If a signal changes when you:
- Change the probe → it's probe-dependent (likely artifact or loading effect)
- Change the ground lead → it's ground-related (ringing artifact)
- Change the sample rate → it's aliasing
- Move your hand → it's body coupling
- Change the scope settings → it's a scope issue

If the signal stays the same regardless of measurement setup changes, it's real.

### Gotchas

- Some real signals are affected by measurement setup (probe loading changing oscillator frequency). The distinction isn't always clean. Use the lightest-touch measurement you can to minimize the observer effect
- Confirming an artifact explains a weird reading is satisfying but doesn't answer the original question. Once you've identified and eliminated the artifact, you still need to make the measurement correctly
