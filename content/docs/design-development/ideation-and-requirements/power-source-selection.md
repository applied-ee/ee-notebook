---
title: "Power Source Selection"
weight: 27
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Power Source Selection

The power source is one of the earliest and most constraining design choices. It determines voltage rails, thermal budget, physical form factor, safety requirements, and often the regulatory landscape. Choosing late — or choosing by default — causes cascading rework when the supply can't deliver what the circuit needs.

This page surveys common power source types for learning and hobby projects, with the deepest treatment on batteries since they raise the most sizing and safety questions.

## AC Mains

Wall adapters and offline supplies convert mains AC (120 V / 230 V) to a usable DC rail.

- **Use a pre-made adapter.** For hobbyist and learning projects, always start with a certified wall adapter or bench supply. Designing an offline (mains-connected) supply requires isolation, creepage/clearance knowledge, and regulatory compliance that is well beyond beginner scope — and the safety consequences of getting it wrong are severe
- **Voltage and frequency** — mains voltage varies by country (100–240 V, 50/60 Hz). Most modern adapters are universal-input; check the label
- **Output types** — unregulated wall warts (voltage varies with load), linear regulated adapters (clean but inefficient), and switching adapters (efficient, compact, some output ripple)
- **Safety fundamentals** — isolation separates the mains-referenced primary from the user-accessible secondary. Earth ground provides a fault path. These aren't optional — they are what keeps a metal enclosure from becoming lethal
- **When to design your own** — essentially never as a learner. Buy a certified adapter and design your circuit from its DC output. The interesting design problems start after the adapter

## USB Power

USB as a power source (separate from its data role) is increasingly common for portable and desk-tethered projects.

- **Voltage and current by spec:**
  - USB 2.0 — 5 V, 500 mA (2.5 W)
  - USB 3.x — 5 V, 900 mA (4.5 W)
  - USB Battery Charging (BC 1.2) — 5 V, 1.5 A (7.5 W)
  - USB Power Delivery (PD) — negotiated, up to 20 V / 5 A (100 W), with EPR up to 48 V / 5 A (240 W)
- **Before enumeration** — a device may only draw 100 mA from a USB 2.0 port until it has enumerated and requested more. Exceeding this can cause the host to shut off the port
- **Not all ports deliver rated current** — cheap hubs, long cables, and older laptops may current-limit below spec. Design for the minimum you can tolerate, not the maximum the spec allows
- **When USB power is enough** — microcontroller projects, sensor nodes, small displays, LED strips (short ones). When it isn't — motors, high-power RF, heating elements, anything that needs more than a few watts continuously
- **USB-C and PD negotiation** — drawing more than 5 V / 3 A requires PD negotiation with a PD controller IC. Without negotiation, you get 5 V only

## Batteries

Batteries are the default for portable and untethered projects. They introduce constraints that wall-powered circuits don't face: finite energy, voltage that changes with state of charge, current limits from internal resistance, and safety considerations that vary by chemistry.

### Chemistry Overview

No single chemistry is best — each trades off energy density, safety, cost, discharge characteristics, and availability.

| Chemistry | Nominal V/cell | Energy Density | Self-Discharge | Safety Notes | Typical Use |
|-----------|---------------|----------------|----------------|--------------|-------------|
| Alkaline (primary) | 1.5 V | Moderate | Low (years shelf life) | Safe, non-rechargeable | Remote controls, low-drain devices |
| NiMH | 1.2 V | Moderate | Moderate (1–3%/month for LSD) | Safe, tolerates abuse well | AA/AAA rechargeables, legacy devices |
| Li-ion (18650, 21700) | 3.6–3.7 V | High | Low (~2%/month) | Requires protection circuit; fire risk if shorted, punctured, overcharged | Laptops, power banks, high-drain portable |
| LiPo (pouch) | 3.7 V | High | Low | Same risks as Li-ion; puncture-sensitive pouch | Drones, RC, compact wearables |
| LiFePO4 (LFP) | 3.2 V | Moderate | Low | Most abuse-tolerant Li chemistry | Solar storage, EVs, safety-critical |
| Coin cell (CR2032 etc.) | 3.0 V | Low | Very low (10-year shelf life) | Safe; very limited current capability | RTC backup, key fobs, low-power sensors |

### Voltage and Cell Count

A single cell rarely matches the voltage rail a circuit needs. Series stacking raises voltage; parallel stacking raises capacity.

- **Series** — voltages add. 3 × NiMH (1.2 V) = 3.6 V. 2 × Li-ion (3.7 V) = 7.4 V. Cell count notation: 1S, 2S, 3S (series), 1P, 2P (parallel). A "3S2P" pack is 3 cells in series, doubled in parallel
- **Regulators bridge the gap** — a boost converter steps up (single Li-ion 3.7 V → 5 V), a buck converter steps down (2S Li-ion 7.4 V → 5 V or 3.3 V). The regulator's dropout and efficiency eat into usable energy
- **Balance charging** — series Li-ion cells drift apart over charge cycles. A balance charger (or BMS with balancing) ensures all cells reach full charge without any cell being overcharged. Unbalanced packs lose capacity and become safety risks

### Capacity Budgeting

Estimate energy needs before choosing a battery, not after.

1. **List every subsystem** and its current draw in each operating mode (active, sleep, transmit, idle)
2. **Estimate duty cycles** — what fraction of time does each subsystem spend in each mode?
3. **Calculate weighted average current:**

   Example — battery-powered sensor node:
   | Subsystem | Active Current | Duty Cycle | Weighted Current |
   |-----------|---------------|------------|-----------------|
   | MCU (active) | 5 mA | 2% | 0.10 mA |
   | MCU (sleep) | 10 µA | 98% | 0.01 mA |
   | Sensor | 15 mA | 1% | 0.15 mA |
   | Radio TX | 120 mA | 0.5% | 0.60 mA |
   | Regulator quiescent | 5 µA | 100% | 0.005 mA |
   | **Total average** | | | **≈ 0.87 mA** |

4. **Multiply by runtime:** 0.87 mA × 8760 h (1 year) = 7,621 mAh at the battery voltage
5. **Derate:** at 75% usable capacity, you need a battery rated ≥ 10,161 mAh (nameplate)

### Discharge Curves and Cutoff

A battery's voltage drops as it discharges. The shape of this curve varies by chemistry:

- **Alkaline** — steady decline from 1.5 V, with a long tail. Capacity is highly dependent on where you set the cutoff
- **NiMH** — relatively flat at 1.2 V for most of discharge, then drops sharply. Easier to work with than alkaline
- **Li-ion** — starts at 4.2 V, relatively flat around 3.6–3.7 V through the middle 60–70%, then drops steeply below 3.4 V. Standard cutoff is 3.0 V (going lower damages the cell)
- **LiFePO4** — very flat at 3.2–3.3 V. Hard to measure state of charge by voltage alone because the curve is so flat

"Capacity" on a datasheet is measured to a specific cutoff voltage at a specific discharge rate. If your regulator drops out at a higher voltage, your usable capacity is less than the rated number.

### Peak Current and Internal Resistance

- **C-rate** — a 1C discharge empties the rated capacity in one hour. A 2000 mAh cell at 1C delivers 2 A; at 0.5C, 1 A. Maximum continuous C-rate varies by chemistry and cell design
- **Internal resistance** — ranges from milliohms (large Li-ion) to tens of ohms (coin cells). Under load: V_terminal = V_open_circuit − (I_load × R_internal)
- **Why a coin cell can't start a motor** — a CR2032 has roughly 10–30 Ω internal resistance. A motor stall current of 200 mA would cause a voltage drop of 2–6 V — the cell voltage collapses. Coin cells are for microamp-to-low-milliamp loads only
- **Pulse capability** — some cells tolerate brief high-current pulses better than sustained high current. Datasheet pulse ratings indicate what the cell can handle without excessive sag or damage

### Thermal and Safety

- **Charging temperature** — most Li-ion cells must not be charged below 0 °C (lithium plating risk) or above 45 °C. Discharging has wider limits but still degrades cells at temperature extremes
- **Protection circuits** — Li-ion and LiPo cells need protection against overcharge (>4.25 V/cell), overdischarge (<2.5 V/cell), overcurrent, and short circuit. Many bare cells ship without protection; you must add a protection IC or use a BMS
- **LiPo handling** — pouch cells are puncture-sensitive. A swollen LiPo is generating gas from internal degradation — stop using it. Store at 3.7–3.8 V/cell (storage charge), not fully charged
- **Venting and fire** — thermal runaway in Li-ion is self-sustaining and produces toxic fumes. Use cells from reputable manufacturers with safety certifications. Never charge unattended without a protection circuit

### Charging

- **CC/CV for Li-ion** — charge at constant current (typically 0.5–1C) until the cell reaches 4.2 V, then hold voltage constant while current tapers. Charging is complete when current drops below a threshold (typically C/20). This is handled by a dedicated charger IC (TP4056, MCP73831, BQ24072, etc.) — do not implement CC/CV in software
- **USB charging** — many charger ICs accept USB 5 V directly. Current draw must respect USB limits unless PD negotiation is used
- **NiMH charging** — more complex to detect full charge. Common methods: −ΔV detection (voltage dip at full charge), temperature rise rate, or timed charging with a conservative rate. Dedicated NiMH charger ICs exist for a reason
- **Charging while running (power path)** — if the circuit must operate while the battery charges, a power-path management IC handles load sharing and seamless switchover between USB/adapter and battery

## Solar and Energy Harvesting

Solar panels and other harvesting sources (thermoelectric, piezoelectric, RF) are variable, low-power sources — not replacements for a battery.

- **Solar panels are current sources, not voltage sources** — output current depends on illumination; open-circuit voltage is relatively stable. A panel's power output varies with angle, shading, weather, and time of day
- **Maximum Power Point Tracking (MPPT)** — the optimal operating voltage for maximum power extraction shifts with conditions. MPPT controllers adjust the load to track this point. Simple solar chargers use a fixed-voltage approach that leaves energy on the table
- **Harvesting rarely powers directly** — the typical architecture is: harvesting source → charge controller → battery → regulator → circuit. The battery buffers the intermittent source and provides stable power
- **Energy budgeting is harder** — you must estimate average harvested energy per day and ensure it exceeds average consumption per day, with margin for bad days (cloudy weather, less vibration, etc.)

## Combining Sources

Real projects often use more than one power source.

- **USB + battery** — the most common pattern for portable devices. USB charges the battery and may power the circuit directly when connected. A power-path IC manages the switchover. When USB is removed, the battery takes over seamlessly
- **Mains adapter + supercap holdup** — for always-on devices that must survive brief power outages. The supercap provides seconds of holdup while the system saves state or shuts down gracefully
- **Solar + battery** — solar charges the battery during the day; the battery powers the circuit at night. Sizing the battery for overnight runtime and the panel for daytime recharging (with margin) is the core design problem
- **Power-path management** — load-sharing circuits prevent the battery from simultaneously charging and discharging (which causes excess heat and accelerated wear). Dedicated ICs (BQ24072, LTC4412, MAX17710) handle this cleanly

## Tips

- Start with the load budget. Everything else — chemistry, cell count, charging method — follows from how much energy and current the circuit actually needs
- Use Wh, not mAh, when comparing batteries of different chemistries or voltages
- Check regulator dropout voltage against the battery's end-of-discharge voltage — this determines usable capacity
- Read the battery datasheet's discharge curves at your expected load current, not the headline capacity number
- For Li-ion projects, buy cells with integrated protection circuits unless you are adding a BMS. Bare cells are one wiring mistake away from a fire
- Budget for the charger IC, protection circuit, and power-path management from the start — they affect board space, cost, and BOM

## Caveats

- Battery datasheets report capacity under ideal conditions. Real-world capacity is 70–80% of nameplate after derating for temperature, age, and discharge rate
- "Rechargeable alkaline" cells exist but have poor cycle life and limited recharge depth — NiMH is almost always a better choice for rechargeable AA/AAA needs
- Parallel Li-ion cells without matched internal resistance will not share current equally. One cell does more work and ages faster. Match cells or use per-cell fusing
- USB-C does not guarantee USB-PD. Many USB-C ports are 5 V only. Don't assume higher voltages are available without PD negotiation hardware
- Solar panel ratings are at standard test conditions (1000 W/m², 25 °C). Real outdoor conditions typically yield 50–70% of rated power averaged over a day
- Supercapacitors have high self-discharge compared to batteries — they lose significant charge in hours to days, making them unsuitable for long-term storage

## In Practice

- A portable project that works on the bench but dies in the field often has an undersized battery — the load budget missed peak current events or overestimated sleep mode efficiency
- Unexpected shutdowns during radio transmit or motor start suggest the battery's internal resistance is too high for the peak current demand. Use a cell with lower internal resistance or add a bulk capacitor across the supply
- A battery that charges but doesn't last as long as the math predicts may be hitting the regulator dropout — check the discharge curve against the regulator's minimum input voltage
- A swollen LiPo pouch is not "a little extra capacity" — it means internal degradation and gas generation. Remove it from service immediately
- Intermittent sensor readings in a coin-cell-powered device suggest the sensor's active-mode current is sagging the coin cell voltage below the sensor's minimum operating voltage
