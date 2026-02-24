---
title: "Grounding & Shielding Patterns"
weight: 40
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Grounding & Shielding Patterns

Ground strategy is the most commonly botched aspect of legacy interfacing. Getting this wrong makes everything else noise.

## Single-Point Grounding

Connect the grounds of the legacy and modern systems at exactly one point. All signal returns flow through that single connection. This prevents ground loops by ensuring there is only one path for ground current.

**When it works:** Low frequencies (audio, slow digital), short cable runs, systems where all ground connections are under control.

**When it fails:** High frequencies (above ~1 MHz), where the inductance of the single-point ground wire becomes significant and forces return currents to find other paths (through capacitive coupling, through cable shields, through the mains ground).

## Shield Termination

Cable shields must be connected to something -- the question is where and how:

- **Audio and low-frequency analog:** Connect the shield at the source end only. This drains static charge and provides electric-field shielding without creating a ground loop through the shield. The receiving end's shield floats (or is connected through a small capacitor to ground for RF shielding without DC ground loop)
- **RF and high-frequency digital:** Connect the shield at both ends. At high frequency, the shield must be a low-impedance return path for the signal's return current. Connecting at one end only creates an antenna above the shield's quarter-wavelength frequency
- **Mixed (common in legacy systems):** Connect at the source end for audio, both ends for data. If a cable carries both audio and data signals, the compromise is to bond the shield at both ends and break the ground loop elsewhere (isolation transformer on the audio, differential signaling on the data)

## Chassis Bonding

When the legacy system uses chassis ground as signal reference (common in industrial equipment, older audio gear, and test instruments):

- Bond the modern system's ground to the legacy chassis ground at the interface point
- Keep the bond short and low-impedance (wide braid or strap, not a long wire)
- Do not create additional ground connections elsewhere -- one point of contact between the two ground systems
- If the chassis bond creates unacceptable noise, insert isolation at the signal level instead of attempting to lift the ground

## Tips

- **Measure the ground voltage between the two systems with a DMM before connecting any signal wires** — this one measurement, which takes seconds, reveals whether isolation is needed and prevents the most common ground-offset damage
- **When troubleshooting ground noise, disconnect signal cables one at a time while monitoring the noise** — the cable whose removal eliminates the noise identifies which connection is creating the ground loop

## Caveats

- **Ground voltage between systems can drift with load** -- A DMM may show 0V between the grounds at idle, but once the interface is active, 500 mV of offset appears because the interface cable carries return current through a high-impedance path. Measure the ground offset with the interface active and current flowing, not just at idle
- **USB ground is not isolated** -- USB-connected devices share ground with the host PC. If the legacy system has a ground offset relative to the PC, that offset appears across the interface. A USB isolator module (such as one based on the ADUM4160) breaks this connection

## In Practice

- **A 50/60 Hz hum that appears when connecting a USB-powered MCU to chassis-grounded audio equipment** is almost always a ground loop through the mains wiring — inserting a USB isolator or an audio isolation transformer breaks the loop
- **A digital interface that works reliably at short cable lengths but produces data errors at longer runs** often has a ground offset that increases with distance — the additional cable resistance in the ground return raises the offset voltage, pushing signal levels outside the receiver's threshold
