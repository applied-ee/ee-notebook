---
title: "Are Logic Levels Correct for This Voltage Domain?"
weight: 10
---

# Are Logic Levels Correct for This Voltage Domain?

Logic families have specific voltage thresholds that define what counts as "high" and "low." A 3.3V output driving a 5V input may or may not be recognized as high — it depends on the specific thresholds, not just whether the signal "looks high" on the scope. Marginal logic levels are the source of intermittent, temperature-dependent, works-on-my-bench failures.

## Logic Level Thresholds

**Concept:** VOH, VOL, VIH, VIL — what the driving and receiving devices guarantee

| Parameter | Meaning | Who specifies it |
|-----------|---------|-----------------|
| VOH | Minimum output high voltage | Driver datasheet |
| VOL | Maximum output low voltage | Driver datasheet |
| VIH | Minimum input voltage recognized as high | Receiver datasheet |
| VIL | Maximum input voltage recognized as low | Receiver datasheet |

For reliable communication: **VOH > VIH** and **VOL < VIL**. The gap between them is noise margin.

### Common Logic Families

| Family | VCC | VOH (min) | VOL (max) | VIH (min) | VIL (max) | Noise margin |
|--------|-----|-----------|-----------|-----------|-----------|-------------|
| 5V CMOS (HC/HCT) | 5V | 4.4V | 0.1V | 3.5V (HC) / 2.0V (HCT) | 1.0V (HC) / 0.8V (HCT) | ~0.9V / ~2.4V |
| 3.3V LVCMOS | 3.3V | 2.4V | 0.4V | 2.0V | 0.8V | ~0.4V |
| 1.8V LVCMOS | 1.8V | 1.2V | 0.45V | 1.17V | 0.63V | ~0.03V |
| 5V TTL | 5V | 2.4V | 0.4V | 2.0V | 0.8V | ~0.4V |

### Level Shifting: 3.3V ↔ 5V

A 3.3V LVCMOS output has VOH ≥ 2.4V. Whether a 5V input recognizes this as high depends on the receiver:

| 5V receiver type | VIH threshold | 3.3V output recognized as high? |
|-----------------|---------------|-------------------------------|
| 5V HCT (TTL thresholds) | 2.0V | Yes — 2.4V > 2.0V, safe |
| 5V HC (CMOS thresholds) | 3.5V | No — 2.4V < 3.5V, marginal |
| 5V standard TTL | 2.0V | Yes |

This is why HCT-family parts are commonly used for 3.3V-to-5V interfaces — their TTL input thresholds accept 3.3V outputs cleanly.

## Oscilloscope: Measuring Logic Levels

**Tool:** Oscilloscope, DC-coupled, 10x probe
**When:** Checking actual high and low voltage levels against threshold specs

### Procedure

1. DC-couple the channel and set vertical scale to show the full logic swing (e.g., 1V/div for 3.3V logic)
2. Probe the signal — trigger on an edge to get a stable display
3. Use cursors or auto-measurement to read the high and low levels:
   - **High level:** the flat portion when the signal is high
   - **Low level:** the flat portion when the signal is low
4. Compare to the VOH/VOL specs (driving side) and VIH/VIL specs (receiving side)

### What You Learn

- Whether the actual voltage levels meet the threshold requirements
- How much noise margin exists — a signal at 2.5V into a 2.0V VIH threshold has only 500 mV of margin
- Whether levels shift under load (measure at the receiver pin, not just the driver output)

### Gotchas

- Measure at the receiver's input pin, not the driver's output. Voltage drops across traces, connectors, and series resistors reduce the level that arrives at the receiver
- Temperature affects thresholds and output drive. A signal that's marginal at room temperature may fail at high temperature (where output drive weakens and input thresholds shift)
- Pull-up and pull-down resistors affect levels on open-drain/open-collector buses. Measure the actual level, not the supply voltage of the pull-up

## DMM: DC Level Spot Check

**Tool:** DMM, V⎓ (DC Volts)
**When:** Quick check on a static logic level — enable pins, chip selects, configuration pins

### Procedure

1. Set DMM to DC Volts
2. Probe the pin vs. ground
3. Compare reading to VIH/VIL thresholds

### What You Learn

- Whether a static pin is solidly high, solidly low, or floating in the undefined region

### Gotchas

- A DMM averages the reading. If the pin is toggling (even slowly), the DMM shows an average voltage between high and low — which lands in the undefined region and looks like a fault. Use a scope for anything that might be switching
- Floating pins can read any voltage. If a pin reads ~1.5V on a 3.3V system and it's not driven, it's probably floating, not stuck at some intermediate voltage. Check whether the pin has a pull-up/pull-down in the schematic

## Identifying Marginal Logic Levels

### Symptoms of Marginal Levels

- Works at room temperature, fails when hot or cold
- Works at one supply voltage, fails when the supply droops slightly
- Works on one board but not another (component tolerance)
- Works at low speed but fails at high speed (capacitive loading slows edges, reducing time at valid level)

### Diagnosis

1. Measure VOH at the driver with the actual load connected
2. Measure the voltage at the receiver input pin
3. Calculate noise margin: **NM_high = V_at_receiver - VIH_threshold**
4. If noise margin is less than a few hundred millivolts, the interface is marginal
5. Check both high and low noise margins — either can be the weak link

### Gotchas

- Open-drain buses (I2C) have levels set by pull-up resistors and bus capacitance. A weak pull-up (too high resistance) gives slow rise times and a VOH that depends on the RC time constant vs. the data rate. The level may not reach a valid high before the next transition
- Capacitive loading rounds edges and reduces the time the signal spends at valid levels. At high data rates, the signal may never fully reach VOH or VOL — it swings between intermediate voltages that are technically in the undefined zone
