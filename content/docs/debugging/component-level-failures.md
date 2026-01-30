---
title: "Component-Level Failures"
weight: 40
---

# Component-Level Failures

Once fault isolation points to a specific area of the circuit, the next step is identifying which component failed and how. Each component type has characteristic failure modes — knowing them narrows the search.

## Electrolytic Capacitors

The most failure-prone passive component. Electrolytics have a liquid electrolyte that dries out over time, and the failure rate increases dramatically with temperature.

**Failure modes:**
- **ESR rise** — The electrolyte dries out, series resistance increases. The cap still reads correct capacitance on a basic meter, but it can't deliver current fast enough. Ripple increases, decoupling degrades
- **Capacitance loss** — Advanced dry-out. The cap loses significant capacitance
- **Short circuit** — Dielectric breakdown, often from overvoltage or reverse polarity. Can be catastrophic (venting, leaking, sometimes popping)
- **Leaking electrolyte** — Visible: brown/orange residue around the base or on the PCB. Corrosive — damages nearby traces and components

**Visual signs:** Bulging top vent, leaking residue, discolored PCB underneath, cap sitting crooked (pushed up by internal pressure).

**Testing:** Basic capacitance meter won't catch the most common failure (ESR rise). You need an ESR meter or an LCR meter that reports ESR at the right frequency. See [ESR, Leakage & Gain]({{< relref "/docs/measurement/component-testing/esr-leakage-gain" >}})

**Rule of thumb:** In consumer electronics more than 5–10 years old, electrolytics near heat sources (voltage regulators, power transistors) are the first suspect.

## Ceramic Capacitors

Usually reliable, but they have one spectacular failure mode.

**Cracked ceramic → dead short:** Mechanical stress (board flex, thermal shock, rough handling) cracks the ceramic body. If the crack bridges the internal electrode layers, the cap becomes a short circuit. This is a hard fault — the rail drops to near zero and something upstream (fuse, regulator) takes the hit.

**How to find it:** If a rail is shorted, disconnect or remove ceramic caps one at a time (starting near the fault area) and recheck. The short clears when you remove the cracked cap. Visually, cracked ceramics sometimes show a hairline crack on the top surface, but often the crack is hidden underneath.

**Other modes:** Ceramic caps can also lose capacitance over time (especially high-K dielectrics like X5R/X7R at high DC bias), but this is a design issue, not a field failure.

## Voltage Regulators

Linear and switching regulators are common failure points because they handle the full load current and dissipate power.

| Symptom | Likely cause |
|---------|-------------|
| No output voltage | Input missing, enable pin wrong state, regulator dead |
| Output low (e.g., 2.8 V instead of 3.3 V) | Overcurrent (regulator in current limit), dropout (input too close to output for LDO), damaged regulator |
| Output oscillating | Missing or wrong output capacitor, ESR too high (LDO stability), layout problem |
| Output correct but noisy | Switching noise (normal for DC-DC, but too much), input noise coupling through |
| Regulator extremely hot | Excessive load, shorted output, excessive dropout dissipation |

**Check first:** Input voltage at the regulator pin (not just "somewhere on the board"). Enable/shutdown pin state. Output voltage at the regulator pin and at the load. Current draw.

## Transistors (BJT and MOSFET)

**BJT failures:**
- **Shorted C-E or C-B** — Often from overcurrent or avalanche breakdown. The transistor becomes a low-resistance path. Diode-check reads near zero in both directions across the shorted junction
- **Open** — The bond wire or die cracks. Diode-check reads open on all junctions

**MOSFET failures:**
- **Gate oxide breakdown** — The gate is destroyed by overvoltage (ESD or exceeding Vgs max). The FET may short drain-to-source or become unpredictable
- **Drain-source short** — Common in power MOSFETs from overcurrent or avalanche energy. The FET is now a wire
- **Increased Rds(on)** — Partial damage. The FET still switches but dissipates more power. Runs hotter than expected

**Testing:** Diode check across all terminals (gate-source, gate-drain, drain-source in both directions). Compare readings to what you expect from the device. See [Dead or Alive?]({{< relref "/docs/measurement/component-testing/dead-or-alive" >}})

## Resistors

Resistors are generally reliable, but they fail under stress.

- **Open circuit** — From sustained overcurrent. The resistive element burns through. Visually: discolored, charred, or cracked body. Sometimes a fuse resistor is designed to fail this way
- **Value drift** — From overheating (not enough to open, but enough to change the material). The resistance shifts, changing bias points, voltage dividers, or timing circuits
- **Solder joint failure** — The resistor is fine, but the connection to the board isn't. Common with through-hole resistors in high-vibration environments

**The sneaky one:** A resistor that's drifted 20% doesn't look broken, and a quick resistance measurement might seem "close enough." Compare to the marked value (color code or package markings) and to the schematic.

## Connectors

Connectors are mechanical components, and mechanical components wear.

- **Corroded pins** — Green or white deposits. Increases contact resistance. Clean with contact cleaner and a brush, or replace
- **Backed-out terminals** — The terminal isn't fully seated in the housing. Apply gentle tension to each wire — a backed-out terminal pulls free
- **Worn springs** — The contact spring has lost tension from many mating cycles. The connector mates but the contact is intermittent. Replacement is the only fix
- **Cracked solder joints** — Board-mount connectors, especially ones that see mechanical stress from cable insertion/removal. Reflow or replace

## ICs

IC failures are often diagnosed by exclusion: verify everything around the IC (all supply pins, all input signals, clock, reset, enable) and if the outputs are still wrong, the IC itself is suspect.

**Approach:**
1. Check all supply pins — correct voltage, decoupled, no excessive ripple
2. Check all control inputs — reset, enable, chip-select at correct levels
3. Check clock (if applicable) — correct frequency, clean edges
4. Check data/signal inputs — correct levels, timing, protocol
5. If all inputs are correct and outputs are wrong → suspect the IC

**Don't jump to "bad IC" too quickly.** An IC with incorrect inputs will produce incorrect outputs, and the problem is upstream, not in the IC. Most "bad IC" diagnoses in hobbyist debugging turn out to be bad power, missing pull-ups, or wrong configuration.

## Failure Mode Quick Reference

| Component | Common failure | How it presents | Quick test |
|-----------|---------------|-----------------|------------|
| Electrolytic cap | ESR rise / dry-out | Increased ripple, poor decoupling | ESR meter |
| Ceramic cap | Cracked → short | Rail drops to zero, upstream protection trips | Remove and check for short |
| Voltage regulator | Overcurrent / dead | Wrong output voltage or no output | Measure in/out/enable |
| BJT | Shorted junction | Circuit stuck high or low | Diode check all junctions |
| MOSFET | Gate oxide / D-S short | Won't switch, always on | Diode check, gate threshold |
| Resistor | Open from overcurrent | Circuit open, looks charred | Resistance measurement |
| Connector | Corroded / worn | Intermittent, position-sensitive | Wiggle test, continuity |
| IC | Various | Wrong outputs despite correct inputs | Verify all inputs first |
