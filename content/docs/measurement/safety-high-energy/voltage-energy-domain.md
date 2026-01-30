---
title: "What Voltage/Energy Am I Dealing With?"
weight: 20
---

# What Voltage/Energy Am I Dealing With?

Before choosing instruments, probes, and safety precautions, you need to know the energy domain of the circuit you're working on. A 3.3V microcontroller board and a 400V PFC stage require completely different approaches — and the difference isn't always obvious from the outside of the box.

## Identifying the Energy Domain

**Tool:** Schematic, markings, context — then DMM to confirm
**When:** Before any measurement on unfamiliar equipment

### The Domains

| Domain | Typical voltages | Examples | Hazard level |
|--------|-----------------|----------|--------------|
| Low-voltage DC | < 50V | Battery circuits, MCU boards, LED drivers, USB | Generally safe to touch; current can still burn at high amps |
| Mains AC | 120V / 240V (±10%) | Power entry, after the plug, before the transformer or supply | Lethal. Full stop |
| High-voltage DC | 100V–800V+ | Tube amplifiers (B+), PFC bus, CRT anode, switchmode primary side, EV batteries | Lethal, with stored energy that persists after power-off |
| Transient / induced | Varies | Flyback spikes, boost converter outputs, inductive kick, ESD | Can exceed steady-state voltage by 10x or more |

### Procedure

1. Read the schematic if available — identify power supply topology and voltage rails
2. Look at component voltage ratings on the board: electrolytic capacitors are labeled with max voltage, giving a strong clue about the rail voltage
3. Check transformer markings, fuse ratings, and regulator part numbers
4. Note where isolation boundaries exist — primary vs. secondary side of a transformer, optocoupler barriers
5. Measure with DMM to confirm — schematic values are nominal; actual voltages can differ

### What You Learn

- Which parts of the circuit require high-voltage safety precautions
- What instrument ratings (CAT category, voltage) you need
- Where isolation boundaries exist and which ground reference to use

### Gotchas

- Voltage can exceed the supply rail. Boost converters, flyback converters, charge pumps, and ringing LC circuits all produce voltages higher than their input. A 12V-input boost converter can have a 48V output. A flyback transformer can ring to hundreds of volts
- Mains-connected equipment has primary-side voltages present on the PCB even when the output is "low voltage." The inside of a USB charger has mains on it
- Battery-powered doesn't mean low-voltage. EV battery packs run 300V–800V. Some test equipment has internal HV supplies

## Stored Energy: Why Voltage Alone Isn't Enough

**Tool:** Calculation from schematic values, then DMM to verify voltage
**When:** Whenever capacitors > 10 uF exist at > 50V, or large inductors carry significant current

### Energy in Capacitors

**E = 1/2 CV²**

| Capacitor | Voltage | Energy | Danger |
|-----------|---------|--------|--------|
| 10 uF | 12V | 0.7 mJ | Negligible |
| 100 uF | 50V | 125 mJ | Noticeable spark |
| 220 uF | 400V | 17.6 J | Potentially lethal |
| 1000 uF | 400V | 80 J | Absolutely lethal |

The threshold for a dangerous shock is debated, but stored energy above ~1 joule at voltages above ~50V deserves serious respect.

### Energy in Inductors

**E = 1/2 LI²**

Inductor energy is usually a transient hazard — it's released as a voltage spike when current is interrupted, not as sustained stored energy after power-off. But in active circuits, breaking a current-carrying inductor connection creates an arc.

### Procedure

1. Identify large capacitors and their voltage ratings from schematic or board markings
2. Calculate worst-case stored energy at rated voltage
3. After power-off, measure actual voltage across capacitors with DMM
4. Discharge as needed before working (see "Can I probe this safely?" for discharge procedures)

### What You Learn

- Whether the circuit holds enough energy to be dangerous after power is removed
- How long you might need to wait (or actively discharge) before it's safe

### Gotchas

- Multiple capacitors in a power supply stage add up — three 220 uF/400V caps in a PFC stage store over 50 joules total
- Even "low-voltage" circuits can store surprising energy if the capacitance is large enough. A 1 farad supercap at 5V stores 12.5 joules
- Inductors don't store energy after current stops flowing, but if you're the one interrupting the current (pulling a connector, opening a switch), the inductive kick is directed at you

## DMM: Confirming the Domain

**Tool:** DMM, V⎓ (DC Volts) and V~ (AC Volts)
**Mode:** Auto-range or manual range set above expected voltage

### Procedure

1. Set DMM to DC Volts, highest manual range if you're unsure of the voltage
2. Measure main supply rails, capacitor voltages, and any point where you plan to connect a scope or other instrument
3. Switch to AC Volts and re-measure to catch AC components
4. Record the values — you'll need them to select probe ratings and oscilloscope vertical ranges

### What You Learn

- Actual voltages vs. schematic nominal values
- Whether the circuit is in the expected energy domain
- Any unexpected voltages that indicate a fault or a topology you didn't account for

### Gotchas

- Auto-range starts at the lowest range and steps up — on a high-voltage circuit, you may briefly get an "OL" reading before it settles. Starting at a high manual range avoids this and is safer
- True RMS meters give accurate AC readings on non-sinusoidal waveforms; average-responding meters can be significantly off on switched or distorted waveforms
- Measuring across a series string of capacitors only tells you the total voltage, not the per-capacitor voltage. Measure each one individually if balance matters
