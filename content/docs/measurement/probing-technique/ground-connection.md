---
title: "Is My Ground Connection Adding Artifacts?"
weight: 30
---

# Is My Ground Connection Adding Artifacts?

The ground lead on your scope probe is not just a return path — it's an inductor. Every millimeter of wire has inductance (~1 nH/mm), and that inductance resonates with the probe's input capacitance to create ringing on fast edges. The result is overshoot and oscillation in your measurement that does not exist in the circuit. Learning to recognize this artifact is one of the most useful things you can do at the bench.

## The Physics: Ground Lead Inductance

**Concept:** LC resonance between lead inductance and probe capacitance

### The Numbers

| Ground lead | Approximate length | Inductance (~1 nH/mm) | Resonant freq with 15 pF probe cap |
|------------|-------------------|----------------------|-------------------------------------|
| Standard alligator clip lead | 150 mm (6") | ~150 nH | ~106 MHz |
| Short clip lead | 75 mm (3") | ~75 nH | ~150 MHz |
| Spring-tip / barrel ground | 3–5 mm | ~3–5 nH | ~580–750 MHz |

**Resonant frequency: f = 1 / (2 * pi * sqrt(L * C))**

At the resonant frequency, the ground lead's impedance peaks, and any fast edge in the signal excites this resonance. The result is a damped oscillation (ringing) superimposed on the actual signal.

### What It Looks Like

On a fast edge (say, a digital signal with 5 ns rise time), the standard 6-inch ground clip adds:

- 20–50% overshoot on the rising edge
- Damped ringing at ~100 MHz that decays over 2–5 cycles
- The ringing frequency is set by the ground lead, not the circuit

This looks exactly like real impedance-mismatch ringing in the circuit — and that's what makes it dangerous. You can spend hours debugging a "ringing problem" that's actually your probe.

## Spring-Tip / Barrel Ground: The Fix

**Tool:** Scope probe with spring-tip ground (barrel ground, ground spring)
**When:** Any measurement where you care about edge fidelity — digital signals, switching nodes, anything with rise times under ~50 ns

### How It Works

The spring-tip ground makes contact through the probe's ground barrel (the metal ring near the probe tip) directly to a ground point on the board. The ground path is only a few millimeters long, reducing inductance by 30–50x compared to the alligator clip lead.

### Procedure

1. Remove the alligator clip ground lead from the probe
2. Attach the spring-tip ground accessory (comes with most probes, often left in the box)
3. Press the probe tip onto the signal point and the ground barrel/spring onto the adjacent ground pad or plane
4. For best results, probe directly at a component where signal and ground are adjacent (e.g., a decoupling cap, an IC pin pair)

### What You Learn

- The true edge shape without probe-induced ringing
- Whether overshoot and ringing seen with the long ground lead were real circuit artifacts or measurement artifacts
- Cleaner, more trustworthy measurements of rise time, overshoot, and settling

### Gotchas

- The spring-tip requires that signal and ground points are physically close together on the board (within a few mm). On boards without accessible ground near the measurement point, you may need to solder a ground wire stub or use a probe tip adapter with a built-in ground contact
- Some probes don't come with a spring-tip accessory. Aftermarket options exist, or you can solder a short (< 10 mm) ground wire directly to the probe's ground barrel

## When the Long Ground Clip Is Fine

Not every measurement needs a short ground. The ringing artifact only matters when the signal has frequency content near the ground lead's resonant frequency. For slow signals, the long clip is perfectly adequate.

### Safe to Use Long Ground Clip

- DC measurements (obviously)
- Audio-frequency signals (< 100 kHz) — resonance at 100 MHz is irrelevant
- Low-frequency ripple measurements (fundamental < 2 MHz, harmonics well below resonance)
- Slow analog signals: temperature, strain, battery voltage
- Any signal where rise time is > 100 ns

### Need Short Ground

- Digital logic with rise times < 20 ns (most modern CMOS)
- Switching converter switch nodes (transitions in single-digit nanoseconds)
- Clock signals where you care about edge quality, jitter, or duty cycle
- Any measurement where you're looking at overshoot, ringing, or settling behavior
- EMI pre-compliance measurements where high-frequency accuracy matters

## Diagnosing the Artifact

**Test:** Switch between long ground clip and spring-tip ground on the same signal

### Procedure

1. Measure the signal with the standard ground clip lead — note any ringing or overshoot
2. Switch to the spring-tip ground (or the shortest ground connection you can manage)
3. Compare the two waveforms:
   - If the ringing disappears or significantly reduces → it was a ground-lead artifact
   - If the ringing remains at the same frequency and amplitude → it's real circuit behavior
   - If the ringing changes frequency → mixed: some is real, some is probe-induced

### What You Learn

- Whether the ringing you see is a measurement artifact or a real circuit problem
- This is one of the most valuable diagnostic techniques for scope measurements

### Gotchas

- Ground-lead ringing looks exactly like real impedance-mismatch ringing. The only way to distinguish them is to change the ground lead and see if the ringing changes. If you only ever use the long ground clip, you'll never know which ringing is real
- The ringing frequency is a clue: if it matches the calculated LC resonance of your ground lead (~100 MHz for a 6" clip), it's probably the probe. If it matches a frequency you can calculate from the circuit's impedance and parasitics, it's probably real
- Even with a spring-tip ground, the probe still adds some capacitance and a small amount of inductance. For the most critical measurements (multi-GHz), active probes with integrated ground contacts are needed
