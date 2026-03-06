---
title: "Frequency-Dependent Behavior"
weight: 20
---

# Frequency-Dependent Behavior

Every passive component has an ideal model and a real model, and the gap between them grows with frequency. A resistor isn't purely resistive, a capacitor isn't purely capacitive, and an inductor isn't purely inductive — each one carries parasitic elements that eventually dominate. Understanding where the ideal model breaks down is essential for any circuit that operates above DC.

## Resistors at Frequency

At DC, a resistor is just R. At higher frequencies, two parasitics appear:

- **Parasitic capacitance** — Mostly between the terminals and through the body. Creates a parallel path that bypasses the resistance at high frequency. A 1 MΩ resistor with 1 pF of parasitic capacitance has its impedance dominated by the capacitance above about 160 kHz
- **Parasitic inductance** — From the leads and body. Adds series inductance that increases impedance at very high frequencies. Wire-wound resistors are the worst — they're literally coils

The practical effect: high-value resistors stop being resistive at lower frequencies than low-value ones. A 10 MΩ resistor is essentially a capacitor at RF. Conversely, low-value resistors stay resistive to much higher frequencies but eventually the lead inductance matters.

**Skin effect** also changes things at high frequency. Current crowds toward the surface of conductors, increasing effective resistance. This matters for high-current, high-frequency paths (switching power, RF) but not for typical signal-level resistors.

## Capacitors at Frequency

The ideal capacitor model says impedance decreases forever as frequency rises: X_C = 1/(2πfC). In reality, every capacitor has:

- **ESR (Equivalent Series Resistance)** — Internal resistance from electrode and lead material, electrolyte conductivity (in electrolytics), and dielectric losses. ESR sets a floor on how low the impedance can go
- **ESL (Equivalent Series Inductance)** — Inductance from leads, internal electrodes, and current paths through the capacitor body. Above a certain frequency, the inductance dominates and impedance starts rising again

### Self-Resonant Frequency (SRF)

At the SRF, the capacitive reactance and inductive reactance from ESL are equal and cancel. The impedance reaches its minimum (equal to ESR). Below SRF, the component is capacitive. Above SRF, it's inductive — the "capacitor" is actually behaving as an inductor.

**SRF = 1 / (2π√(L_ESL × C))**

Typical SRF values:
- Large electrolytic (100 µF): SRF might be 10-100 kHz
- 100 nF MLCC (0805 package): SRF around 20-30 MHz
- 100 pF C0G (0402 package): SRF might be 500 MHz to 1 GHz+

### The Impedance Curve

Manufacturers publish impedance vs. frequency plots for capacitors. The characteristic shape is a V (on a log-log plot):

1. **Capacitive region** (left side of V) — Impedance drops at -20 dB/decade as frequency rises
2. **Minimum impedance** (bottom of V) — At or near SRF, impedance equals ESR
3. **Inductive region** (right side of V) — Impedance rises at +20 dB/decade as ESL dominates

Reading these curves is one of the most practical skills for selecting bypass and filter capacitors. A capacitor that's inductive at the frequency of interest isn't doing its job.

### Why "10 µF" Isn't 10 µF Everywhere

Beyond DC bias effects (covered in [Capacitors]({{< relref "../passive-components/capacitors" >}})), the effective impedance of a capacitor changes dramatically with frequency. A 10 µF electrolytic might have minimum impedance around 50 kHz. At 10 MHz, it's fully inductive and essentially useless as a capacitor. This is why decoupling strategies use multiple capacitor values in parallel — each one covers a different frequency range.

## Inductors at Frequency

An ideal inductor has impedance that rises linearly with frequency: X_L = 2πfL. Real inductors have:

- **Distributed capacitance** — Capacitance between adjacent turns of the winding. At low frequencies this is negligible. At high frequencies it creates a parallel path that eventually resonates with the inductance
- **Core losses** — Hysteresis and eddy current losses in the magnetic core increase with frequency, adding to the effective resistance
- **Skin and proximity effects** — At high frequency, current concentrates on the wire surface (skin effect) and is pushed around by fields from adjacent turns (proximity effect), both increasing AC resistance

### Inductor Self-Resonant Frequency

Just like capacitors, inductors have an SRF where the distributed capacitance resonates with the inductance. Below SRF, the component is inductive. Above SRF, it's capacitive.

An inductor used above its SRF isn't an inductor anymore — it's a capacitor. For RF chokes and filter inductors, the SRF must be well above the operating frequency.

### The Inductor Impedance Curve

On a log-log impedance plot:

1. **Inductive region** — Impedance rises at +20 dB/decade
2. **Peak impedance** — At SRF, impedance reaches maximum (parallel resonance — the opposite of a capacitor's minimum)
3. **Capacitive region** — Above SRF, impedance decreases as the distributed capacitance dominates

## Implications for Real Circuits

### Bypassing and Decoupling

Effective decoupling requires capacitors that are actually capacitive at the frequencies of interest. A single capacitor value covers about two decades of frequency effectively. Multi-value bypass networks (e.g., 10 µF + 100 nF + 1 nF) aim to cover the full range, but interactions between the capacitors can create anti-resonances — frequency points where the parallel combination has higher impedance than any individual cap. Careful selection and simulation help, but placement and PCB layout often matter more than exact values.

### Filtering

A filter's real-world response diverges from the ideal as component parasitics kick in. An LC low-pass filter designed for 1 MHz stops working when the capacitor goes inductive above its SRF — the filter might actually pass high frequencies better than the design frequency. Similarly, the inductor's distributed capacitance creates a parallel path that bypasses the inductance at high frequency.

### Signal Integrity

Fast digital edges contain frequency content well above the fundamental clock rate. A bypass cap that's fine for a 10 MHz clock might be useless for the 1 ns edges on that same clock (which contain energy into the GHz range). This is where ESL and package inductance become the dominant design constraints.

## Tips

- **Always check impedance curves, not just capacitance or inductance values** — The impedance vs. frequency plot tells you what the component actually does at your operating frequency. The nominal value alone is misleading
- **Smaller packages have lower ESL and higher SRF** — An 0402 cap reaches higher frequencies than an 0805 of the same value. When high-frequency performance matters, package size is as important as capacitance
- **Parallel capacitors of the same value beat a single larger cap for high-frequency performance** — Two 100 nF caps in parallel have half the ESR and half the ESL of a single one, with the same total capacitance. The SRF stays roughly the same but the impedance minimum is lower
- **Wire-wound resistors are inductors at high frequency** — Use film or thin-film resistors when frequency behavior matters. Carbon composition resistors have lower parasitic inductance than wire-wound

## Caveats

- **Anti-resonance in parallel capacitor networks** — When two capacitors of different values are paralleled, there's a frequency between their SRFs where they can create a high-impedance peak. The larger cap is inductive while the smaller cap is still capacitive, and they form a parallel LC tank. This is real and measurable, though layout parasitics usually dominate in practice
- **Datasheet impedance curves are measured on specific test fixtures** — The actual impedance on a PCB includes pad, via, and trace inductance. The real SRF is lower and the real minimum impedance is higher than the datasheet suggests
- **Temperature changes ESR** — Electrolytic ESR increases dramatically at low temperature. A bypass network that works at room temperature may fail in a cold environment because the electrolytic caps become high-impedance
- **Aging shifts parameters** — Electrolytic caps dry out, increasing ESR over years. Ceramic caps (Class II) lose capacitance logarithmically. Both effects shift the impedance curve

## In Practice

- **A bypass network that works on the bench but fails in production** may have different capacitor brands or lots with different ESR/ESL characteristics. The impedance curves shifted enough to move an anti-resonance onto a critical frequency
- **Ringing on a signal line after a fast edge** often points to a resonance between parasitic inductance and capacitance at the receiver. The ringing frequency tells you the LC product — measure it and work backwards to identify the parasitic elements involved
- **A filter that passes frequencies it should reject** is likely operating above the SRF of its capacitor or above the SRF of its inductor. Check the impedance curves of both components at the problem frequency
- **Switching regulator noise that doesn't respond to adding more output capacitance** may be at a frequency where the added caps are already inductive. Smaller-value, lower-ESL caps (or caps in a smaller package) that are capacitive at the switching frequency are needed instead
