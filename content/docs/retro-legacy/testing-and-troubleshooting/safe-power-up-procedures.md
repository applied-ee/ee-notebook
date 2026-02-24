---
title: "Safe Power-Up Procedures"
weight: 10
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Safe Power-Up Procedures

Applying full line voltage to an unknown or long-dormant board is the fastest way to turn a repairable fault into a catastrophic one. A controlled power-up sequence limits the damage from shorted components, failed capacitors, or degraded insulation.

## Visual and Sensory Inspection First

Before applying any power:

- Inspect the board under magnification for obvious damage — bulging or leaking electrolytic capacitors, burnt resistors, cracked solder joints, corrosion around battery holders, heat discoloration on the PCB
- Check for loose hardware, broken traces, or solder bridges (especially on boards that have been previously repaired)
- Smell the board — charred components and leaked electrolyte have distinctive odors that indicate prior failure even when visual damage is not obvious

## Variac Bring-Up

A variable autotransformer (variac) allows gradual increase of the AC supply voltage from zero to full line voltage while monitoring current draw.

- Start at zero volts and increase slowly (5-10 V per step), pausing at each step to monitor current draw on an ammeter in series with the supply
- At each voltage step, check for unusual heat generation (touch test on power semiconductors and regulators), smell, and smoke
- If current draw is abnormally high at low voltage, stop and investigate before increasing further — a shorted rectifier, failed filter capacitor, or shorted transformer winding will draw excessive current even at reduced voltage
- The variac does not provide isolation from mains — use with an isolation transformer when working on mains-referenced equipment

## Current-Limited Supply Bring-Up

For DC-powered boards, a bench supply with adjustable current limiting serves the same role as a variac. Set the current limit to the board's expected idle draw (or lower if unknown), then increase the voltage gradually.

- If the supply hits current limit immediately at low voltage, something on the board is shorted or heavily loading the rail
- A board that draws expected current at rated voltage but consumes increasing current over minutes may have a thermally unstable component entering runaway

## Dim Bulb Tester

An incandescent light bulb in series with the AC line limits inrush current to whatever the bulb draws at full brightness. A 60 W bulb limits current to roughly 500 mA at 120 VAC.

- If the bulb glows dimly and the equipment powers up, the board is drawing normal current
- If the bulb glows at full brightness, the equipment has a near-short somewhere in the power supply — the bulb is absorbing most of the power and protecting the board from further damage
- A dim bulb tester is a cheap, always-available alternative to a variac for initial screening, though it provides less control over the voltage ramp

## Caveats

- **A variac does not provide isolation** — it is an autotransformer with a direct electrical connection between input and output. Working on mains-referenced equipment through a variac without an isolation transformer is a shock hazard. Always use an isolation transformer for safety on any equipment that connects to AC mains

## In Practice

- **A power supply that current-limits on the bench supply at well below rated voltage** usually has a shorted rectifier diode or a failed electrolytic capacitor with near-zero impedance — measure across the diode bridge and check the main filter cap before investigating further
