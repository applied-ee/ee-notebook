---
title: "Are Clock and Data Edges Aligned?"
weight: 40
---

# Are Clock and Data Edges Aligned?

Setup and hold timing. The data must be stable before the clock edge (setup time) and remain stable after it (hold time). When these margins are violated, the receiver captures wrong or metastable data — and the failure is often intermittent, temperature-dependent, and maddening.

## Setup and Hold: The Fundamentals

**Concept:** Timing relationships between clock and data

| Parameter | Definition | What happens if violated |
|-----------|-----------|------------------------|
| Setup time (tSU) | Minimum time data must be stable before the active clock edge | Data may not be captured correctly — wrong bit value |
| Hold time (tH) | Minimum time data must remain stable after the active clock edge | Captured value may change after being latched — metastability |

Both are specified in the receiver's datasheet. The transmitter's datasheet specifies when data transitions occur relative to the clock — the difference between these is your timing margin.

### Timing Margin

**Setup margin = (time data is stable before clock edge) − tSU_required**

**Hold margin = (time data remains stable after clock edge) − tH_required**

Both margins must be positive for reliable operation.

## Oscilloscope: Measuring Setup and Hold

**Tool:** Oscilloscope, 2+ channels, triggered on clock edge
**When:** Verifying timing margins on SPI, I2C, parallel buses, or any clocked data interface

### Procedure

1. CH1 on the clock signal, CH2 on the data signal
2. Trigger on the active clock edge (the edge where data is sampled — rising or falling depending on the protocol)
3. Zoom the timebase to see the clock edge and the surrounding data transitions clearly (typically ns/div for SPI/I2C)
4. Measure the time from the data transition to the clock edge (setup time) and from the clock edge to the next data transition (hold time)
5. Use cursors for manual measurement, or the scope's timing measurement functions if available

### Reading the Waveform

On the scope display, with trigger on the clock's active edge:

- The data line should be flat (stable) before and after the clock edge
- The data transitions should happen during the "don't care" window — between one sample point and the next
- If a data transition happens close to the clock edge, timing margin is tight

### What You Learn

- Actual setup and hold times vs. the receiver's requirements
- Which signals have the tightest margins — these are the ones that will fail first at higher speeds or different temperatures

### Gotchas

- Propagation delay through the PCB traces, buffers, and level shifters adds to the clock-to-data skew. Measure at the receiver's input pins, not at the transmitter's output
- Both clock and data have rise/fall times — the "edge" isn't instantaneous. Measure timing from the threshold crossing point (typically 50% of swing), not from the start or end of the transition
- Probe delay mismatch between channels can introduce apparent skew that isn't real. Use matched probes and cables, or measure the channel-to-channel skew on the same signal and compensate

## SPI Timing

### Clock Polarity and Phase (CPOL / CPHA)

| Mode | CPOL | CPHA | Clock idle | Data sampled on | Data shifted on |
|------|------|------|-----------|----------------|----------------|
| 0 | 0 | 0 | Low | Rising edge | Falling edge |
| 1 | 0 | 1 | Low | Falling edge | Rising edge |
| 2 | 1 | 0 | High | Falling edge | Rising edge |
| 3 | 1 | 1 | High | Rising edge | Falling edge |

### What to Verify

1. **CPOL:** Does the clock idle at the expected level between transactions (when CS is high)?
2. **CPHA:** Is data valid (stable) at the sampling edge?
3. **CS-to-clock timing:** Some devices require CS to go low a minimum time before the first clock edge
4. **Clock-to-CS-release timing:** Some devices require CS to stay low for a minimum time after the last clock edge

### Gotchas

- CPOL/CPHA mismatch between master and slave is the #1 SPI problem. The bus "works" (you see toggling) but data is shifted by one bit — the master reads the previous bit instead of the current one
- At high SPI clock rates (>10 MHz), trace length differences between SCK and data lines become significant. A few centimeters of extra trace adds nanoseconds of delay that can eat into setup/hold margins

## I2C Timing

### Key Timing Parameters

| Parameter | Standard mode (100 kHz) | Fast mode (400 kHz) | Fast mode+ (1 MHz) |
|-----------|------------------------|--------------------|--------------------|
| SCL clock period | ≥ 10 µs | ≥ 2.5 µs | ≥ 1 µs |
| Setup time (data) | ≥ 250 ns | ≥ 100 ns | ≥ 50 ns |
| Hold time (data) | ≥ 0 ns | ≥ 0 ns | ≥ 0 ns |
| Rise time (SCL, SDA) | ≤ 1000 ns | ≤ 300 ns | ≤ 120 ns |

### What to Verify

1. **Rise times:** Controlled by pull-up resistor value and bus capacitance. Slow rise times eat into the valid data window. Calculate: **t_rise ≈ 0.8473 × R_pullup × C_bus**
2. **SDA changes only when SCL is low:** SDA changing while SCL is high is a START or STOP condition, not data
3. **Clock stretching:** Slow devices can hold SCL low to pause the master. Verify the master waits

### Gotchas

- Bus capacitance accumulates with every device and every centimeter of trace. The I2C spec limits total bus capacitance to 400 pF (standard/fast mode). Exceeding this gives rise times that violate timing specs
- Pull-up resistor selection is a trade-off: lower resistance gives faster rise times (better timing margin) but higher current draw and lower noise immunity. There's an optimal range for each bus speed and capacitance
- At Fast mode+ (1 MHz), timing margins are very tight. Layout quality matters — keep traces short and route SCL and SDA close to their pull-ups

## Persistence Display: Visualizing Timing Jitter

**Tool:** Oscilloscope with infinite persistence, triggered on clock edge
**When:** Checking whether timing margins are consistent or varying

### Procedure

1. Trigger on the clock's active edge
2. Enable infinite persistence
3. Let the display accumulate many acquisitions
4. Data transitions appear as a band — the width of this band shows the timing jitter

### What You Learn

- Whether timing is tight but consistent (narrow band, safe) or jittery (wide band, risky)
- Whether there are occasional outlier transitions that come dangerously close to the clock edge

### Gotchas

- Persistence shows the distribution of transitions but doesn't quantify margin directly. You still need to measure the worst-case transition timing against the setup/hold specs
- Temperature and voltage variations cause timing to shift. Room-temperature measurements may look fine, but at temperature extremes the margins may close
