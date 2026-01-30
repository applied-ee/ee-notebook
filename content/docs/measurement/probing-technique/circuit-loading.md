---
title: "Am I Loading the Circuit?"
weight: 10
---

# Am I Loading the Circuit?

Your instrument changes the circuit by connecting to it. Every probe has finite impedance, and it draws current from the node you're measuring. Most of the time this is negligible — but when it isn't, you get a reading that's accurate for the loaded circuit and wrong for the unloaded one. Knowing when loading matters saves you from chasing phantom problems.

## DMM: Resistive Loading

**Tool:** DMM, V⎓ (DC Volts) mode
**Input impedance:** Typically 10 MOhm (Klein MM520: 10 MOhm)

### When It Matters

The DMM's 10 MOhm input impedance forms a voltage divider with the source impedance of the node you're measuring. If the source impedance is much less than 10 MOhm, loading is negligible. If it's comparable, your reading is low.

| Source impedance | Loading error with 10 MOhm DMM | Matters? |
|-----------------|-------------------------------|----------|
| 100 Ohm (power rail) | 0.001% | No |
| 10 kOhm (resistive divider) | 0.1% | Rarely |
| 1 MOhm (high-Z sensor, feedback divider) | 9.1% | Yes — reading is ~91% of actual |
| 10 MOhm (piezo, pH probe) | 50% | Severely — reading is half of actual |

### Rule of Thumb

Source impedance should be less than 1/10th of the meter's input impedance for < 1% loading error. For a 10 MOhm DMM, that means source Z < 1 MOhm.

### Procedure

1. Measure the voltage
2. If the reading seems low or unstable, consider the source impedance
3. If source Z is unknown, try two meters with different input impedances — if they agree, loading isn't a factor; if they disagree, the higher-impedance reading is closer to truth

### Gotchas

- Feedback circuits are sensitive to loading. Placing a DMM across a voltage divider that sets a regulator's output can shift the output voltage because you've changed the divider ratio
- LoZ mode (Low Impedance, ~3 kOhm on MM520) intentionally loads the circuit. This is useful for ghost voltage rejection but will pull down real high-impedance sources. Don't use LoZ for measuring high-Z nodes

## Oscilloscope: Resistive + Capacitive Loading

**Tool:** Oscilloscope with passive probe (10x)
**Input impedance:** 10 MOhm || ~13–20 pF (probe + cable capacitance)

### The Dual Nature of Scope Loading

A scope probe loads the circuit both resistively (10 MOhm, usually negligible) and capacitively (~15–20 pF including the cable). At DC and low frequencies, the resistive loading dominates and is almost always irrelevant. At high frequencies, the capacitive loading dominates and can significantly affect fast signals.

### Capacitive Loading Effects

The probe capacitance forms a low-pass filter with the source impedance:

**f_corner = 1 / (2 * pi * R_source * C_probe)**

| Source impedance | Probe capacitance | Corner frequency | Effect |
|-----------------|-------------------|-----------------|--------|
| 50 Ohm | 15 pF | 212 MHz | Negligible — probe BW is the limit |
| 1 kOhm | 15 pF | 10.6 MHz | Starting to matter for fast digital |
| 10 kOhm | 15 pF | 1.06 MHz | Significant — rolls off above 1 MHz |
| 100 kOhm | 15 pF | 106 kHz | Severe — audio frequencies and above are attenuated |

### Procedure

1. Consider the source impedance of the node you're probing
2. For high-impedance nodes (> 10 kOhm), recognize that the probe capacitance will filter high-frequency content
3. If you need to probe a high-Z node at high frequency, use an active probe (sub-pF input capacitance) or a FET-input probe

### What You Learn

- Whether the waveform you see is what the circuit actually produces, or the probe's filtered version
- Whether connecting the probe changes the circuit's behavior (oscillators stopping, timing shifting)

### Gotchas

- Crystal oscillator circuits are notoriously sensitive to probe loading. Connecting a 15 pF probe to a crystal oscillator can shift its frequency, reduce amplitude, or stop oscillation entirely. A 10x probe is better than 1x (lower tip capacitance), but an active probe is best
- Capacitive loading adds phase shift, which matters in feedback loops. Probing a compensation network can change the loop response
- 1x probes have much higher tip capacitance (~100 pF+) than 10x probes (~12–15 pF). Use 10x for anything where loading matters

## Current Measurement: Burden Voltage

**Tool:** DMM in A (current) mode
**Internal shunt resistance:** Varies by range (typically 1–100 Ohm)

### The Problem

A DMM measures current by inserting a low-value shunt resistor in series with the circuit and measuring the voltage across it. This shunt drops some voltage — the "burden voltage" — which reduces the voltage available to the circuit and can change the current you're trying to measure.

### Typical Burden Voltages

| Current range | Typical shunt | Burden at full-scale | Matters? |
|--------------|--------------|---------------------|----------|
| 10A | ~10 mOhm | ~100 mV | Usually not |
| 400 mA | ~1 Ohm | ~400 mV | Maybe — depends on supply margin |
| 4 mA | ~100 Ohm | ~400 mV | Yes — significant fraction of a 3.3V supply |
| 40 uA | ~10 kOhm | ~400 mV | Almost certainly — this is a large drop |

### Procedure

1. Note which current range the DMM is on (auto-range can change shunt resistance mid-measurement)
2. Estimate or check the meter's burden voltage specification for that range
3. Compare the burden voltage to the supply voltage. If burden is > 1% of supply, it may be affecting the circuit

### What You Learn

- Whether the current reading is accurate for the actual operating circuit, or for a circuit with reduced supply voltage
- Whether using a different measurement technique (current probe, sense resistor + scope) would be less invasive

### Gotchas

- Auto-range switching changes the shunt resistance, which can cause transient current disturbances in sensitive circuits
- On low-current ranges, the shunt resistance is high enough that connecting the DMM can significantly change circuit behavior. If a 3.3V circuit normally draws 20 uA and you put it on the 40 uA range (10 kOhm shunt), you've dropped ~200 mV — the circuit is now running at 3.1V
- Current clamp probes (AC or DC) measure current without breaking the circuit and have zero burden voltage — but they have their own limitations (bandwidth, sensitivity, DC offset)
