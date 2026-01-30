---
title: "Is This Sensor Outputting the Expected Voltage/Current?"
weight: 40
---

# Is This Sensor Outputting the Expected Voltage/Current?

Analog sensor interfaces — thermocouples, strain gauges, photodiodes, microphones, potentiometers, Hall sensors. Before blaming the signal conditioning or the ADC, verify that the transducer itself is producing the expected output for the physical stimulus. Many "signal conditioning" bugs are actually "sensor isn't connected right" bugs.

## DMM: DC Sensor Output Check

**Tool:** DMM, V⎓ (DC Volts) or mA mode
**When:** Verifying that a sensor produces the expected output for a known physical input

### Voltage-Output Sensors

| Sensor type | Output type | Expected output | How to verify |
|------------|------------|----------------|---------------|
| Potentiometer (position) | Ratiometric DC | 0V to Vref, proportional to position | Rotate and check for smooth, proportional voltage change |
| Thermistor (in divider) | DC voltage | Depends on divider and temperature | Measure at known temperature, compare to calculated value |
| LM35 / TMP36 (temp) | DC voltage | 10 mV/°C (LM35), 10 mV/°C + 500 mV offset (TMP36) | At room temp (~25°C): LM35 = 250 mV, TMP36 = 750 mV |
| Phototransistor / photodiode (with load R) | DC voltage | Varies with light intensity | Cover sensor (dark) = near 0V; shine light = increased voltage |
| Hall effect (linear) | DC voltage | Vcc/2 ± proportional to field | No magnet = Vcc/2 (quiescent); magnet present = offset from center |
| Pressure sensor (resistive bridge) | DC voltage (differential) | mV-range differential, ratiometric | At zero pressure: near 0 mV differential. At known pressure: per datasheet sensitivity |

### Current-Output Sensors (4–20 mA)

1. Wire the DMM in series with the sensor loop (break the loop, insert DMM on mA range)
2. Apply known stimulus
3. Read current:
   - 4 mA = zero/minimum (sensor powered, reporting low end of range)
   - 20 mA = full scale
   - < 4 mA = sensor fault, wiring open, or power issue
   - > 20 mA = sensor fault or wiring error

### What You Learn

- Whether the sensor is alive and responsive to stimulus
- Whether the output magnitude is correct for the known input condition
- Whether the output changes smoothly with stimulus (not stuck, not jumping)

### Gotchas

- Many sensors have small outputs (millivolts for thermocouples, microvolts for strain gauges). Your DMM must be on an appropriate range and its noise floor must be below the expected signal
- Sensor output depends on excitation. A resistive bridge needs stable excitation voltage/current. If the excitation is wrong, the sensor output is wrong — check excitation first
- "Ratiometric" sensors (output proportional to supply voltage) need the supply measured too. If the supply drifts, the output drifts proportionally — the ratio is correct but the absolute voltage is off. Measure the supply and calculate the ratio

## Oscilloscope: Dynamic Sensor Output

**Tool:** Oscilloscope, DC-coupled (or AC-coupled for small AC signals on a DC bias)
**When:** The sensor output varies dynamically — vibration sensors, microphones, audio pickups, AC current sensors

### Procedure

1. DC-couple the channel to see the full output (bias + signal)
2. Set vertical scale to the expected signal range
3. Apply the physical stimulus:
   - Tap or vibrate for an accelerometer / piezo
   - Speak or play a tone for a microphone
   - Move a magnet for a Hall sensor
   - Vary light for a photodiode
4. Observe the output waveform — does it respond to the stimulus?

### What You Learn

- Whether the sensor produces an output that correlates with the physical input
- The waveform shape, amplitude, and frequency content of the sensor output
- Whether the output is clean or noisy

### Gotchas

- Piezoelectric sensors (accelerometers, some microphones, knock sensors) are AC-only — they produce a voltage pulse in response to change, not a steady DC output. Don't expect a DC level from a piezo sensor
- Electret microphones need a bias voltage (typically through a resistor from VCC to the output pin). Without bias, the mic produces nothing. Check the bias circuit first
- Capacitive loading from scope probes can affect high-impedance sensor outputs (piezo, pH probes, some photodiode circuits). Use a FET-input probe or buffer the signal before measuring

## Thermocouple Measurement

**Tool:** DMM with thermocouple input, or DMM in mV mode + cold junction reference
**When:** Verifying thermocouple output

### With a Thermocouple-Capable DMM

1. Connect the thermocouple to the DMM's TC input (observing polarity — TC wire colors indicate polarity)
2. Set DMM to temperature mode and select the thermocouple type (K, J, T, etc.)
3. Read temperature directly — the DMM handles cold junction compensation internally

### With a Standard DMM (mV Mode)

1. Set DMM to millivolt DC mode
2. Connect the thermocouple to standard banana leads (be aware this introduces a junction at the probe-to-wire connection)
3. Read the millivolt output
4. Look up the voltage in the thermocouple reference table for the wire type
5. Add cold junction compensation: the reading is the *difference* between the hot junction and the cold junction (where the TC wire meets the meter). If the meter is at room temperature (~25°C), add the voltage corresponding to 25°C for that TC type

### Expected Outputs (Type K)

| Temperature | Voltage (ref to 0°C cold junction) |
|------------|-------------------------------------|
| 0°C | 0.000 mV |
| 25°C | 1.000 mV |
| 100°C | 4.096 mV |
| 200°C | 8.138 mV |
| 500°C | 20.644 mV |

### Gotchas

- Thermocouple voltages are tiny (microvolts per degree). Any stray voltage at the connections (dirty contacts, dissimilar metals in the wiring path) adds error. Use proper TC extension wire, not random copper wire
- Cold junction compensation is essential. Without it, the reading is relative to the meter's temperature, not to absolute temperature. A 10°C change in room temperature shifts the reading by ~0.4 mV (Type K)
- Open TC wire reads a random voltage (often railing to full-scale on dedicated TC meters). If the reading is wildly wrong, check for open circuit

## Strain Gauge / Wheatstone Bridge

**Tool:** DMM in millivolt mode (for bridge output), plus excitation supply
**When:** Verifying strain gauge or load cell output

### Procedure

1. Verify excitation voltage across the bridge (typically 5V or 10V)
2. Measure the differential output of the bridge (between the two output nodes)
3. At zero load/strain: output should be near 0 mV (within the bridge's offset spec, typically < ±1 mV per V excitation)
4. Apply a known load: output should change per the gauge factor and applied strain

### What You Learn

- Whether the bridge is powered and producing output
- Whether the output responds correctly to applied force/strain
- Whether the zero offset is within spec (large offset suggests a damaged gauge or wiring error)

### Gotchas

- Bridge outputs are small — 1–3 mV/V at full scale for typical load cells. A 10V excitation at full load gives only 10–30 mV. Your DMM needs millivolt resolution and low noise
- Lead resistance matters. For long cable runs to a strain gauge, use 4-wire (Kelvin) connection or 6-wire (remote sense) excitation to eliminate voltage drop in the wires
- Temperature affects strain gauges. Most gauges have temperature compensation built into the bridge, but large temperature changes still cause drift. Allow thermal equilibration before measuring
