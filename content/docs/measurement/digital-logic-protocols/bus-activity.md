---
title: "Is This Bus Communicating?"
weight: 20
---

# Is This Bus Communicating?

Quick checks before diving into protocol decode. Is there any activity at all? Are clock and data both toggling? Is the bus stuck high, low, or floating? These first-pass checks take seconds and immediately narrow down whether the problem is electrical (no activity) or protocol-level (activity but wrong content).

## Oscilloscope: Activity Check

**Tool:** Oscilloscope, 2 channels, DC-coupled, 10x probes
**When:** First look at a digital bus — SPI, I2C, UART, or any clocked/serial interface

### Procedure

1. Identify the bus signals from the schematic:
   - **SPI:** SCK (clock), MOSI (data out), MISO (data in), CS (chip select)
   - **I2C:** SCL (clock), SDA (data)
   - **UART:** TX, RX
   - **CAN:** CANH, CANL (differential pair)
2. Probe the clock line on CH1 and a data line on CH2
3. Set trigger to Auto (free-running) to see whatever is present
4. Look for:
   - Toggling signals: the bus is active
   - Flat lines: the bus is not communicating (stuck or idle)
   - The idle state: I2C idles high (pull-ups), SPI CS idles high (inactive), UART idles high (mark state)

### What You Learn

- Whether the bus has any electrical activity at all
- Whether both clock and data are present (for clocked buses)
- The approximate data rate and burst pattern (continuous vs. periodic transactions)

### Quick Bus Health Table

| Bus | What "healthy idle" looks like | What "active" looks like |
|-----|-------------------------------|------------------------|
| I2C | SCL and SDA both high (pulled up) | Both lines toggling, SDA changes while SCL is low |
| SPI | SCK idle (high or low depending on CPOL), CS high | CS goes low, SCK toggles, MOSI/MISO toggle |
| UART | TX and RX both high (idle = mark) | Line drops low (start bit), followed by data bits |
| CAN | CANH ~2.5V, CANL ~2.5V (recessive) | CANH rises to ~3.5V, CANL drops to ~1.5V (dominant) |

### Gotchas

- "No activity" doesn't always mean the bus is broken. Some buses are idle until the host initiates a transaction. Make sure your firmware is actually trying to communicate when you're looking at the bus
- Triggering on Auto mode shows everything including noise. If you see low-level noise on an idle bus, switch to Normal trigger and set the level appropriately to confirm whether there's real activity or just pickup
- Level-shifted buses may idle at unexpected voltages if the level shifter has no pull-ups or the wrong pull-up voltage. Check the actual idle voltage against expectations

## DMM: Static Pin Check

**Tool:** DMM, V⎓ (DC Volts)
**When:** Checking bus lines for correct idle state, stuck conditions, or shorts before powering the scope

### Procedure

1. With the system powered, set DMM to DC Volts
2. Check each bus line vs. ground
3. Compare to expected idle voltage:
   - I2C SCL/SDA: should be near VCC (pull-up voltage) when idle
   - SPI CS: should be near VCC when inactive
   - UART TX: should be near VCC (idle high)

### What You Learn

- Whether pull-up resistors are present and working (I2C lines should be high when idle)
- Whether a line is stuck low (possible short, driver fault, or bus contention)
- Whether a line is floating (reads intermediate or unstable voltage — missing pull-up or disconnected)

### Gotchas

- A DMM averages the reading. If the bus is actively communicating, the DMM shows an average voltage between high and low, which can look like a "stuck at mid-voltage" fault. Only trust DMM readings on idle or static buses

## Common Bus Failure Modes

### Bus Stuck Low

**Symptom:** One or more lines stuck near 0V, no toggling

| Possible cause | How to check |
|---------------|-------------|
| Shorted to ground (solder bridge, trace damage) | Disconnect all devices; measure resistance to ground. Low resistance = short |
| Device holding the line low (driver stuck on) | Remove devices one at a time and check if the line releases |
| Missing pull-up resistor (I2C/open-drain buses) | The line has nothing to pull it high. Check schematic for pull-ups; measure resistance from line to VCC |
| Bus contention (two drivers fighting) | Both a high and low driver are on simultaneously. Current flows, bus stays near ground or intermediate |

### Bus Stuck High

**Symptom:** Line stays high, never transitions low

| Possible cause | How to check |
|---------------|-------------|
| Device not driving (firmware not running, wrong pin config) | Verify firmware is running and GPIO is configured as output/alternate function |
| CS/enable not asserted | SPI slave won't respond if CS is high. Check CS line |
| Wrong pin (you're probing the wrong signal) | Double-check the pinout against schematic and PCB layout |

### Bus Floating

**Symptom:** Line at intermediate voltage, noisy, or randomly toggling

| Possible cause | How to check |
|---------------|-------------|
| Missing pull-up or pull-down | Check schematic; add external pull-up/down and see if the line stabilizes |
| Device not populated or not powered | Verify the device is present and has power |
| Broken trace or cold solder joint | Continuity check from device pin to the connector/test point |

### Gotchas

- On multi-device buses (I2C with many slaves), any single device can hold the bus low and prevent all communication. Isolate devices to find the culprit
- I2C bus lockup is a common failure: a slave holds SDA low waiting for a clock that never comes (interrupted transaction). The fix is to clock SCL manually until SDA releases, then issue a STOP condition. Some I2C masters have automatic bus recovery for this
