---
title: "Energy Over Time"
weight: 35
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Energy Over Time

Watts tell the rate. Joules tell the total. The distinction matters whenever battery life, capacitor holdup, inrush current, or fault survival time is in question.

## Joules vs. Watts

- **Power (watts)** = rate of energy transfer. How fast energy flows
- **Energy (joules)** = power × time. How much energy flows in total

1 W for 1 second = 1 J. A 60 W light bulb running for 1 hour consumes 216,000 J (60 W × 3600 s) = 216 kJ.

In battery contexts, energy is often expressed in watt-hours (Wh) or milliamp-hours (mAh). These are just energy in more convenient units:

- 1 Wh = 3600 J
- mAh is charge, not energy — multiply by voltage to get energy: E = mAh × V (approximately, since voltage varies with discharge)

## Capacitors as Energy Storage

A charged capacitor stores energy:

**E = ½CV²**

- A 1000 µF cap at 12 V stores ½ × 0.001 × 144 = 0.072 J
- That's not a lot. Capacitors are fast energy sources, not bulk storage

### Holdup Time

How long can a capacitor sustain a load after the input is removed?

Rough estimate: t = (C × V_drop) / I_load

Where V_drop is the voltage the cap can sag before the downstream regulator drops out. A 1000 µF cap feeding a 5 V LDO with 1 V of headroom at 100 mA gives about 10 ms of holdup. Capacitors buy milliseconds, not seconds.

### Inrush Current

A discharged capacitor looks like a short circuit at t=0. Charging a big cap through a low-impedance source creates a large inrush current spike. The energy stored in the cap has to come from somewhere, and the supply has to deliver it:

- Peak inrush current = V_source / R_path (where R_path is the total series resistance including source impedance, trace resistance, and any inrush limiter)
- Inrush limiters (NTC thermistors, resistors, active circuits) deliberately add resistance to control this spike

## Inductors as Energy Storage

A current-carrying inductor stores energy:

**E = ½LI²**

- A 10 µH inductor at 1 A stores 5 µJ — tiny, but released very quickly it creates a large voltage spike
- Switching off inductive loads (relays, solenoids, motors) creates voltage spikes that destroy transistors. The stored energy has to go somewhere, and if no path is provided (flyback diode), the inductor forces the voltage up until something breaks down

## Time Constants

The time constant indicates how fast energy moves in and out of storage elements.

### RC Time Constant

τ = R × C

After one time constant, a charging capacitor reaches 63% of the final voltage. After five time constants, it's within 1% (effectively fully charged for most purposes).

- 1 kΩ × 1 µF = 1 ms
- 10 kΩ × 100 nF = 1 ms (same result, different parts)
- The RC time constant sets response time, filter cutoff frequencies, and timing circuit behavior

### RL Time Constant

τ = L / R

Same exponential behavior, but for current in an inductor. After one time constant, current reaches 63% of its final value.

- 10 mH / 100 Ω = 0.1 ms
- Inductive time constants are usually short in electronics (small L, moderate R) but can be significant with large inductors or very low resistance paths

## Battery Sizing

Batteries are the most common bulk energy store in project work. The math is simple — the gotchas are in the derating.

- **Wh vs. mAh** — Watt-hours are the honest comparison unit across chemistries and voltages. mAh is charge, not energy: a 3000 mAh cell at 3.7 V (11.1 Wh) holds nearly four times the energy of a 3000 mAh cell at 1.2 V (3.6 Wh). Always convert to Wh when comparing options
- **Basic sizing** — estimate average system current, multiply by desired runtime, and add margin. Example: an MCU drawing 20 mA average plus a sensor drawing 5 mA = 25 mA average. For 24 hours of runtime: 25 mA × 24 h = 600 mAh minimum at the operating voltage
- **Discharge curve reality** — a "3.7 V" Li-ion cell delivers 4.2 V when full and sags to 3.0 V when empty. Usable capacity depends on the minimum voltage your regulator tolerates — if the regulator drops out at 3.3 V, you lose whatever capacity remains below that point
- **Derating** — nameplate capacity is measured under ideal conditions (room temperature, low discharge rate, new cell). Design for 70–80% of nameplate to account for age, temperature, and real-world discharge rates
- **Peak vs. average current** — a battery must handle burst loads (radio TX, motor start, LED flash) without excessive voltage sag. Internal resistance × peak current = instantaneous voltage drop. A coin cell with 30 Ω internal resistance and a 100 mA pulse sags 3 V — it cannot do it

For a fuller treatment — chemistry trade-offs, cell count, thermal and safety considerations — see [Power Source Selection]({{< relref "/docs/design-development/ideation-and-requirements/power-source-selection" >}}).

## Tips

- Use E = ½CV² for quick capacitor energy estimates — remember energy scales with voltage squared
- For holdup time calculations, work backward from the minimum voltage the downstream circuit needs
- When sizing inrush limiters, consider both the peak current and the energy that must be dissipated in the limiter

## Caveats

- Energy scales with V² for caps and I² for inductors — Doubling the voltage on a cap quadruples the stored energy. This matters for safety (charged caps can deliver dangerous energy) and for sizing storage
- mAh is charge, not energy — A 2000 mAh battery at 3.7 V has 7.4 Wh. A 2000 mAh battery at 1.2 V has 2.4 Wh. Same mAh rating, very different energy
- Time constants assume linear circuits — The exponential response assumes constant R and C (or L and R). Nonlinear elements (voltage-dependent capacitance, current-dependent inductance) change the dynamics
- Supercapacitors bridge the gap — With farads of capacitance, supercaps can provide seconds to minutes of holdup. But they have limited voltage ranges and high ESR compared to batteries

## In Practice

- A circuit that works during continuous operation but fails on power-up suggests inrush current issues
- Voltage droop on a power rail after the supply is disconnected indicates how much holdup time the bulk capacitance provides
- An inductor making noise or running hot after switching off current indicates inadequate flyback suppression
- Time constant mismatches (too slow or too fast) show up as sluggish response or ringing in filter and timing circuits
