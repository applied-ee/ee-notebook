---
title: "Aging, Drift, and Failure Modes"
weight: 60
bookCollapseSection: true
---

# Aging, Drift, and Failure Modes

Components change over time, and older circuits have had more time to change. These aren't random failures — they're predictable, era-specific degradation patterns. A 1970s amplifier with carbon composition resistors will have different drift behavior than a 1990s design with metal film parts. Knowing which components in a given era are likely to have drifted or failed narrows the search space dramatically. For general failure analysis methodology, see [Debugging, Failure & Repair]({{< relref "/docs/debugging" >}}) — this section focuses on the aging signatures specific to legacy components.

## Electrolytic Capacitors

The single most common failure mode in vintage electronics. Aluminum electrolytic capacitors have a liquid electrolyte that evaporates over time — faster at higher temperatures, under higher ripple current, or when the cap sits unused for years (the oxide layer degrades without a forming voltage).

**Failure modes:**
- **Capacitance loss** — The electrolyte dries out, reducing capacitance. A 1000 µF filter cap might measure 200 µF after 30 years. The circuit still works, but ripple increases, voltage sags under load, and downstream regulators work harder
- **ESR increase** — Often more impactful than capacitance loss. A cap that still measures near its rated capacitance on a basic meter might have ESR ten times the original value. High ESR means the cap can't supply fast transient current, causing voltage droop on digital switching edges and instability in regulators that depend on output cap ESR for loop stability
- **Leakage current increase** — The aluminum oxide dielectric degrades without voltage applied. A cap that's been sitting unpowered for years may draw excessive leakage current when first powered up. In severe cases, this can overheat the cap or overload the power supply
- **Outright failure** — Electrolyte leaks out (visible as crust or corrosion on the board around the cap leads), vent opens (bulging or popped top), or internal open circuit

**What to look for:** Bulging tops or bottoms, electrolyte residue on the board (brown or white crust around the leads), discoloration of the PCB near the cap, or any cap that's physically deformed. But bad caps often look fine visually — ESR measurement with a dedicated ESR meter or an LCR meter at 100 kHz is the reliable test.

**The recapping question:** Wholesale replacement of all electrolytics ("recapping") is common in vintage audio and computer restoration. It's not always necessary — test first. But for power supply filter caps and any electrolytic in a timing or coupling role, proactive replacement is often justified if the equipment needs to be reliable.

## Resistors

**Carbon composition** — The standard resistor type through the 1960s and into the 1970s. Carbon comp resistors absorb moisture over decades, causing their resistance to increase — often 10–20% above nominal, sometimes more. They also become noisy (excess noise above thermal noise) as the carbon granule structure degrades. If you measure carbon comp resistors in a vintage circuit and they're all reading high, this is expected age-related drift, not a circuit design error.

**Carbon film** — More stable than carbon comp but still susceptible to moisture absorption and value drift over decades. Common in 1970s through 1990s consumer electronics.

**Metal film** — Excellent long-term stability. Drift is typically less than 0.1% per year. Equipment designed with metal film resistors from the 1980s onward will have resistors that are still within spec unless physically damaged.

**Wirewound** — Used for power applications. Very stable over time. Failure mode is open circuit from thermal stress (the wire breaks) or insulation breakdown in high-voltage applications.

## Capacitors (Non-Electrolytic)

**Paper and paper-in-oil** — Found in equipment from the 1940s through the 1960s. Paper capacitors absorb moisture and develop leakage current over time. Wax-sealed paper caps are almost guaranteed to be leaky after 50+ years. These should be replaced on sight in any circuit where leakage matters (coupling caps, timing circuits, anything that blocks DC).

**Early ceramic disc** — Older ceramic capacitors (pre-1980s, especially non-NPO/C0G types) can change value significantly with age. The ceramic dielectric undergoes aging that reduces capacitance logarithmically over time. Heating the cap above its Curie temperature resets the aging clock.

**Film capacitors** — Polyester (Mylar) and polypropylene film caps are very stable over time. These are generally safe to leave in place unless physically damaged. They're often found in audio signal paths, timing circuits, and filter applications in equipment from the 1970s onward.

**Tantalum** — Tantalum capacitors can fail short circuit with little warning, especially if operated near their voltage rating or subjected to voltage surges. A tantalum that has been in service for decades at a safe derating may be fine, but one that has been running near its limit is a time bomb.

## Solder Joints

**Cold joints from the factory** — Wave soldering and hand soldering from earlier decades had higher defect rates than modern reflow processes. A joint that worked for years might finally fail as thermal cycling and vibration propagate a crack that was present from manufacturing.

**Thermal cycling cracks** — Components that heat and cool repeatedly (power transistors, regulators, bridge rectifiers) stress their solder joints through differential thermal expansion. The solder cracks around the lead, creating an intermittent connection that works when cold and fails when hot — or vice versa.

**Tin whiskers** — Pure tin finishes (used before and after the RoHS transition) can grow conductive whiskers over years that short adjacent pins or traces. This is a documented failure mechanism in aerospace and military systems. Lead-tin solder is much less susceptible.

**How to check:** Visual inspection under magnification for cracked, dull, or fractured joints. For intermittent connections, flexing the board gently while monitoring the circuit can reveal cracked joints. Reflow suspect joints with fresh solder — the original flux is long exhausted, so add flux before reheating.

## Connectors and Switches

**Oxidation** — Contact surfaces oxidize over time, especially in humid environments. Gold-plated contacts resist this; tin and silver contacts don't. The result is increased contact resistance that may be intermittent — the connection works when the contact is freshly mated but degrades over time as the oxide layer rebuilds.

**Mechanical wear** — Switch contacts and connector pins wear out from repeated mating cycles. Potentiometers develop noisy spots where the wiper has worn through the resistive element. Rotary switches develop dead spots. Edge connector contacts (common in vintage computers and card-cage industrial equipment) wear from repeated insertion.

**Contact cleaning:** DeoxIT (or similar contact cleaner) is the standard treatment for oxidized contacts. For potentiometers, a cleaning spray rated for carbon or cermet elements works if the element isn't physically worn through. If cleaning doesn't fix a noisy pot, the element is damaged and the pot needs replacement.

## Semiconductors

Semiconductors are generally the most stable components in a legacy circuit. Silicon transistors and diodes don't drift or wear out under normal operating conditions. Failure is typically caused by overstress events (voltage spikes, overcurrent, ESD) rather than aging.

**Exceptions:**
- **Germanium devices** — Found in equipment from the 1950s and 1960s. Germanium has higher leakage current than silicon, and leakage increases with temperature. Germanium transistors in hot environments may have drifted significantly from their original parameters
- **Early ICs** — Some early IC packaging (especially ceramic DIP with gold wire bonds from the 1970s) can develop internal open circuits as wire bonds fatigue or corrode. This is rare but documented in long-life military and industrial applications
- **Optocouplers** — The LED inside an optocoupler degrades over time, reducing the current transfer ratio (CTR). An optocoupler that originally had 100% CTR might be down to 30% after 20 years of operation. This gradual degradation can cause intermittent or marginal behavior in feedback loops and isolation circuits
- **LEDs** — Display LEDs in vintage equipment dim over time, especially early red LEDs and seven-segment displays that have been running continuously. The function is unimpaired but the display becomes hard to read

## Gotchas

- **Don't replace everything at once** — Shotgun replacement of all suspect components makes it impossible to learn what actually failed. Replace the most likely culprit first, test, and iterate. The exception is electrolytic capacitors in a power supply — those are cheap and almost certainly degraded, so batch replacement is reasonable
- **New parts can cause new problems** — A modern replacement electrolytic may have much lower ESR than the original, which can destabilize voltage regulators that relied on the ESR for loop compensation. Check the regulator requirements before substituting ultra-low-ESR caps
- **Measure before you condemn** — A resistor that reads 10% high might be a drifted carbon comp, or it might be the right value and you're misreading the color code (faded bands are common on old resistors). Measure, don't guess
- **Reforming electrolytics** — Capacitors that have been unpowered for years can sometimes be reformed by slowly ramping up the applied voltage through a current-limiting resistor. This gradually rebuilds the oxide dielectric layer. It doesn't always work, but it can save a rare or hard-to-replace cap
