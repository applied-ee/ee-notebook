---
title: "Does Shielding / Rerouting Fix It?"
weight: 40
---

# Does Shielding / Rerouting Fix It?

Empirical fixes and verification. You've identified the noise and the coupling path — now you try a fix and measure whether it actually works. Shielding, filtering, rerouting, and grounding changes are the main tools. The key is measuring before and after, changing one thing at a time, and not assuming something worked because it "should" work.

## Shielding Experiments

**Tool:** Copper foil, aluminum foil, mu-metal sheet, or improvised shields + oscilloscope
**When:** Testing whether a physical barrier reduces radiated or near-field coupling

### Copper / Aluminum Foil Test

1. Measure the noise level baseline
2. Place a grounded piece of copper or aluminum foil between the suspected source and victim
3. Re-measure — compare to baseline
4. If noise drops significantly, a permanent shield (enclosure, shield can, copper pour on PCB) will help

### What Different Shield Materials Do

| Material | Effective against | Not effective against |
|----------|------------------|---------------------|
| Copper foil | Electric fields (capacitive coupling), high-frequency magnetic fields (> 100 kHz via eddy currents) | Low-frequency magnetic fields (50/60 Hz) |
| Aluminum foil | Same as copper (slightly less conductive) | Low-frequency magnetic fields |
| Mu-metal | Low-frequency magnetic fields (50/60 Hz transformer hum) | Less effective at high frequency; expensive |
| Steel enclosure | Broad spectrum — both electric and moderate magnetic shielding | Very low frequency magnetic fields may still penetrate thin steel |

### Procedure for PCB Shield Cans

1. Identify the noisy component or area (near-field probing helps)
2. Improvise a temporary shield: solder a piece of copper tape over the area, grounded to the ground plane
3. Measure the improvement
4. If effective, design a proper shield can into the PCB (ground pad footprint + snap-on shield)

### Gotchas

- A shield must be grounded to work against electric fields. An ungrounded piece of foil does almost nothing — it becomes a floating conductor that re-radiates
- Gaps and seams in a shield leak. A shield with a 1 cm gap provides much less attenuation than a complete enclosure. At high frequencies, any gap comparable to a wavelength is effectively open
- Cables that penetrate a shield carry noise through the shield boundary. Every cable entering a shielded enclosure should be filtered at the entry point (feedthrough capacitor, filtered connector, or ferrite)

## Filtering Experiments

**Tool:** Capacitors, ferrite beads, LC filters + oscilloscope
**When:** Testing whether a filter on a power rail or signal line reduces conducted noise

### Decoupling Capacitor Test

1. Measure noise on the power rail at the victim device
2. Add a ceramic capacitor (100 nF is a good starting point) from VCC to GND, as close to the victim IC as possible
3. Re-measure — compare to baseline
4. If the noise is at a specific frequency, choose the capacitor value for lowest impedance at that frequency:
   - 100 nF: low impedance around 5–10 MHz
   - 10 nF: low impedance around 15–30 MHz
   - 1 µF: low impedance around 1–3 MHz

### Ferrite Bead Test

1. Insert a ferrite bead in series with the power rail feeding the victim device (between the main rail and the local bypass cap)
2. The ferrite bead attenuates high-frequency noise while passing DC
3. Re-measure — the ferrite + bypass cap forms an LC low-pass filter

### Series Resistor on Signal Lines

1. Add a small series resistor (22–100 Ohm) in a signal line that's picking up noise
2. Combined with the input capacitance of the receiver, this forms a low-pass filter
3. The filter reduces high-frequency noise at the cost of slowing the signal edge
4. Only appropriate if the signal can tolerate the slowed edge and the voltage drop

### What You Learn

- Whether filtering at the victim reduces the noise to acceptable levels
- Which filter value and topology works best for this specific noise frequency

### Gotchas

- Capacitors have parasitic inductance (ESL). Above their self-resonant frequency, they become inductive and stop filtering. Use multiple capacitors of different values in parallel for broadband filtering (e.g., 1 µF + 100 nF + 1 nF)
- Ferrite beads saturate at high DC current — their impedance drops dramatically when carrying current near their rated limit. Check the DC current vs. impedance curve in the datasheet
- Adding filter capacitors to voltage regulator outputs can affect stability. Check the regulator's datasheet for output capacitor requirements and stability criteria

## Rerouting and Layout Changes

**Tool:** PCB rework (cut traces, add jumper wires) + oscilloscope
**When:** Testing whether a layout change reduces coupling

### Trace Rerouting Test

1. Identify the aggressor trace (switching node, clock, high-current pulse) and victim trace (sensitive analog, ADC input, reference)
2. Cut the victim trace and reroute it with a jumper wire that increases distance from the aggressor, or takes a different path
3. Re-measure noise on the victim
4. If improved, incorporate the new routing into the next PCB revision

### Ground Plane Experiments

1. If the PCB has a split or slotted ground plane, try bridging the gap with copper tape or a jumper wire
2. Slots and gaps in the ground plane force return current to detour, enlarging the current loop and increasing both radiation and coupling
3. Bridging the gap often dramatically reduces noise that was caused by return current detours

### What You Learn

- Whether proximity coupling is the dominant mechanism
- How sensitive the circuit is to trace routing — which informs future layout decisions

### Gotchas

- Jumper wires have different impedance than PCB traces. For high-frequency signals, a jumper wire can make signal integrity worse even while improving noise coupling. Use jumper wires for validation, not as permanent fixes
- Moving one trace may improve one coupling path and worsen another. Always measure, don't assume
- Ground plane modifications change return current paths for all signals, not just the one you're fixing. Verify that other signals haven't degraded after the change

## Grounding Changes

**Tool:** Wire, copper tape, rework station + oscilloscope
**When:** Testing whether a grounding improvement reduces noise

### Common Grounding Experiments

| Experiment | What it tests |
|-----------|--------------|
| Connect a dedicated ground wire from victim to source ground | Whether the existing ground path has too much impedance or is shared with noisy current |
| Star-ground: route all ground returns to one point | Whether ground loop currents between stages are the problem |
| Add a ground pour around a sensitive signal | Whether a local ground shield reduces capacitive coupling |
| Bond the PCB ground to the enclosure | Whether a connection to the enclosure ground reduces radiated pickup |

### Gotchas

- Adding ground connections can create new ground loops if not done carefully. A dedicated ground wire that creates a parallel path to an existing ground can make things worse
- Ground planes should be continuous under signal traces. A ground pour with many gaps and islands can be worse than no pour because current has to find tortuous paths
- "Better grounding" is the most common first instinct and the most commonly misapplied fix. Identify the actual coupling mechanism before adding ground wires — sometimes the problem isn't grounding at all
