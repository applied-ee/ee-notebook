---
title: "Impedance & Reactance"
weight: 10
---

# Impedance & Reactance

At DC, resistance tells the whole story: V = IR, and that's it. But as soon as a signal changes over time — a switching edge, an audio waveform, an RF carrier — resistance alone doesn't capture what's happening. Capacitors and inductors push back against changing signals in ways that depend on frequency, and the voltage and current through them aren't in phase anymore. Impedance is the concept that extends Ohm's law to handle all of this.

> For the formulas referenced on this page, see the [Formula Reference appendix]({{< relref "/docs/appendix/formulas" >}}#ac--impedance).

## From Resistance to Impedance

Resistance is opposition to current that dissipates energy as heat. It doesn't depend on frequency — a 1 kΩ resistor is 1 kΩ at DC, at 1 kHz, and at 1 GHz (ignoring parasitics for now).

Reactance is opposition to current that stores and releases energy rather than dissipating it. It's what capacitors and inductors do, and it depends entirely on frequency.

Impedance (Z) combines both into a single quantity. It's the total opposition to current flow in an AC circuit, including both the energy-dissipating part (resistance) and the energy-storing part (reactance).

## Capacitive Reactance

A capacitor opposes changes in voltage. At DC, no current flows through an ideal cap — it's an open wall. At high frequencies, the cap passes current freely and looks nearly like a short. The transition between these extremes is smooth and predictable: double the frequency, and the cap's impedance drops by half. Go up a decade in frequency, and it drops by 10×.

This is why a bypass cap that works great at 1 kHz might be nearly invisible at 1 MHz — its impedance has dropped by three decades. Conversely, a coupling cap that passes audio signals easily will block DC completely.

The current through a capacitor leads the voltage by 90°. When voltage is at zero and rising, current is already at its peak. This phase relationship is one of the signatures that distinguishes reactive behavior from resistive behavior.

## Inductive Reactance

An inductor opposes changes in current. At DC it's just wire — current flows freely. But as frequency increases, the inductor pushes back harder: double the frequency, double the impedance. Go up a decade, and the impedance rises by 10×.

This is the opposite of a capacitor's behavior, and it's why inductors show up in circuits that need to block high-frequency signals while passing DC — like the ferrite beads on power rails or the chokes in EMI filters.

The current through an inductor lags the voltage by 90°. When voltage is applied, current ramps up gradually rather than appearing instantly. Again, this phase shift is the hallmark of reactance rather than resistance.

## Complex Impedance

Impedance combines resistance and reactance into a single complex number. The real part is resistance — energy that gets dissipated as heat. The imaginary part is reactance — energy that gets stored in an electric or magnetic field and then returned to the circuit.

Engineers use j (not i, to avoid confusion with current) as the imaginary unit. A capacitor's impedance is negative imaginary (current leads voltage), an inductor's is positive imaginary (current lags voltage), and a resistor's impedance is purely real (voltage and current are in phase).

What matters practically: the magnitude of impedance is what an impedance analyzer or LCR meter reports when it displays |Z|. It tells you how much the component or circuit opposes current at that frequency. The phase angle tells you where the energy goes — a phase near zero means mostly dissipation, a phase near ±90° means mostly storage. Most real circuits land somewhere in between.

## Series and Parallel Impedance

The same series and parallel rules from DC analysis apply, but with complex numbers. Series impedances add directly. Parallel impedances combine with the reciprocal rule, just like parallel resistors.

A resistor in series with a capacitor is a good example: at low frequencies the capacitor dominates (high impedance, phase near −90°). At high frequencies the resistor dominates (impedance approaches R, phase approaches 0°). This is exactly how an RC filter works — the frequency-dependent impedance division between R and C creates the filtering action.

## Ohm's Law Generalized

Ohm's law still works for AC — you just use impedance instead of resistance. The familiar V = IR becomes V = IZ, where all three quantities are complex numbers (phasors) at a specific frequency. The magnitude relationship gives the amplitude, and the phase relationship gives the timing between voltage and current waveforms.

This generalized form is valid for steady-state sinusoidal signals at a single frequency. For signals with multiple frequency components, each frequency is analyzed separately (superposition applies in linear circuits).

## What "High Impedance" and "Low Impedance" Mean

These terms come up constantly, and their meaning is always relative to what's connected:

- **High impedance input** — Draws very little current from the source. A 1 MΩ oscilloscope input is "high impedance" relative to most circuits. It doesn't load the signal significantly
- **Low impedance output** — Can supply current without significant voltage drop. A power supply with 10 mΩ output impedance is "low impedance" — it maintains its voltage under load
- **Impedance matching** — Making source and load impedance equal (or conjugate). Maximizes power transfer in RF systems. Not the same goal as in power delivery, where low source impedance is preferred
- **Impedance mismatch** — Source and load impedance are very different. In RF, this causes reflections. In audio, it can cause frequency-dependent signal loss

The key insight: impedance is about the *relationship* between source and load. A 50 Ω impedance isn't "high" or "low" in absolute terms — it depends on what it's connected to.

## Tips

- **Use reactance to estimate frequency behavior quickly** — X_C = 1/(2πfC) and X_L = 2πfL are worth memorizing. They let you estimate whether a cap or inductor matters at a given frequency without a calculator
- **Think in decades** — Capacitive reactance drops by 10× for every 10× increase in frequency. Inductive reactance rises by 10× for every 10× increase. This makes quick mental estimates practical
- **Phase tells you where the energy goes** — If voltage and current are in phase, energy is being dissipated (resistance). If they're 90° apart, energy is being stored and returned (reactance). Anything in between is a mix
- **Complex math isn't optional for AC circuits** — Series/parallel combinations of R, C, and L at a given frequency require complex arithmetic. Trying to work with magnitudes alone gives wrong answers because phase matters

## Caveats

- **Impedance is a single-frequency concept** — Z is defined at one frequency. A circuit's impedance at 1 kHz is generally different from its impedance at 1 MHz. Broadband analysis requires evaluating impedance across the frequency range of interest
- **Phasor analysis assumes steady state** — The V = IZ relationship applies to sinusoidal signals that have been running long enough for transients to die out. During turn-on or transient events, time-domain analysis (differential equations or simulation) is needed
- **Reactance doesn't dissipate power** — A purely reactive component (ideal cap or inductor) returns all stored energy to the circuit. Only the resistive part of impedance dissipates power. This is why power factor matters in AC power systems
- **Parasitic elements change the picture at high frequency** — A capacitor's impedance doesn't decrease forever with frequency — eventually ESL takes over and it becomes inductive. Real impedance behavior is covered in [Frequency-Dependent Behavior]({{< relref "frequency-dependent-behavior" >}})

## In Practice

- **An RC circuit that attenuates a signal more than expected at a given frequency** usually means the impedance ratio between R and C isn't what was assumed. Calculate X_C at the actual operating frequency and check the voltage divider ratio — a 100 nF cap that looks like an open circuit at 60 Hz is nearly a short at 10 MHz
- **Unexpectedly large voltage across an inductor** at a switching edge points to high dI/dt through the inductance. The inductive reactance formula gives the steady-state picture, but for transients, V = L(dI/dt) is the relationship to use — and stray inductance in wiring or traces is often the culprit
- **A circuit that behaves differently at different frequencies** even though "nothing changed" is showing impedance effects. The passive components in the circuit have frequency-dependent impedance, and the operating point shifts as frequency changes
