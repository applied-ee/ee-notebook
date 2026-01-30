---
title: "Do I Need Isolation or Differential Measurement?"
weight: 30
---

# Do I Need Isolation or Differential Measurement?

When a ground-referenced measurement is dangerous or wrong. Most bench instruments (scopes, DMMs in some modes, signal generators) have their ground terminal connected to earth ground through the power cord. If the point you want to measure isn't at earth potential, connecting a ground-referenced instrument either shorts something to earth or gives you a meaningless reading — or both.

## The Core Problem: Scope Ground = Earth Ground

**Concept:** Understanding before you connect

Most oscilloscopes connect the ground clip (the outer shell of BNC, the probe ground clip) directly to earth ground through the three-prong power cord. This means:

- The scope ground clip is not a "reference" you can put anywhere — it's hard-wired to the building's earth
- Connecting the ground clip to any node that isn't already at earth potential creates a short circuit through the scope's ground path
- This can destroy the circuit, destroy the scope, blow fuses, or in mains-connected circuits, create a lethal shock hazard

### Scenarios Where This Bites You

| Scenario | What happens if you clip scope ground to it |
|----------|---------------------------------------------|
| Half-bridge midpoint (motor drive, class-D amp) | Ground clip shorts the low-side switch through earth — smoke and/or dead FETs |
| Primary side of mains SMPS | Ground clip connects mains live or neutral to earth — massive fault current, tripped breaker, possible fire |
| Bridge rectifier output (no isolation transformer) | Ground clip shorts a diode — blows the diode or the scope's ground path |
| Floating battery in series stack | Ground clip forces one terminal to earth — disrupts the circuit, potentially dangerous if high-voltage stack |
| Across a current-sense resistor not at ground | Ground clip shorts out the sense resistor — removes current sensing, changes circuit behavior |

## Differential Probe: The Right Solution

**Tool:** Differential probe (e.g., Micsig DP10013, Tektronix TDP1000)
**When:** Measuring voltage between two nodes that are both not at earth ground

### How It Works

A differential probe has two input pins (+ and −) and no ground clip connection to the circuit. It amplifies the difference between the two inputs and rejects the common-mode voltage (the voltage both inputs share relative to earth). The output connects to a single-ended scope input.

### Procedure

1. Connect the differential probe's + and − inputs across the two nodes of interest
2. Set the probe attenuation on the scope to match the probe ratio
3. Verify common-mode voltage is within the probe's rating (often 600V–1000V peak)
4. Measure as normal — the scope sees only the differential signal

### What You Learn

- True voltage between two floating nodes without disturbing the circuit
- Safe measurement of nodes at high common-mode voltage (e.g., high-side gate drive)

### Gotchas

- Differential probes have finite CMRR (Common-Mode Rejection Ratio) — typically 60–80 dB. At 1000:1 rejection (60 dB), 100V of common mode leaks through as 100 mV of error. On small signals riding on high common-mode voltages, this error can dominate your measurement
- Bandwidth of differential probes varies widely. Cheap probes may be 25–50 MHz; high-end active differential probes reach 1 GHz+. Match to your signal bandwidth
- The two inputs should have matched impedance and length for best CMRR at high frequency

## Isolation Transformer: Making the Scope Float

**Tool:** Isolation transformer, rated for the power level of the DUT
**When:** You need a ground-referenced scope to measure a non-earth-referenced circuit, and a differential probe isn't available or doesn't have enough bandwidth

### How It Works

An isolation transformer breaks the galvanic connection between the DUT's mains power and earth ground. With the DUT floating, its internal nodes can be at any potential relative to earth — so connecting a ground-referenced scope probe doesn't create a short.

### Procedure

1. Power the DUT through an isolation transformer
2. Now the DUT's "ground" is floating relative to earth
3. Connect scope ground clip to the DUT's local ground (circuit common)
4. Probe signals as usual — the scope's earth ground and the DUT's floating ground are now connected through the scope, but no earth-referenced short circuit exists

### What You Learn

- Same as a normal scope measurement, but with the DUT safely floating
- Particularly useful for primary-side SMPS measurements where you need real scope bandwidth and multiple channels

### Gotchas

- The isolation transformer must be rated for the DUT's power draw and voltage. An undersized transformer saturates and stops isolating
- Isolation transformers have parasitic capacitance between windings. At high frequencies, they leak. This limits their effectiveness for high-frequency common-mode rejection
- Everything downstream of the isolation transformer is now floating — including the DUT's chassis if it has one. This can be a shock hazard if you touch the DUT and earth simultaneously. Use this technique knowingly, not casually
- **Never float the scope by lifting its earth prong.** This is sometimes suggested online and it's genuinely dangerous. The scope chassis becomes live at whatever potential the circuit puts on it

## CH1 − CH2 Math: Poor-Man's Differential

**Tool:** Oscilloscope, two channels, math subtraction
**When:** No differential probe available, low common-mode voltage, low frequency

### Procedure

1. Connect CH1 probe to the "+" node, CH2 probe to the "−" node
2. Connect both ground clips to a common earth-ground point on the circuit (not to either measurement node)
3. Set up Math: CH1 − CH2
4. The math trace shows the differential voltage

### What You Learn

- Approximate differential voltage between two nodes
- Good enough for low-frequency, low-common-mode situations (e.g., across a sense resistor near ground)

### Gotchas

- Both probes must be well-matched in attenuation, delay, and bandwidth. Mismatched probes degrade CMRR
- CMRR is poor — maybe 20–30 dB at DC, degrading rapidly with frequency. This is 100x worse than a real differential probe
- Both channels get the full common-mode voltage. If the common-mode exceeds the scope's input range, you clip before subtraction happens
- At high frequency, even small differences in probe cable length create phase errors that look like differential signal. This technique is only trustworthy at low frequencies (audio, slow power signals)
- You use two scope channels for one measurement — expensive in channel-limited setups
