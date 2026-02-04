---
title: "Dummy Load for Car Audio Amps"
weight: 10
categories:
  - Audio
  - High-Current
  - Automotive
---

# Dummy Load for Car Audio Amps

A dummy load replaces the speaker during bench testing. It presents the same impedance as a speaker without producing ear-splitting volume, without risking a good driver, and without the mechanical and acoustic variables that make real speakers unreliable as test loads. When verifying a car amp after repair, a dummy load is needed before a scope.

## Why Not Just Use a Speaker?

- **Volume** — A car amp at full power into a speaker on the bench is dangerously loud and will annoy everyone within earshot
- **Speaker protection** — If the amp has a fault (DC offset, oscillation, rail failure), it will damage or destroy the speaker
- **Consistency** — A speaker's impedance varies with frequency (the nominal "4 Ω" is only accurate at one frequency). A resistive dummy load is a known, flat impedance — measurements against it are repeatable
- **Safety** — No moving cone means no acoustic feedback, no vibrating objects on the bench, no accidentally shorting exposed speaker terminals against something

## Design Requirements

| Parameter | Requirement | Rationale |
|-----------|-------------|-----------|
| Impedance | Match the amp's rated load — typically 4 Ω, sometimes 2 Ω | The amp's output stage is designed for this impedance. Testing into the wrong impedance gives misleading power and current draw readings |
| Power rating | At least the amp's rated RMS power per channel | The load must absorb the amp's full continuous output without overheating. Undersized loads change resistance as they heat, and eventually fail open |
| Non-inductive | Wirewound resistors are fine if non-inductive types are used, or if the inductance is negligible at audio frequencies | An inductive load shifts phase at high frequencies, which can affect amplifier stability measurements. For most car amp testing below 20 kHz, standard wirewound is acceptable |
| Connectable | Banana jacks, binding posts, or heavy-gauge wire with spade terminals | This gets connected and disconnected frequently. Make the connections solid and fast |

## Build Options

### Option 1: Wirewound Power Resistors on a Heatsink

The simplest and most common approach.

**Parts:**
- Wirewound power resistors — 4 Ω (or 2 Ω) rated for the required power. Common choices: 50 W chassis-mount resistors (Dale / Ohmite / TE type). Parallel or series-parallel multiple resistors to get the target impedance and power rating
- Aluminum heatsink or a piece of thick aluminum plate
- Thermal paste or thermal pads between resistors and heatsink
- Binding posts or banana jacks for connections

**Example — 4 Ω / 200 W load:**
- Four 16 Ω, 50 W wirewound resistors in parallel → 4 Ω, 200 W total
- Or two 8 Ω, 100 W resistors in parallel → 4 Ω, 200 W total
- Mount on an aluminum plate with thermal compound. Add a fan for sustained full-power runs

**Example — 2 Ω / 400 W load:**
- Four 8 Ω, 100 W resistors in parallel → 2 Ω, 400 W total
- This gets hot. A substantial heatsink and active cooling (fan) are necessary for extended testing

**Construction tips:**
- Bolt the resistors flat against the heatsink for maximum thermal contact
- Use thermal paste between the resistor body and the heatsink — same as for a transistor
- Keep leads short and heavy gauge — at high currents, even short leads drop meaningful voltage
- Add a small fan blowing across the heatsink for sustained full-power tests. A 12 V PC fan powered from the same supply works well

### Option 2: Commercial Dummy Load

Several companies sell purpose-built audio dummy loads (8 Ω is common for home audio; 4 Ω models exist for car audio). These are typically a resistor bank in an enclosure with binding posts and sometimes a built-in attenuator tap for monitoring the signal.

**Advantages:** Ready to use, properly rated, sometimes include a scope tap.

**Disadvantages:** Expensive relative to the DIY option, and the exact impedance/power combination needed for car audio work may not be available.

### Option 3: Halogen Work Lamp

A 12 V halogen bulb (the kind from a car headlight or a work light) is a rough-and-ready dummy load in a pinch. A 55 W H7 bulb presents roughly 2–3 Ω when hot. Not precise — the resistance changes with temperature (cold filament is much lower resistance) — but it works for a quick smoke test and provides a visual power indicator.

Not recommended for real measurements. Use it only when a load is needed *right now* and resistors aren't on hand.

## Measuring With the Dummy Load

The dummy load isn't just a heat sink — it's a measurement point.

**Output power:**
1. Scope or DMM AC across the load while driving a sine wave
2. Read the RMS voltage (V_rms)
3. Power = V_rms² / R_load
4. Example: 20 V_rms across 4 Ω = 100 W

**Distortion and clipping:**
- Scope across the load. A clean sine at moderate power should look like a clean sine. Flat tops = clipping. Ragged edges or ringing = oscillation or instability
- Increase drive level until clipping appears — note the voltage and calculate the power. This is the amp's actual maximum clean output

**DC offset:**
- DMM DC across the load with no signal. Should read < 50 mV. A dummy load tolerates DC offset that would cook a speaker voice coil

**Scope tap:** If the load dissipates hundreds of watts, a scope probe's ground clip should not be the only connection point. Build in a permanent scope connection — a BNC jack wired across the load, or binding posts that allow clipping on without fumbling near high-current connections.

## Multi-Channel Setup

Car amps are typically 2-channel or 4-channel (sometimes more with bridging). A dummy load is needed on *every channel under test*.

**Why all channels matter:**
- The power supply is shared. Loading one channel affects the rails and the available current for other channels. Testing one channel at a time doesn't reveal power supply sag or thermal issues that only appear under full load
- Bridged mode doubles the voltage swing across the load. If the amp supports bridging, the load must be rated for bridged power (typically 4× the per-channel power) at the bridged impedance (typically double the per-channel impedance)

**Practical approach:** Build four identical 4 Ω loads for testing 4-channel amps. Use two in parallel (2 Ω) for bridged-mono testing. Label each one.

## Safety

- **These get hot.** A 200 W resistor bank at full power causes burns on contact. Don't touch the resistors or heatsink during or immediately after a test. Keep flammable materials away
- **Ventilation** — Resistors at full dissipation in an enclosed space will heat the room and may overheat. Test in a ventilated area or use a fan
- **Secure connections** — Loose connections at high current arc and melt. Use proper terminals, tighten them, and inspect before each session
- **Don't leave it unattended at full power** — Especially with a DIY build. Stay at the bench during sustained power tests

## Cross-References

- Current-limited power supply setup: [Tools & Fixtures for Debugging]({{< relref "/docs/debugging/tools-and-fixtures" >}})
- Current draw measurement: [Is Current Draw Expected?]({{< relref "/docs/measurement/power-rails-supplies/current-draw" >}})
