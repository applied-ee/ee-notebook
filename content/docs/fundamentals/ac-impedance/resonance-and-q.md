---
title: "Resonance & Q Factor"
weight: 30
---

# Resonance & Q Factor

Resonance is what happens when capacitive and inductive reactance cancel at a specific frequency. Energy sloshes back and forth between the electric field of the capacitor and the magnetic field of the inductor, and the circuit's behavior at that frequency is dramatically different from its behavior at any other frequency. Resonance is the basis of filters, oscillators, and tuned circuits — and it's also the mechanism behind parasitic ringing and unexpected frequency peaks in circuits that weren't meant to resonate at all.

## LC Resonance

The resonant frequency of an LC circuit is:

**f₀ = 1 / (2π√(LC))**

At this frequency, the capacitive reactance X_C = 1/(2πf₀C) exactly equals the inductive reactance X_L = 2πf₀L. The two reactances cancel, and what's left depends on whether the L and C are in series or parallel.

### Series Resonance

In a series LC circuit, the impedance at resonance drops to its minimum — ideally zero (just the series resistance of the real components). At f₀, the series combination of L and C looks like a short circuit to the driving source (limited only by the resistance in the loop).

- Below f₀: the circuit is capacitive (X_C > X_L, net negative reactance)
- At f₀: purely resistive (reactances cancel, Z = R_series)
- Above f₀: the circuit is inductive (X_L > X_C, net positive reactance)

Series resonance creates an impedance minimum. It's used in series-tuned filters (bandpass) and is the mechanism behind crystal oscillator operation at the series resonant frequency.

### Parallel Resonance

In a parallel LC circuit, the impedance at resonance rises to its maximum. At f₀, the parallel combination of L and C looks like an open circuit (ideally infinite impedance, limited by losses).

- Below f₀: the circuit is inductive (inductor has lower impedance, dominates the parallel combination)
- At f₀: purely resistive (reactances cancel, Z = R_parallel, which can be very high)
- Above f₀: the circuit is capacitive (capacitor has lower impedance, dominates)

Parallel resonance creates an impedance maximum. It's the basis of tank circuits in RF amplifiers and oscillators, and it's what causes the impedance peak at an inductor's self-resonant frequency.

### The Symmetry

Series resonance: impedance minimum, current maximum. Parallel resonance: impedance maximum, current minimum (from the external source — internally, large circulating currents flow between L and C). Both occur at the same frequency f₀ = 1/(2π√LC) for the same L and C values.

## Quality Factor (Q)

Q measures how "sharp" the resonance is — how much energy is stored vs. how much is lost per cycle. A high-Q circuit resonates strongly at f₀ and attenuates quickly away from it. A low-Q circuit has a broad, gentle resonance.

**Q = 2π × (energy stored per cycle) / (energy dissipated per cycle)**

For a series RLC circuit:

**Q = (1/R) × √(L/C) = X_L/R = X_C/R** (all evaluated at f₀)

For a parallel RLC circuit:

**Q = R × √(C/L) = R/X_L = R/X_C** (all evaluated at f₀)

Note the inversion: in a series circuit, lower R means higher Q (less loss). In a parallel circuit, higher R means higher Q (less loss through the parallel resistance).

### Q and Bandwidth

The bandwidth of a resonant circuit — the range of frequencies where the response is within 3 dB of the peak — is directly related to Q:

**BW = f₀ / Q**

A resonant circuit at 1 MHz with Q = 100 has a 3 dB bandwidth of 10 kHz. The higher the Q, the narrower the bandwidth, and the sharper the frequency selectivity.

### What Q Values Mean in Practice

- **Q < 1** — Overdamped. No real resonance, just a gentle roll-off. This is what many power supply filters look like intentionally
- **Q = 0.5 to 1** — Critically damped to slightly underdamped. Often the design target for maximally flat (Butterworth) filter responses
- **Q = 5 to 50** — Moderate Q. Typical of practical LC filters, tuned amplifier stages, and IF transformers
- **Q = 100 to 1000+** — High Q. Quartz crystals, ceramic resonators, cavity resonators. Very narrow bandwidth and sharp frequency selection
- **Q > 10,000** — Extremely high Q. Found in high-quality quartz crystals used for frequency standards and precision oscillators

## Loaded vs. Unloaded Q

The **unloaded Q** (Q_U) of a resonant circuit is its Q with no external load — determined only by the internal losses of the L and C (mostly the inductor's resistance and core losses).

The **loaded Q** (Q_L) includes the effect of whatever is connected to the resonant circuit. External loads provide an additional path for energy to leave the resonant circuit, which always reduces Q:

**1/Q_L = 1/Q_U + 1/Q_external**

This matters a lot in practice. A high-Q inductor in a tank circuit might have Q_U of 200, but once coupled to a low-impedance load, Q_L drops to 20. The selectivity that looked great on the bench with nothing connected degrades when the circuit is put to use.

## Where Resonance Shows Up

### Intentional Resonance

- **Bandpass filters** — Series or parallel LC circuits tuned to pass a desired frequency range. The Q determines the bandwidth
- **Oscillators** — LC tank circuits, crystal oscillators, ceramic resonator circuits. The resonant element determines the oscillation frequency and Q determines frequency stability
- **Impedance matching networks** — L-networks, pi-networks, and T-networks use resonant or near-resonant structures to transform impedance at a specific frequency
- **Tuned amplifiers** — RF amplifiers with tank circuit loads that amplify only near the resonant frequency

### Unintentional Resonance

- **Parasitic ringing** — Stray inductance (trace, lead, wiring) resonating with stray or intentional capacitance. Shows up as damped oscillation after switching edges
- **Component self-resonance** — Every real capacitor and inductor has an SRF where it stops being what it's supposed to be. This is LC resonance between the component's intended element and its dominant parasitic
- **PCB resonances** — Power plane pairs, long traces, and connector pin inductances form resonant structures at frequencies determined by their geometry
- **Cable resonances** — Unterminated cables are transmission line stubs that resonate at frequencies related to their length

### Crystal Operation

Quartz crystals are mechanical resonators with extremely high Q (typically 10,000 to 100,000+). They have both a series and parallel resonant frequency very close together. The series resonant frequency is where the crystal impedance is minimum (acts like a very low resistance). The parallel resonant frequency — slightly higher — is where the impedance is maximum. Crystal oscillator circuits are designed to operate at one or the other, and the crystal specification (series vs. parallel cut) must match the circuit type.

## Why Every Real Circuit Has Unintended Resonances

Any time there's inductance and capacitance in proximity, there's a potential resonance. In real circuits:

- Every wire and PCB trace has inductance
- Every pair of conductors has capacitance
- Every component has parasitic L and C

The result: unintended LC combinations exist everywhere. Most are at frequencies far from the operating range and cause no trouble. But when a parasitic resonance lands near a clock frequency, switching frequency, or signal bandwidth, it can amplify noise, cause ringing, or create unexpected frequency peaks.

Identifying and damping unwanted resonances is a recurring theme in EMC work, power integrity, and high-speed digital design.

## Tips

- **Use f₀ = 1/(2π√LC) as a diagnostic tool** — When you see ringing on a scope, measure the frequency. Then work backwards to figure out what L and C are resonating. The frequency often points directly to the culprit
- **Damping resistors kill unwanted resonance** — A small resistor in series with an inductor (or parallel with a capacitor) lowers Q and suppresses ringing. The tradeoff is reduced filter performance or increased losses, but in many cases a controlled low-Q response is better than an unpredictable high-Q one
- **Check component SRF against operating frequency** — If a capacitor's SRF is below the operating frequency, it's an inductor at that frequency. If an inductor's SRF is below the operating frequency, it's a capacitor. Neither is doing its intended job
- **Higher Q means more sensitivity to component tolerance** — A high-Q filter needs tighter component values. A 5% tolerance on L or C shifts f₀ by up to 2.5%, which might be a large fraction of the bandwidth if Q is high

## Caveats

- **Resonance can amplify voltage or current beyond supply levels** — In a series RLC circuit at resonance, the voltage across L or C individually can be Q times the source voltage. A Q of 50 means the cap might see 50× the input voltage. This is real and can damage components
- **Parallel resonant impedance is not infinite** — It's limited by losses (mostly inductor resistance and core losses). In practice, the impedance peak at parallel resonance is Q × X_L, which can be very high but is always finite
- **Q is frequency-dependent in real components** — Inductor Q varies with frequency due to skin effect, proximity effect, and core losses. The Q at the designed resonant frequency might be quite different from the Q measured at a test frequency. Always check Q at the actual operating frequency
- **Series and parallel resonant frequencies aren't exactly equal in real circuits** — Losses and parasitic elements shift them slightly apart. For most practical purposes they're close enough to treat as equal, but in high-Q crystal circuits the difference matters and is specified separately

## In Practice

- **Damped ringing after a switching edge** is the signature of a parasitic resonance. The ring frequency identifies the LC product, and the decay rate indicates Q. More cycles before decay = higher Q = less damping
- **An LC filter with a sharp peak in its response near the cutoff frequency** has higher Q than intended. Adding series resistance to the inductor (or choosing an inductor with higher DCR) flattens the response at the cost of insertion loss
- **A circuit that oscillates at a frequency unrelated to any clock or signal** is likely hitting a parasitic resonance. Check for resonances between bypass cap ESL and nearby capacitance, or between trace inductance and load capacitance
- **Crystal oscillator that starts up slowly or not at all** may have excessive loading that kills the Q below what the oscillator circuit needs to sustain oscillation. Check that the load capacitance matches the crystal specification
