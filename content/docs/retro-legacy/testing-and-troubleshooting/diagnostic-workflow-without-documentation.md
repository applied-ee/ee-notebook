---
title: "Diagnostic Workflow Without Documentation"
weight: 20
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Diagnostic Workflow Without Documentation

When no schematic, service manual, or test procedure exists, the diagnostic process starts with orientation — understanding enough of the circuit to narrow the search.

## Identify Power Rails

Power distribution is the foundation. Without knowing where the supply voltages are and whether they are correct, no other measurement is meaningful.

- Follow the largest traces and widest copper pours — these carry power and ground
- Electrolytic capacitors mark filter points on power rails; their polarity indicates which rail is positive
- Three-terminal regulators (78xx, 79xx, LM317) are easy to identify and immediately reveal the regulated voltage
- Measure every distinct voltage on the board and label them. A board with +5 V, +12 V, and -12 V rails has a different architecture than one with a single +5 V rail

This overlaps with the power-rail identification in [Repair, Substitution, and Reverse Engineering]({{< relref "/docs/retro-legacy/repair-substitution-and-reverse-engineering" >}}) — the distinction is that here the goal is just enough understanding to find the fault, not building a complete schematic.

## Locate Active Devices and Signal Flow

Once the power rails are mapped:

- Identify the major active devices — transistors, ICs, and their general function (amplifier, regulator, logic, driver)
- Trace signal flow from input connectors to output connectors. In most equipment, signal flows left-to-right or input-connector-to-output-connector on the board
- Group the board into functional blocks: power supply, input stage, processing/logic, output stage, feedback paths

## Compare Working vs. Failing Behavior

If the equipment partially works, the diagnostic boundary is the transition from working signal to failing signal:

- Inject a known signal at the input and trace it stage by stage until it disappears or becomes distorted
- If a reference unit is available (same model, working), compare voltages and waveforms at corresponding points — the differences reveal where the fault lies

## Tips

- Start every diagnostic session by measuring the power supply voltages — most legacy faults trace back to a failed power supply component, and no downstream diagnosis is meaningful if the supply is wrong
- Keep a notebook (physical or digital) of voltage readings at key nodes during diagnosis — these readings become the baseline for verifying the repair and for future troubleshooting of the same equipment
- When a board has multiple identical channels (stereo audio, multi-channel industrial), compare the working channel to the failing one — the differences point directly to the fault

## Caveats

- **Do not assume a single fault** — legacy boards that have been sitting unpowered for years often have multiple failures. Fixing one fault and finding the board still does not work does not mean the diagnosis was wrong; it means there is a second fault to find

## In Practice

- **A board with correct voltage on the main rail but incorrect or absent voltages on secondary rails** usually has a failed local regulator or a cracked trace in the secondary distribution — measure each rail independently rather than assuming they are all derived correctly from a single source
- **A mystery IC whose markings have been sanded off or are unreadable** can often be identified from its pinout — power and ground pins follow predictable patterns by package type, and measuring which pins connect to power rails narrows the possibilities quickly
