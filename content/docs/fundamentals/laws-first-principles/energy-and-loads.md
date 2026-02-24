---
title: "Energy, Fields & Loads"
weight: 5
---

# Energy, Fields & Loads

Before diving into specific laws, it helps to establish what is physically happening in a circuit. At the most basic level, circuits move energy from sources to loads. Every component along the way either stores energy, converts it from one form to another, or dissipates it. The laws that follow — Ohm's, Kirchhoff's, conservation — are all ways of accounting for this movement and transformation.

## Where Energy Exists

Energy that stays in the electrical domain — stored in fields:

- **Electric fields** — present wherever there is a voltage difference. A charged capacitor holds energy in its electric field.
- **Magnetic fields** — present wherever current flows. An inductor stores energy in its magnetic field. Transformers couple energy between windings through shared magnetic fields.
- **Chemical potential** — batteries convert stored chemical energy into electrical energy. This is the dominant energy source at the boundary between the electrical and chemical domains.

Energy that leaves the electrical domain — conversion at boundaries:

- **Thermal energy** — heat. The universal sink. Every real component converts some electrical energy into heat, whether intended or not.
- **Mechanical energy** — motors convert electrical energy into rotation; generators do the reverse.
- **Radiation** — antennas convert electrical energy into electromagnetic waves. LEDs convert it into light.

In the electrical domain, energy is stored only in electric and magnetic fields. Chemical energy enters at battery terminals, and thermal, mechanical, and radiated energy leave at transducers — but inside the circuit, fields are where the energy lives. This is the physical foundation everything else in this section builds on.

## How Energy Moves

Two quantities track energy movement through a circuit:

- **Voltage** (potential difference) is the driving force. It represents how much energy is available per unit charge between two points.
- **Current** is charge in motion. It represents the rate at which charge flows past a point.

Together, voltage and current describe the energy flow at any point in a circuit. Energy actually travels in the electromagnetic fields surrounding conductors, but for circuit analysis, tracking voltage and current at nodes and through branches is sufficient — and far more practical.

Two things that always matter:

- **The circuit must form a closed loop.** No loop, no sustained current. An open circuit can have voltage (and stored field energy), but without a closed path there is no sustained energy transfer.
- **The return path always matters.** Current returns to the source. Where and how it returns affects everything from signal integrity to noise. Ignoring the return path is one of the most common sources of confusion at the bench.

## What Stores Energy

Some components hold energy temporarily and can return it:

- **Capacitors** store energy in electric fields. Charge a capacitor, disconnect it, and the energy remains — available to be released later.
- **Inductors** store energy in magnetic fields. Current through an inductor builds a magnetic field; interrupting that current releases the stored energy (sometimes violently).
- **Batteries** store energy chemically. They convert between chemical and electrical energy in both directions (if rechargeable).

Storage is ideally lossless — the mechanism returns energy to the circuit rather than consuming it. Real components have parasitic losses, but the storage function itself is fundamentally different from conversion. For the quantitative treatment of how stored energy relates to time, see [Energy Over Time]({{< relref "energy-over-time" >}}).

## What Converts Energy

Other components transform electrical energy into a different form, permanently:

- **Resistors** → heat (by design or by necessity)
- **Motors** → mechanical work
- **Speakers** → acoustic energy
- **LEDs** → light (plus some heat)
- **Antennas** → electromagnetic radiation
- **Heaters** → thermal energy (resistors on purpose)

The key pattern: energy is never created or destroyed, only converted from one form to another. Every watt that enters a component leaves as something — if not useful work, then heat. This principle gets formal treatment in [Conservation of Energy]({{< relref "conservation-of-energy" >}}).

## Loads

A **load** is anything that draws current from a source and converts electrical energy into another form. It is a role, not a specific component type. A resistor is a load when it dissipates power from a supply. A motor is a load when it draws current to produce torque. Even a battery is a load when it is being charged.

### Types of Loads

**Resistive loads** — voltage and current are in phase. Energy converts directly to heat. Examples: heaters, incandescent bulbs, bias resistors. These are the simplest loads to analyze and the easiest to supply.

**Reactive loads** — voltage and current are out of phase. Energy is stored and returned cyclically rather than consumed in a single pass.

- *Inductive loads* store energy in magnetic fields. Motors, solenoids, relay coils, and transformers are all inductive. They resist changes in current and can produce voltage spikes when switched off — a direct consequence of the energy stored in their magnetic fields.
- *Capacitive loads* store energy in electric fields. Long cables, filter networks, and piezo elements present capacitive loading. They resist changes in voltage and can draw large inrush currents when first connected.

This ties directly back to the energy storage section above — reactive loads are loads that include significant energy storage.

**Active loads** — contain their own energy source or regulation circuitry. They control their own voltage or current draw rather than passively responding to what the source provides. Voltage regulators, battery chargers, and active electronic loads fall into this category.

**Nonlinear loads** — the voltage-current relationship is not constant. They may draw current in pulses or only during part of a cycle. Rectifiers, switching power supplies, and digital circuits are common nonlinear loads. Their current waveforms contain harmonics that the source must be able to handle.

### Why Load Classification Matters

Knowing what type of load a circuit presents determines:

- **Source requirements** — a reactive load needs a source that can handle energy flowing back and forth, not just forward. A nonlinear load needs a source that can deliver current peaks well above the average.
- **Power factor** — reactive and nonlinear loads draw more apparent power than they consume as real power. This affects everything from component ratings to utility bills.
- **Meaningful measurements** — measuring a DC voltage across a nonlinear load tells a different story than measuring across a resistive one. The load type determines which measurements are informative and which are misleading.

For the quantitative relationship between voltage, current, and power in different load types, see [Power Basics]({{< relref "power-basics" >}}).

## Tips

- **Start with the energy path** — before analyzing a circuit in detail, trace where energy enters, where it's stored, and where it leaves. This gives you the big picture before diving into specific voltages and currents.
- **Classify the load first** — knowing whether a load is resistive, reactive, nonlinear, or active tells you what the source must provide and which measurements will be meaningful. It's the single most useful thing to establish early.
- **Return path as a first-class concern** — when debugging, give the return path as much attention as the forward path. Many "mystery" failures trace back to an inadequate or unintended return.
- **Use heat as a diagnostic** — unexpected warmth localizes unintended energy conversion. An IR thermometer or even a finger check narrows the search faster than voltage measurements in many cases.

## Caveats

- **Voltage without current is not power delivery** — an open circuit can have full supply voltage across it, but no energy is being transferred. Both voltage and current must be present simultaneously for energy to move.
- **"Lossless" storage is an idealization** — real capacitors have ESR, real inductors have winding resistance and core loss. Energy stored and returned is always less than ideal; the difference becomes heat.
- **Load type can change with operating conditions** — a motor is an inductive load at stall but presents back-EMF (acting partly as an active load) when spinning. A battery is a source when discharging and a load when charging. Don't assume the classification is fixed.
- **Reactive loads don't consume average power (ideally)** — energy sloshes back and forth between source and reactive element each cycle. The source must handle the peak current even though average real power delivery is zero. This is the origin of power factor concerns.

## In Practice

- **Circuit does nothing when powered** — verify the complete energy path. Is there actually a closed loop from source through load and back? An open return path is one of the most common reasons a circuit sits there doing nothing.
- **Circuit does something unexpected when powered** — suspect an unintended energy path. A short circuit bypasses the intended load, routing current where it should not go. The result is excessive current, rapid heating, and potential damage. Solder bridges, stray wire strands, and failed insulation are common causes. If a fuse blows or a supply shuts down immediately, look for a short before anything else.
- **Load behaves differently on different supplies** — suspect a mismatch between load type and source capability. A reactive or nonlinear load on a source that assumes resistive behavior will not get what it needs.
- **Heat where none is expected** — something is acting as an unintended load. Parasitic resistance in a connector, a degraded solder joint, or a component conducting when it should be off — all convert electrical energy to heat in places the design did not account for.
