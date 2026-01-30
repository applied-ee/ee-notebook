---
title: "Can I Touch/Probe This Safely?"
weight: 10
---

# Can I Touch/Probe This Safely?

The first question before any measurement. Before you connect a probe, you need to know whether the circuit is energized, whether there's stored energy waiting to bite you, and whether it's safe to make contact. This isn't paranoia — it's the habit that keeps you measuring for decades instead of months.

## Visual Inspection: Before Power

**Tool:** Your eyes (and nose)
**When:** Before applying power to anything unfamiliar, freshly repaired, or suspect

### Procedure

1. Look for obvious damage: burnt or discolored components, bulging or leaking electrolytic capacitors, cracked PCBs, arcing marks on traces or connectors
2. Smell for the distinctive burnt-electronics odor — if something already released the magic smoke, you want to know before energizing
3. Check for loose wires, solder bridges, or anything that could short when power is applied
4. Verify that safety-critical components (fuses, bleeder resistors, protective earth connections) are intact and present
5. On mains equipment, check the power cord for damage, and verify the earth/ground connection with a DMM continuity test before plugging in

### What You Learn

- Whether there's visible damage that would make power-up dangerous or pointless
- Whether safety components are present and intact
- Whether someone before you left something disconnected, bridged, or modified

### Gotchas

- Visual inspection can't tell you about internal component failure — a MOSFET can be dead-shorted and look perfectly fine
- Bulging capacitors are obvious, but caps can be bad without any visible sign. ESR testing is a separate question
- Don't assume "unplugged" means "safe" — capacitors in power supplies, CRT circuits, and photoflash units can hold lethal charge for hours or days

## Non-Contact Voltage Detection: First-Pass Live Check

**Tool:** Multimeter with NCV mode (e.g., Klein MM520 NCV)
**When:** Quick check to see if mains voltage is present before touching anything

### Procedure

1. Enable NCV mode on the meter — on the MM520 this is a dedicated position on the dial
2. Bring the front of the meter near the conductor, outlet, or wire insulation
3. The meter beeps or lights up if it detects AC electric field

### What You Learn

- Whether mains-level AC is present — yes/no, fast and hands-off
- Useful for confirming a breaker is actually off, or that a wire you're about to cut is de-energized

### Gotchas

- NCV only detects AC fields — it won't detect DC, and it won't find stored charge in capacitors
- Shielded cables and metal enclosures can block the field, giving a false "clear" reading
- Very low-current circuits or wires far from the sensor may not trigger detection
- NCV is a first check, never the only check. Always follow up with a contact measurement

## DMM: Voltage Check on "Dead" Circuits

**Tool:** DMM, DC and AC voltage modes
**When:** After power is removed, confirming the circuit is truly de-energized

### Procedure

1. Verify the DMM is working: test on a known-live source first (or use a proving unit). A meter with dead batteries or a blown fuse will read 0V on a live circuit — and that can kill you
2. Set to DC Volts, measure across the main power rails or capacitor terminals
3. Switch to AC Volts and re-measure — some circuits have residual AC even when the DC supply is off
4. On circuits where ghost voltages are possible (long cable runs, circuits near transformers, high-Z nodes), use LoZ (Low Impedance) mode if your meter has it. LoZ loads the node with ~3 kOhm, collapsing ghost voltages to zero while showing real voltages at full value

### What You Learn

- Whether voltage is truly zero or just looks zero on a high-impedance meter
- Whether stored energy has been drained
- Whether backfeed from another source is energizing the circuit

### Gotchas

- Ghost voltages: capacitive coupling from nearby live conductors can make a dead circuit read 30–80V on a high-impedance (10 MOhm) DMM. LoZ mode is the fix
- Always test on DC *and* AC — a circuit can be DC-dead but have AC induced from a transformer or nearby wiring
- Stored energy in capacitors doesn't show up once the cap has charged to its final voltage and the meter reads "stable." The danger is the energy, not the reading. A 400V capacitor reads 400V DC — but the important thing is that 400V × the capacitance means joules that will arc-weld your probe to the board

## Capacitor Stored-Energy Awareness

**Tool:** DMM for voltage measurement, bleeder/discharge resistor for draining
**When:** Before working on any circuit with significant capacitance at high voltage

### Procedure

1. Identify large capacitors in the circuit — especially bulk electrolytics in power supplies, PFC caps (often 200–450V), and any cap rated > 50V
2. Measure voltage across each suspect capacitor with the DMM
3. If voltage is present, discharge through an appropriate resistor — not a screwdriver. A screwdriver discharge creates a current spike that can damage the capacitor, weld contacts, or spray molten metal
4. Use a discharge resistor rated for the voltage and energy: typical approach is a 1–10 kOhm power resistor (5–25W) with insulated leads, applied across the cap terminals. Discharge time ≈ 5RC
5. Re-measure after discharge to confirm voltage is below 1V (or whatever your safety threshold is)
6. For repeated work on the same equipment, a proper bleeder circuit or discharge tool is worth building

### What You Learn

- Whether stored energy has been drained to a safe level
- How much energy was stored: E = 1/2 CV². A 220 uF cap at 400V stores 17.6 joules — enough to kill

### Gotchas

- Capacitors can recover voltage after discharge ("dielectric absorption" or "soakback"). Large electrolytics at high voltage can bounce back to tens of volts after sitting for a few minutes. Measure again before touching
- Inductors store energy too: E = 1/2 LI². Disconnecting a current-carrying inductor creates a voltage spike. Less common as a stored-energy hazard on powered-down equipment, but be aware if you're breaking connections in active circuits
- Bleeder resistors in the original circuit may have failed open, leaving capacitors charged indefinitely. Don't assume the designer's discharge path is working

## General Habits

These aren't measurements, but they keep you alive while making measurements:

- **One-hand rule:** When probing anything that might be at hazardous voltage, keep one hand in your pocket or behind your back. Current path hand-to-hand crosses the heart. Hand-to-ground is survivable more often than hand-to-hand
- **Insulated tools:** Use probes and tools rated for the voltage you're working with. CAT-rated probes exist for a reason
- **Work alone carefully:** If you must work on live high-voltage, tell someone where you are. Better yet, don't work on live high-voltage alone
- **Assume it's live until proven dead:** By measurement, not by assumption, memory, or the position of a switch you didn't personally verify
