---
title: "Which Probe Type for This Situation?"
weight: 40
---

# Which Probe Type for This Situation?

The default probe isn't always the right probe. Each probe type makes trade-offs between bandwidth, loading, voltage range, and what it can physically connect to. Picking the right one before you start measuring avoids getting a technically correct but practically useless reading.

## Quick Decision Table

| What you're measuring | Recommended probe | Why |
|----------------------|------------------|-----|
| General-purpose voltage (digital, analog, power) | Passive 10x | Good all-rounder: wide voltage range, decent bandwidth, low loading |
| Low-level signal (mV range, audio) | Passive 1x or direct BNC | No 10:1 attenuation — better sensitivity and SNR |
| Fast digital (> 50 MHz edge content) | Passive 10x with spring-tip ground, or active probe | 10x for adequate bandwidth; active for lowest loading |
| Very fast digital or RF (> 500 MHz) | Active probe | Low capacitance (~1 pF), high bandwidth |
| Voltage across floating nodes | Differential probe | Two-input, no ground clip to short things |
| Current without breaking the circuit | Current clamp (AC or AC/DC) | Clips around a wire, non-invasive |
| EMI source localization | Near-field probe (H or E) | Sniffs magnetic or electric fields without contact |
| Low resistance (milliohms) | DMM with Kelvin clips (4-wire) | Eliminates lead resistance from measurement |
| General DMM voltage/resistance | Standard test leads | Banana plug leads with shrouded probes |

## Passive 10x Probe: The Default

**Bandwidth:** 100–500 MHz (probe-dependent)
**Input impedance:** 10 MOhm || ~12–15 pF
**Attenuation:** 10:1 (signal is divided by 10, scope compensates in software)
**Max voltage:** Typically 300V CAT I

### When to Use

- Default for almost everything: digital logic, analog signals, power rail checks
- Whenever loading needs to be reasonable and you need decent bandwidth
- When voltage exceeds the scope's direct input range (> ±5V or so)

### How It Works

The 10x probe uses a compensated resistive-capacitive divider: a 9 MOhm resistor in series with the probe tip, combined with the scope's 1 MOhm input impedance, gives 10:1 attenuation. A trimmer capacitor in the probe compensates for the cable capacitance so the divider ratio is flat across frequency.

### Gotchas

- Must be compensated (trimmer cap adjusted on the scope's 1 kHz cal output). Under-compensated shows drooping square wave edges; over-compensated shows overshoot. Check this whenever you use a new probe or move between scopes
- 10:1 attenuation means the scope's vertical noise is 10x more significant relative to the signal. For millivolt-level signals, 1x or direct BNC is better
- Max voltage rating is typically 300V peak. Not for mains

## Passive 1x Probe: Low-Level Signals

**Bandwidth:** 6–15 MHz (much lower than 10x)
**Input impedance:** 1 MOhm || ~100+ pF
**Attenuation:** None (1:1)
**Max voltage:** Typically 300V CAT I

### When to Use

- Audio-frequency signals where sensitivity matters
- Low-level DC or slow AC where you need to see millivolts without 10x attenuation noise
- Situations where bandwidth above 10 MHz is not needed

### Gotchas

- High capacitance (~100 pF) loads the circuit significantly at high frequencies. This probe is a low-pass filter at high-Z nodes
- Much lower bandwidth than 10x — a fast edge looks completely different on 1x vs. 10x
- Many modern probes are switchable 1x/10x. When in 1x mode, the probe is essentially a shielded cable with all its capacitance presented to the circuit

## Active Probes: High Bandwidth, Low Loading

**Bandwidth:** 500 MHz – 10+ GHz
**Input impedance:** ~100 kOhm || ~1 pF (varies by model)
**Attenuation:** Varies (probe has built-in amplifier)
**Max voltage:** Low — typically ±8V or less

### When to Use

- Signals with frequency content above a few hundred MHz
- High-impedance nodes where even 15 pF is too much loading (crystal oscillators, VCOs, high-Z analog)
- Rise time measurements where you need the true edge shape

### How It Works

An active probe contains a FET amplifier at the probe tip, which presents high impedance to the circuit and drives the cable capacitance with a low-impedance output. This decouples the circuit from the cable, achieving both low loading and high bandwidth.

### Gotchas

- Low voltage range — active probes are easily damaged by overvoltage. Don't probe power rails without knowing the voltage first
- Expensive, and require power (usually from the scope through a proprietary interface)
- Require specific scope compatibility (not universal BNC)
- ESD-sensitive at the tip

## Current Probes

### AC Current Probes (Transformer-Based)

**Bandwidth:** Typically a few Hz to 50–100 MHz
**Works by:** Clamping around a current-carrying conductor; the changing magnetic field induces a proportional voltage

- No circuit modification needed — clamp around a wire
- AC only (transformer can't pass DC)
- Needs a single conductor (not a cable with supply and return together — the fields cancel)
- Best for measuring AC current waveforms: switching transients, inrush, AC load current

### DC/AC Current Probes (Hall-Effect)

**Bandwidth:** DC to 50–100 MHz (probe-dependent)
**Works by:** Hall sensor detects DC field; transformer winding extends bandwidth for AC

- Measures both DC and AC current
- More expensive than AC-only probes
- Hall sensor needs periodic degaussing (zeroing) — DC offset drifts
- Lower bandwidth than the best AC-only probes

### Gotchas for Both Types

- Sensitivity decreases for small currents — most clamp probes are best above 100 mA to 1 A
- Position the conductor in the center of the clamp for best accuracy; off-center reading is lower
- External magnetic fields (nearby transformers, motors) can affect the reading
- The conductor must be a single wire, not a cable bundle. If supply and return are in the same cable, their fields cancel and the probe reads zero

## Differential Probes

**Bandwidth:** 25 MHz – 1 GHz+ (varies widely by model and price)
**Input impedance:** High (typically 1–10 MOhm || a few pF per input)
**Common-mode voltage:** 600V – 1500V (probe-dependent)

### When to Use

- Measuring voltage across two nodes that are both not at earth ground
- High-side current sense resistors, motor phase voltages, bridge circuits
- Any measurement where connecting a scope ground clip would create a short

### Gotchas

- CMRR degrades with frequency — fine at DC and low frequency, but at 10 MHz the common-mode rejection may be 40 dB or less
- Cheap differential probes may have CMRR of only 30–40 dB even at low frequency
- Differential probes have separate ratings for differential voltage (between inputs) and common-mode voltage (both inputs to earth). Don't exceed either

## Near-Field Probes: EMI Debugging

**Type:** H-field (magnetic loop) or E-field (capacitive stub)
**Bandwidth:** Typically broadband, DC to GHz range
**Use:** Localizing sources of electromagnetic emission on a board

### When to Use

- EMI pre-compliance debugging — finding which component or trace is radiating
- Tracking down noise coupling paths
- Not for measuring a specific voltage or current — for finding where emissions come from

### Gotchas

- Near-field measurements are relative, not calibrated. You're comparing "more here" vs. "less there," not measuring dBuV/m
- H-field probes pick up current loops; E-field probes pick up voltage nodes. Use both to triangulate
- Orientation matters — the H-field probe is directional (sensitive to field through the loop plane)

## DMM Probes and Accessories

### Standard Test Leads

- Banana plug to shrouded probe tip — the default for most DMM measurements
- CAT rating printed on the leads. Replace if damaged, melted, or not rated for your environment

### Kelvin Clips (4-Wire)

**When to use:** Measuring resistances below ~1 Ohm where lead resistance affects the reading

- Four terminals: two carry the test current, two sense the voltage
- The sense leads carry negligible current, so lead resistance drops out of the measurement
- Essential for milliohm measurements (PCB trace resistance, contact resistance, fuse resistance)

### Gotchas

- Standard DMM leads contribute ~0.1–0.5 Ohm of lead resistance. For measurements above 10 Ohm, this is negligible. Below 1 Ohm, it's a significant fraction of the reading
- Kelvin (4-wire) mode requires a meter that supports it — not all DMMs have 4-wire capability
- Probe tip contact quality matters more at low resistance. Dirty or oxidized contacts add resistance that looks like the DUT
