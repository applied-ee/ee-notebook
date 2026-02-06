---
title: "Common Component Reference"
weight: 80
---

# Common Component Reference

A quick reference to building blocks commonly encountered in embedded system design, emphasizing integration, power budget, and peripheral interaction. Key parameters include current consumption and interface compatibility.

## Primitives

**Resistor** *(Primitive)*
Pull-ups, current limiting, voltage dividers. Power rating matters when driving LEDs at high brightness or sensing current.

**Capacitor** *(Primitive)*
Decoupling near every IC, bulk storage on power rails, timing with RC circuits. Ceramic X5R/X7R for decoupling, electrolytic or tantalum for bulk. Voltage derating affects actual capacitance in ceramics.

**Crystal/resonator** *(Primitive)*
Clock source for MCU and communication peripherals. Key parameters are frequency, load capacitance, and startup time. Wrong load capacitors cause frequency error or failure to oscillate.

**Connector** *(Primitive)*
Board-to-board, wire-to-board, and programming/debug interfaces. Key parameters are current rating, mating cycles, and pin pitch. Connector failures cause the most frustrating intermittent bugs.

**MOSFET** *(Primitive)*
Load switching, level shifting, and reverse polarity protection. Logic-level threshold essential for direct MCU drive at 3.3 V.

**Fuse** *(Primitive)*
Overcurrent protection. Key parameters are current rating, voltage rating, and speed. Slow-blow fuses tolerate inrush current; fast-blow fuses protect semiconductors.

## Blocks

**Level shifter** *(Block)*
Connects 3.3 V MCU to 5 V peripherals or legacy devices. Bidirectional MOSFET-based shifters work for I2C; SPI may need faster dedicated ICs.

**Decoupling/bypass network** *(Block)*
Every IC needs local decoupling — 100 nF ceramic per power pin is the starting point. Place as close as possible; trace inductance matters.

**Pull-up/pull-down network** *(Block)*
Defines default states for I2C bus, chip selects, and reset lines. 4.7 kΩ typical for I2C; lower values for faster edges at the cost of power.

**Crystal oscillator circuit** *(Block)*
MCU crystal plus load capacitors. Most MCUs specify load capacitance — get it wrong and the clock runs off-frequency or fails to start.

**ESD/TVS clamp** *(Block)*
Protects external interfaces (USB, Ethernet, GPIO headers) from static discharge. Capacitance matters for high-speed signals.

**Reverse polarity protection** *(Block)*
Prevents damage when power is connected backwards. Series diode is simple but lossy (0.3–0.7 V drop); P-channel MOSFET drops millivolts but adds complexity.

## Subsystems

**Linear regulator** *(Subsystem)*
Drops input voltage with low noise but wastes power as heat. Key parameters are dropout voltage, quiescent current, and PSRR. Good for analog and noise-sensitive supplies; inefficient for large voltage drops.

**Buck converter** *(Subsystem)*
Steps down voltage efficiently. Key parameters are input/output range, efficiency, ripple, and EMI. Layout is critical — poor layout causes noise and instability.

**Boost converter** *(Subsystem)*
Steps up voltage from batteries or low-voltage sources. Input current exceeds output current — size input capacitors and traces accordingly.

**Reset/supervisor** *(Subsystem)*
Holds MCU in reset during power-up and brown-out. Key parameters are threshold voltage and reset delay. Essential for reliable operation — without it, the MCU may execute random instructions on unstable supply.

**Battery management** *(Subsystem)*
Charging, protection (over-voltage, under-voltage, over-current, over-temperature), and cell balancing for multi-cell packs. Getting lithium charge termination wrong is a safety issue.

**ADC front end** *(Subsystem)*
Signal conditioning between sensor and MCU's ADC: filtering, buffering, level shifting. Anti-alias filter must be designed for the actual sample rate.

**UART/serial** *(Subsystem)*
The universal debug interface. Key parameters are baud rate and voltage level (3.3 V TTL, 5 V TTL, RS-232). Clock mismatch causes garbled data.

**SPI** *(Subsystem)*
Fast synchronous peripheral interface. Key parameters are clock frequency, clock mode, and chip-select polarity. Mode mismatch produces almost-valid-looking garbage.

**I2C** *(Subsystem)*
Two-wire peripheral bus with addressing. Key parameters are clock speed and bus capacitance. Many devices on long traces slow rising edges — reduce pull-up resistance or reduce device count.

**USB interface** *(Subsystem)*
Standard connectivity to computers. Key parameters are speed class (Full/High/Super), device class (CDC, HID, MSC), and power capability. Implementation is mostly in silicon — the concern is choosing the right device class and handling enumeration.

**Power sequencer** *(Subsystem)*
Ensures multiple rails come up in the correct order. Key parameters are sequencing delay and fault response. Required for complex devices like FPGAs where incorrect sequencing can cause latch-up.

## Devices

**MCU development board** *(Device)*
MCU plus power regulation, debug interface, and headers. Key parameters are MCU family, peripheral set, and toolchain support. Use for prototyping; design custom boards for production.

**Wireless module** *(Device)*
RF transceiver (often with integrated MCU) for Wi-Fi, Bluetooth, LoRa, or other protocols. Key parameters are protocol, range, power consumption, and host interface. Antenna placement dramatically affects real-world range.

**GPS/GNSS module** *(Device)*
Satellite receiver outputting position and time over serial. Key parameters are constellation support, accuracy, and time-to-first-fix. Needs clear sky view — indoor or obstructed antenna causes long fix times or no fix.

**Display module** *(Device)*
Character LCD, OLED, or TFT with integrated controller. Key parameters are resolution, interface (I2C, SPI, parallel), and power consumption. I2C is simpler but slower; SPI handles faster refresh.

**USB-to-serial adapter** *(Device)*
Bridges USB to TTL UART for debug and programming. Key parameters are supported baud rates and driver compatibility. Driver issues with cheap clones remain common.

**Sensor module/breakout board** *(Device)*
Sensor IC plus minimal support circuitry for prototyping. Convenient but layout may not match production requirements.

**Battery pack/holder** *(Device)*
Cells plus mechanical connection and possibly protection BMS. Key parameters are chemistry, voltage, capacity, and protection features. Unprotected lithium cells need external protection.

## Systems

**Battery-powered embedded system** *(System)*
MCU, sensors, wireless, and power management designed for field deployment. Power budget dominates — every subsystem's sleep current matters. A single device that fails to enter low-power mode cuts battery life from months to days.

**Wireless sensor network** *(System)*
Multiple nodes communicating with a gateway. Key parameters are node count, topology (star, mesh), protocol, and per-node power budget. Reliability challenges multiply with node count.

**Sensor-to-display chain** *(System)*
Complete path from physical quantity through sensing, processing, and display. The error budget exercise allocates allowable error across each stage.

**Environmental monitoring system** *(System)*
Multi-sensor data collection, logging, and optionally alerting. Key parameters are sensor count, sample interval, storage, and power budget. Sample rate trades directly against battery life.
