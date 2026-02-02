---
title: "Conservation of Energy"
weight: 30
---

# Conservation of Energy

Where does the power go? Every watt sourced by a supply or battery must be accounted for — dissipated as heat, stored in a field, or radiated. When the missing power can't be found, it's the accounting that's off, not the physics.

## Power In = Power Out

In any circuit at steady state, the total power delivered by sources equals the total power consumed by loads. This is just the energy form of KVL applied to the whole circuit.

For a simple DC circuit:

- Power source delivers: P_source = V_source x I
- Power consumed by each element: P = V_drop x I (for series elements) or P = V x I_branch (for parallel elements)
- Sum of all consumed power = source power

If these don't balance, there's a path, a drop, or a dissipation that hasn't been accounted for.

## Where Power Goes

### Resistive Dissipation (Heat)

The most common destination. P = I^2 x R = V^2 / R. Every resistor, every trace, every contact resistance, every wire converts electrical energy to heat. This is usually the bulk of the power budget.

### Energy Storage (Capacitors and Inductors)

Capacitors store energy in electric fields (E = 1/2 CV^2). Inductors store energy in magnetic fields (E = 1/2 LI^2). In steady-state AC, they cycle energy in and out but don't dissipate (ideally). During transients, they're absorbing or releasing energy, and that energy has to come from or go somewhere.

### Radiation

At RF frequencies, circuits radiate energy. At low frequencies, radiation is negligible. But it's always nonzero, and in some contexts (EMC, antenna design) it's the whole point.

### Mechanical Work

Motors, speakers, actuators — energy leaving the electrical domain to do mechanical work. The power budget must account for mechanical output plus electrical losses.

## Accounting for "Missing" Power

When something doesn't add up:

- **Trace and wire resistance** — Often overlooked. 10 A through a PCB trace with 10 milliohms = 1 W. That matters
- **Connector drops** — Every connector has contact resistance. At high currents, this dissipates real power
- **Snubber and clamp losses** — Voltage clamps and snubber networks absorb energy from switching transients. This power appears as heat in the snubber resistor
- **Switching losses** — In transistors, the transition between on and off states dissipates power. Not captured by the simple on-resistance model
- **Leakage currents** — Small but nonzero. In high-voltage circuits, leakage through PCB contamination or component insulation can add up
- **Ground path resistance** — Current flowing through the ground return path drops voltage across ground impedance. That's real power dissipation that doesn't show up if the model assumes ground is zero ohms everywhere

## Tips

- **Thermal troubleshooting** — If a component is hotter than expected, it's dissipating more power than the model predicts. Find the extra current or voltage drop
- **Efficiency calculations** — Output power / input power. The difference is losses, and each watt of loss should be identifiable
- **Battery life estimation** — Total energy consumed determines runtime. Every milliwatt that can't be accounted for is eating into battery life
- **Debugging overcurrent** — If a supply is delivering more current than expected, that power is going somewhere. Follow the energy to find the fault

## Caveats

- **Reactive power isn't "lost"** — In AC circuits, capacitors and inductors cause current to flow without doing real work. The power factor tells how much of the apparent power is actually being dissipated. Don't confuse VA with W
- **Transient vs. steady state** — During power-up, energy is being stored in caps and inductors. The power balance only works as "source = dissipation" after everything has settled
- **Negative resistance isn't free energy** — Tunnel diodes and certain oscillator circuits exhibit negative resistance in a limited region. This doesn't violate energy conservation — there's always an external energy source enabling the behavior

## Bench Relevance

Conservation of energy is the logic behind every thermal surprise and every power budget that doesn't close. A component running hotter than expected means the power model is missing something — a current path, a voltage drop, or a loss mechanism that isn't in the schematic. A supply delivering more current than the load should need points to the same gap. The discipline is always the same: find where each watt goes, and the ones that seem to be missing will point directly to the part of the circuit that isn't understood yet.
