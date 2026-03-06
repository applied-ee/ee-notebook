---
title: "Class D Switching"
weight: 70
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Class D Switching

The radical departure: instead of using output transistors as continuously variable current sources (linear operation), use them as switches — fully on or fully off. Encode the audio signal as a pattern of switching, then filter the output to recover the audio.

## The Core Idea

A PWM (pulse-width modulation) Class D amplifier works in three stages:

### 1. Modulator

The audio input is compared against a high-frequency triangle or sawtooth wave (typically 300kHz-1MHz). When the audio signal is higher than the triangle, the output is high; when it's lower, the output is low. The result is a rectangular wave whose pulse widths encode the audio signal.

- Audio near maximum → wide pulses (long on-time)
- Audio near zero → narrow pulses (short on-time)
- Audio at midpoint → 50% duty cycle

This is analog PWM modulation. The audio information is in the duty cycle variations.

### 2. Switching Output Stage

A half-bridge (two MOSFETs) or full-bridge (four MOSFETs) switches the power supply to the output in time with the PWM signal. The MOSFETs are either fully enhanced (on, R_DS(on) typically 10-50mΩ) or fully off. They spend minimal time in the linear transition region.

Power dissipation in the output devices:

$$P_{conduction} = I_{RMS}^2 \times R_{DS(on)}$$
$$P_{switching} = \frac{1}{2} V_{DD} \times I_{load} \times (t_{rise} + t_{fall}) \times f_{sw}$$

Both terms are small. The conduction loss is low because R_DS(on) is low. The switching loss is low because the transitions are fast (nanoseconds) and occur at the switching frequency, not continuously.

### 3. Output Filter

A low-pass LC filter between the switching stage and the speaker recovers the audio signal. The filter passes audio frequencies (20Hz-20kHz) and attenuates the switching frequency and its harmonics.

Typical filter: a second-order LC with cutoff around 30-50kHz. The inductor is usually 10-33µH and the capacitor 0.47-1µF. The speaker's own inductance sometimes serves as part of the filter.

## What It Solves

**Efficiency above 90%.** In practice, modern Class D amplifiers achieve 85-95% efficiency at full power. A 100W Class D amplifier might dissipate only 5-15W of heat, compared to 50-100W for a Class AB amplifier at the same output power.

**Minimal heatsinking.** Less heat means smaller or no heatsinks. Many Class D amplifiers can run without any heatsink at moderate power levels.

**Compact size.** No large heatsinks, no heavy linear power supply transformer. Class D amplifiers pair naturally with switch-mode power supplies (SMPS), and the entire amplifier can be remarkably small and light.

**Low idle power.** When the audio signal is zero, the output switches at 50% duty cycle with minimal current flowing through the filter. Idle dissipation is very low.

## The New Problems

### Switching Noise and EMI

The output stage generates fast edges (dV/dt of several V/ns) at the switching frequency. These radiate as electromagnetic interference and can couple into sensitive analog circuits. Class D amplifiers require careful PCB layout, shielding, and filtering to meet EMC regulations.

### Output Filter Design

The LC filter is not trivial:
- **Inductor core losses** at the switching frequency reduce efficiency
- **Filter resonance** can cause ringing or oscillation if not properly damped
- **Load dependency** — the filter response changes with speaker impedance, affecting frequency response
- **Inductor saturation** at high currents can cause distortion

### Dead Time Distortion

To prevent both MOSFETs from being on simultaneously (shoot-through), a small dead time is inserted between turn-off of one device and turn-on of the other. During dead time, the current flows through the body diode of one MOSFET, and the output voltage is not what the PWM signal commanded. This is analogous to crossover distortion in Class AB — and it's worst at low signal levels.

Modern designs mitigate dead time distortion with feedback, dead time correction circuits, or self-oscillating modulator topologies that inherently compensate.

### Feedback Complexity

Closing a feedback loop around a switching amplifier is more complex than in a linear amplifier. The output filter introduces phase shift, the PWM modulator adds delay, and the switching itself creates sampling effects. Stability analysis requires techniques from both analog feedback theory and discrete-time control.

Many modern Class D amplifiers use self-oscillating topologies (like sigma-delta modulators) that integrate the modulator and feedback loop into a single system, improving distortion and stability.

## Modern Status

Class D has gone from exotic curiosity to dominant technology in many audio applications:

- **Car audio:** Space and efficiency constraints make Class D the obvious choice
- **Portable speakers and soundbars:** Battery life and thermal constraints
- **Powered studio monitors:** Even in pro audio, Class D is now common for woofer amplification
- **Subwoofers:** Bass frequencies are forgiving of the residual HF artifacts
- **PA and live sound:** High power, light weight, low heat

State-of-the-art Class D amplifiers (such as those using GaN FETs and advanced feedback topologies) now achieve THD+N below 0.001% — matching or exceeding the best linear amplifiers. The gap in measured performance has effectively closed, though debate about subjective sound quality continues in audiophile circles.

The remaining advantage of linear amplifiers is simplicity in the output path: no switching frequency, no output filter, no EMI concerns. For applications where these matter (sensitive low-level circuits, measurement equipment, purist audio), linear amplifiers persist. But for the vast majority of audio power applications, Class D has won the efficiency argument decisively.
