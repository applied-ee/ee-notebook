---
title: "Resonance & Q Factor"
weight: 30
---

# Resonance & Q Factor

Resonance is what happens when capacitive and inductive reactance cancel at a specific frequency. Energy sloshes back and forth between the electric field of the capacitor and the magnetic field of the inductor, and the circuit's behavior at that frequency is dramatically different from its behavior at any other frequency. Resonance is the basis of filters, oscillators, and tuned circuits — and it's also the mechanism behind parasitic ringing and unexpected frequency peaks in circuits that weren't meant to resonate at all.

> [!NOTE]
> All resonance and Q formulas on this page are collected in the [Formula Reference]({{< relref "/docs/appendix/formulas" >}}) appendix for quick lookup.

## LC Resonance

At one specific frequency, an inductor's rising reactance and a capacitor's falling reactance meet and exactly cancel. All that remains is resistance — the reactive parts add to zero, and the circuit behaves as though the L and C aren't there. That frequency is the resonant frequency.

The expression comes from setting X_L = X_C — that is, 2πfL = 1/(2πfC) — and solving for f:

**f₀ = 1 / (2π√(LC))**

At f₀, the two reactances are equal and opposite. What happens next depends on whether the L and C are in series or parallel.

### Series Resonance

{{< circuit caption="Series RLC circuit — at resonance, impedance drops to R" >}}
<svg viewBox="0 0 380 160" width="380" height="160">
  <style>
    .wire { stroke: currentColor; stroke-width: 2; fill: none; }
    .comp { stroke: currentColor; stroke-width: 2; fill: none; }
    .label { font-family: 'Georgia', serif; font-size: 14px; fill: currentColor; text-anchor: middle; }
    .node-dot { fill: currentColor; }
  </style>
  <!-- Left vertical wire (input side) -->
  <circle cx="30" cy="30" r="3" class="node-dot"/>
  <line x1="30" y1="30" x2="30" y2="130" class="wire"/>
  <circle cx="30" cy="130" r="3" class="node-dot"/>
  <!-- Top wire: left corner to R -->
  <line x1="30" y1="30" x2="70" y2="30" class="wire"/>
  <!-- Resistor (zigzag) -->
  <polyline points="70,30 78,18 86,42 94,18 102,42 110,18 118,42 126,30" class="comp"/>
  <text x="98" y="12" class="label">R</text>
  <!-- Wire R to L -->
  <line x1="126" y1="30" x2="170" y2="30" class="wire"/>
  <!-- Inductor (humps) -->
  <path d="M170,30 C170,16 182,16 182,30 C182,16 194,16 194,30 C194,16 206,16 206,30 C206,16 218,16 218,30" class="comp"/>
  <text x="194" y="12" class="label">L</text>
  <!-- Wire L to C -->
  <line x1="218" y1="30" x2="270" y2="30" class="wire"/>
  <!-- Capacitor (parallel plates) -->
  <line x1="270" y1="15" x2="270" y2="45" class="comp" stroke-width="2.5"/>
  <line x1="282" y1="15" x2="282" y2="45" class="comp" stroke-width="2.5"/>
  <text x="276" y="8" class="label">C</text>
  <!-- Wire C to right corner -->
  <line x1="282" y1="30" x2="340" y2="30" class="wire"/>
  <!-- Right vertical wire -->
  <circle cx="340" cy="30" r="3" class="node-dot"/>
  <line x1="340" y1="30" x2="340" y2="130" class="wire"/>
  <circle cx="340" cy="130" r="3" class="node-dot"/>
  <!-- Bottom return wire -->
  <line x1="30" y1="130" x2="340" y2="130" class="wire"/>
  <!-- V_in label on left side -->
  <text x="16" y="68" class="label" text-anchor="end" font-size="12">V<tspan dy="4" font-size="10">in</tspan></text>
  <line x1="24" y1="52" x2="24" y2="62" class="wire" stroke-width="1"/>
  <line x1="20" y1="62" x2="28" y2="62" class="wire" stroke-width="1"/>
  <!-- Ground on bottom wire -->
  <line x1="185" y1="130" x2="185" y2="138" class="wire"/>
  <line x1="175" y1="138" x2="195" y2="138" class="wire" stroke-width="2"/>
  <line x1="179" y1="143" x2="191" y2="143" class="wire" stroke-width="2"/>
  <line x1="183" y1="148" x2="187" y2="148" class="wire" stroke-width="2"/>
</svg>
{{< /circuit >}}

In a series LC circuit, the impedance at resonance drops to its minimum — ideally zero (just the series resistance of the real components). At f₀, the series combination of L and C looks like a short circuit to the driving source (limited only by the resistance in the loop).

- Below f₀: the circuit is capacitive (X_C > X_L, net negative reactance)
- At f₀: purely resistive (reactances cancel, Z = R_series)
- Above f₀: the circuit is inductive (X_L > X_C, net positive reactance)

Series resonance creates an impedance minimum. It's used in series-tuned filters (bandpass) and is the mechanism behind crystal oscillator operation at the series resonant frequency.

### Parallel Resonance

{{< circuit caption="Parallel RLC circuit — at resonance, impedance rises to R" >}}
<svg viewBox="0 0 360 260" width="360" height="260">
  <style>
    .wire { stroke: currentColor; stroke-width: 2; fill: none; }
    .comp { stroke: currentColor; stroke-width: 2; fill: none; }
    .label { font-family: 'Georgia', serif; font-size: 14px; fill: currentColor; text-anchor: middle; }
    .node-dot { fill: currentColor; }
  </style>
  <!-- Top rail -->
  <circle cx="30" cy="30" r="3" class="node-dot"/>
  <line x1="30" y1="30" x2="310" y2="30" class="wire"/>
  <!-- Junction dots -->
  <circle cx="100" cy="30" r="3" class="node-dot"/>
  <circle cx="190" cy="30" r="3" class="node-dot"/>
  <circle cx="280" cy="30" r="3" class="node-dot"/>
  <!-- Bottom rail -->
  <circle cx="30" cy="230" r="3" class="node-dot"/>
  <line x1="30" y1="230" x2="310" y2="230" class="wire"/>
  <circle cx="100" cy="230" r="3" class="node-dot"/>
  <circle cx="190" cy="230" r="3" class="node-dot"/>
  <circle cx="280" cy="230" r="3" class="node-dot"/>
  <!-- R branch (left) -->
  <line x1="100" y1="30" x2="100" y2="85" class="wire"/>
  <polyline points="100,85 88,93 112,101 88,109 112,117 88,125 112,133 100,141" class="comp"/>
  <line x1="100" y1="141" x2="100" y2="230" class="wire"/>
  <text x="122" y="118" class="label" text-anchor="start">R</text>
  <!-- L branch (center) -->
  <line x1="190" y1="30" x2="190" y2="85" class="wire"/>
  <path d="M190,85 C204,85 204,97 190,97 C204,97 204,109 190,109 C204,109 204,121 190,121 C204,121 204,133 190,133" class="comp"/>
  <line x1="190" y1="133" x2="190" y2="230" class="wire"/>
  <text x="212" y="114" class="label" text-anchor="start">L</text>
  <!-- C branch (right) -->
  <line x1="280" y1="30" x2="280" y2="115" class="wire"/>
  <line x1="265" y1="115" x2="295" y2="115" class="comp" stroke-width="2.5"/>
  <line x1="265" y1="127" x2="295" y2="127" class="comp" stroke-width="2.5"/>
  <line x1="280" y1="127" x2="280" y2="230" class="wire"/>
  <text x="302" y="125" class="label" text-anchor="start">C</text>
  <!-- Ground -->
  <line x1="160" y1="230" x2="160" y2="238" class="wire"/>
  <line x1="150" y1="238" x2="170" y2="238" class="wire" stroke-width="2"/>
  <line x1="154" y1="243" x2="166" y2="243" class="wire" stroke-width="2"/>
  <line x1="158" y1="248" x2="162" y2="248" class="wire" stroke-width="2"/>
</svg>
{{< /circuit >}}

In a parallel LC circuit, the impedance at resonance rises to its maximum. At f₀, the parallel combination of L and C looks like an open circuit (ideally infinite impedance, limited by losses).

- Below f₀: the circuit is inductive (inductor has lower impedance, dominates the parallel combination)
- At f₀: purely resistive (reactances cancel, Z = R_parallel, which can be very high)
- Above f₀: the circuit is capacitive (capacitor has lower impedance, dominates)

Parallel resonance creates an impedance maximum. It's the basis of tank circuits in RF amplifiers and oscillators, and it's what causes the impedance peak at an inductor's self-resonant frequency.

### The Symmetry

Series resonance: impedance minimum, current maximum. Parallel resonance: impedance maximum, current minimum (from the external source — internally, large circulating currents flow between L and C). Both occur at the same frequency f₀ = 1/(2π√LC) for the same L and C values.

## Quality Factor (Q)

Q measures how "sharp" the resonance is — the ratio of energy stored to energy lost per cycle. A high-Q circuit resonates strongly at f₀ and attenuates quickly away from it. A low-Q circuit has a broad, gentle resonance.

**Q = 2π × (energy stored per cycle) / (energy dissipated per cycle)**

The key to understanding Q in circuits is *where* the resistance sits relative to the energy circulation path between L and C. In a series RLC circuit, the resistance is *in* the circulation path — current flowing between L and C must pass through R, so lower R means less loss and higher Q. In a parallel RLC circuit, the resistance *shunts* energy out of the tank — current leaks through R instead of circulating between L and C, so higher R means less leakage and higher Q. This inversion is why the series and parallel Q formulas look reciprocal:

For a series RLC circuit:

**Q = (1/R) × √(L/C) = X_L/R = X_C/R** (all evaluated at f₀)

For a parallel RLC circuit:

**Q = R × √(C/L) = R/X_L = R/X_C** (all evaluated at f₀)

### Q and Bandwidth

A high-Q circuit is picky about frequency — it responds strongly right at resonance and barely at all a little ways away. The bandwidth of a resonant circuit — the range of frequencies where the response is within 3 dB of the peak — quantifies exactly how picky:

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
