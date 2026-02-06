---
title: "Common Component Reference"
weight: 70
---

# Common Component Reference

A quick reference to building blocks commonly encountered in digital circuit design, emphasizing timing, edges, logic levels, and power integrity. Key parameters include propagation delay, setup/hold times, and voltage thresholds.

## Primitives

**Resistor** *(Primitive)*
Sets pull-up/pull-down strength and limits current through LEDs and protection networks. Power rating rarely matters in logic circuits; tolerance affects voltage divider accuracy in threshold-setting applications.

**Capacitor** *(Primitive)*
Decoupling, filtering, and timing. Ceramic types dominate — X5R/X7R for bulk decoupling, C0G for precision timing. ESR and ESL matter for high-speed power integrity.

**MOSFET** *(Primitive)*
Used as a level shifter, power switch, or load switch. Key parameters are Vth, RDS(on), and gate charge. Logic-level threshold MOSFETs switch fully on with 3.3 V gate drive; standard threshold types need higher voltage.

**Crystal/resonator** *(Primitive)*
Precision frequency-determining element. Key parameters are frequency, load capacitance, ESR, and temperature stability. Wrong load capacitance shifts frequency or prevents startup.

**Connector** *(Primitive)*
Mechanical-electrical interface. Key parameters are pin count, current rating, mating cycles, and contact resistance. Intermittent connector failures are among the most common bench problems.

## Blocks

**Level shifter** *(Block)*
Translates signals between voltage domains (e.g., 3.3 V to 5 V). MOSFET-based bidirectional shifters work for I2C but may not handle fast SPI clocks. Resistor dividers are unidirectional (high-to-low only).

**Decoupling/bypass network** *(Block)*
Capacitors near IC power pins providing local charge storage and suppressing high-frequency noise. Place as close as possible to power/ground pins — trace inductance between cap and IC degrades effectiveness.

**Pull-up/pull-down network** *(Block)*
Holds signal lines at defined logic levels when undriven. Key parameters are resistance (trade-off between power and transition speed) and bus capacitance. Too strong wastes power; too weak causes slow edges and noise susceptibility.

**Crystal oscillator circuit** *(Block)*
Crystal combined with inverter gate (Pierce topology) and load capacitors. Key parameters are frequency accuracy, startup time, and phase noise. Load capacitors must match crystal's specified load capacitance.

**Logic gate** *(Block)*
Elementary combinational logic (AND, OR, NOT, XOR) in a specific logic family (74HC, 74LVC, etc.). Key parameters are propagation delay, voltage thresholds, and fanout. Discrete gates are rare in new designs but remain essential for level translation and glue logic.

**ESD/TVS clamp** *(Block)*
Transient voltage suppressor protecting external interfaces. Key parameters are standoff voltage, clamping voltage, peak current, and capacitance (which matters for high-speed data lines).

## Subsystems

**Reset/supervisor** *(Subsystem)*
Monitors supply voltage and asserts reset when below threshold. Key parameters are threshold voltage, reset delay, and output polarity. Prevents the processor from executing random instructions during brown-out.

**Clock distribution** *(Subsystem)*
Takes a single clock source and distributes it to multiple destinations with controlled skew and jitter. Key parameters are output frequency, jitter, skew between outputs, and output format (LVCMOS, LVDS). At high frequencies, trace length matching and impedance control matter as much as the buffer IC.

**UART/serial** *(Subsystem)*
Asynchronous serial communication with start/stop framing. Key parameters are baud rate and voltage levels. Both ends must agree on baud rate within a few percent — clock mismatch is the most common cause of garbled data.

**SPI** *(Subsystem)*
Synchronous serial bus with separate clock, data, and chip-select. Key parameters are clock frequency and clock mode (CPOL/CPHA). The four possible clock mode combinations are a constant source of confusion — mode mismatch produces garbage that looks almost valid.

**I2C** *(Subsystem)*
Two-wire synchronous serial bus with multi-device addressing. Key parameters are clock speed (100/400/1000 kHz) and bus capacitance. Long traces and many devices slow rising edges beyond spec.

## Devices

**MCU development board** *(Device)*
MCU plus power regulation, programming interface, and pin headers. Key parameters are MCU family, peripheral set, and development environment support. The dev board is for prototyping; production uses a custom board with only needed peripherals.

**FPGA development board** *(Device)*
FPGA plus power sequencing, configuration flash, clock sources, and I/O headers. Key parameters are FPGA size, I/O standards, and onboard memory. Power sequencing is handled by the board but matters when debugging startup issues.

**Logic level converter module** *(Device)*
Bidirectional level shifter on a breakout board, typically MOSFET-based. Speed is the common limitation — cheap modules work for I2C but may not handle fast SPI.

## Systems

**Multi-rail power distribution system** *(System)*
Generates, sequences, and distributes multiple voltage rails. Key parameters are rail voltages, sequencing order, and fault response. Load transients on one rail can couple into others through shared input capacitance or PCB impedance.
