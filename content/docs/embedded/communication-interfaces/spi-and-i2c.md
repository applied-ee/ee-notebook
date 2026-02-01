---
title: "SPI & I2C"
weight: 10
---

# SPI & I2C

The two synchronous serial buses that show up on nearly every embedded project. SPI is fast and simple — a master-driven clock, full-duplex, one chip select per device. I2C is slower but pin-efficient — two shared wires for the whole bus, with addressing built into the protocol. The digital electronics section covers their [bus-level protocols and timing]({{< relref "/docs/digital/data-transfer-and-buses/common-bus-protocols" >}}); this page focuses on the MCU side.

## SPI

Synchronous, full-duplex, fast. The master generates the clock, so there is no baud rate matching problem — the slave just follows the clock edges.

```
  MCU          MOSI ──┬────────┬────────┐
  (Master)     MISO ──┤────────┤────────┤  (shared)
               SCK  ──┤────────┤────────┤
                      │        │        │
               CS0  ──┘        │        │
               CS1  ───────────┘        │  (one per device)
               CS2  ────────────────────┘
                    Flash     ADC    Display
```

MOSI, MISO, and SCK are shared — all slaves see the same clock and data lines. Chip select lines are individual: only the device whose CS is pulled LOW responds. The master controls CS via GPIO, selecting one device at a time. Devices with CS HIGH ignore the bus and tri-state their DO output.

### Configuration Essentials

Four clock modes exist, defined by CPOL (clock idle polarity) and CPHA (which edge samples data). Mode 0 (CPOL=0, CPHA=0) is the most common, but the device datasheet is the only reliable source. Getting the mode wrong produces garbled data that can look almost right — shifted by one bit, or with the MSB and LSB swapped — which is surprisingly hard to diagnose without a logic analyzer.

Chip select is almost always active-low. Many MCUs offer a hardware-managed CS pin, but in practice most firmware controls CS via GPIO. The reason: hardware CS often deasserts between bytes, which breaks devices that expect CS held low for the entire multi-byte transaction. GPIO control gives firmware full authority over when CS goes low and high.

SPI clock speeds commonly reach 10-40 MHz, and some peripherals support faster. At these speeds, PCB trace length and capacitance start to matter. The MCU side is straightforward — set the prescaler, pick the mode — but the electrical reality depends on the board.

### Data Transfer

SPI is fundamentally a pair of shift registers exchanging data simultaneously. Every byte sent produces a byte received, even if you do not care about it. This means a "read" operation from a sensor typically involves sending a command byte (real data out) and then sending dummy bytes while clocking the response back in. Ignoring the received data during a write, or the transmitted data during a read, is fine — but you still have to clock both directions.

There is no addressing, no ACK, and no flow control. The selected device responds because its CS is asserted, and it has no mechanism to say "wait" or "error." If the slave is not ready, the master gets whatever happens to be on MISO — usually 0xFF or 0x00.

## I2C

Synchronous, half-duplex, two wires, addressing built in. I2C is the go-to bus for connecting a handful of slow peripherals — sensors, EEPROMs, RTCs, I/O expanders — with minimal pin count.

```
  SDA ──┬──────────┬──────────┬──────────┬── Rp ── VDD
  SCL ──┼──────────┼──────────┼──────────┼── Rp ── VDD
        │          │          │          │
       MCU      EEPROM      RTC      Sensor
     (master)    0x50       0x68      0x48
```

All devices share the same two wires — SDA (data) and SCL (clock). Pull-up resistors to VDD are required because all drivers are open-drain. The master addresses each device by its 7-bit address; no chip select lines needed. Any number of devices can share the bus as long as addresses don't conflict.

### Configuration Essentials

The MCU's I2C peripheral handles start/stop conditions, addressing, and ACK/NACK generation in hardware. Firmware configures the speed grade (100 kHz standard, 400 kHz fast, 1 MHz fast-plus) and the peripheral clock source. The I2C timing registers can be surprisingly complex — some MCUs expose separate rise time, fall time, data hold, and setup parameters rather than a single "speed" setting. STM32 MCUs, for example, have a timing register that is best computed using ST's tool rather than by hand.

Pull-up resistors are external and mandatory. The bus is open-drain: devices can only pull lines low, and the pull-ups bring them back high. Pull-up value depends on bus capacitance and speed — 4.7k ohm is a common starting point for 100 kHz, but 400 kHz or longer traces may need 2.2k or lower. Getting this wrong produces slow, rounded rising edges that cause intermittent communication failures. An oscilloscope on SDA and SCL immediately reveals whether the pull-ups are sized correctly.

### Clock Stretching

A slave can hold SCL low to pause the bus while it processes data. This is called clock stretching, and it is part of the I2C specification — but not all masters handle it properly, and not all slaves do it predictably. Some sensors stretch the clock for milliseconds during a conversion, which stalls the entire bus. In firmware, this appears as the I2C transfer taking much longer than expected, or as a timeout if you have one configured.

### Addressing and Bus Management

Each device has a 7-bit address (10-bit addressing exists but is rare). Many devices have partially configurable addresses — a few pins set the lower bits — but address conflicts are a real integration problem. Two identical sensors on the same bus need either different address pin configurations, an I2C multiplexer (like the TCA9548A), or separate buses.

The ACK/NACK mechanism provides basic error feedback: if no device responds to an address, the master sees a NACK and can flag an error. This is the simplest way to detect a missing or dead device.

## Choosing Between SPI and I2C

**SPI** — fast chip-to-chip: flash memory, displays, ADCs, DACs, radio transceivers. Pin-hungry (one CS per device) but dead simple electrically and very fast.

**I2C** — slow multi-device buses: sensors, EEPROMs, RTCs. Two wires for the whole bus regardless of device count, but limited speed and half-duplex.

If the peripheral offers both I2C and SPI, and you have the pins, SPI is almost always easier to bring up and debug. I2C's strength is pin efficiency, not ease of use.

## Data Handling

**Polling** works for low-speed I2C reads and short SPI transfers, but the CPU does nothing else while waiting. For SPI at 10 MHz, polling wastes most of the CPU's time on tight loops.

**Interrupts** let the CPU do other work between bytes. The peripheral raises an interrupt when data is ready (RX) or when the transmit buffer is empty (TX). The ISR moves data between the peripheral and a software buffer. This is the standard approach for I2C.

**DMA** moves data without CPU involvement at all. Essential for high-throughput SPI transfers — reading a flash chip, driving a display, or streaming ADC data. More on how DMA works in [DMA Fundamentals]({{< relref "dma" >}}).

## Debugging SPI and I2C

A logic analyzer with protocol decoding is the single most useful tool for SPI and I2C problems. Reading MCU status registers can tell you that a transfer failed, but not why. Seeing the actual waveforms — clock edges, data transitions, CS timing — reveals whether the issue is configuration (wrong clock mode), electrical (slow rise times), or firmware (CS toggled at the wrong time). Even a cheap 8-channel logic analyzer with protocol decoding changes serial bus debugging from guesswork to direct observation.

For I2C specifically, an oscilloscope is also valuable because rise time and signal shape matter. A logic analyzer shows you the decoded bits; an oscilloscope shows you whether the pull-ups are adequate. Both views are useful. See [Probing & Measurement Technique]({{< relref "/docs/measurement/probing-technique" >}}) for connection details.

## Gotchas

- **Wrong SPI clock mode looks almost right** — A CPOL or CPHA mismatch often produces data that is bit-shifted or byte-shifted rather than completely garbled. This makes it tempting to "fix" the issue in software rather than checking the mode setting against the datasheet. Always check the mode first.
- **I2C pull-up resistor value is not a suggestion** — Too high and the rising edges are too slow for the bus speed. Too low and weak devices cannot pull the bus down. The result in both cases is intermittent NACK or corrupted data, and it only shows up under specific conditions (temperature, bus load, specific device combinations).
- **Hardware CS on SPI often misbehaves for multi-byte transfers** — Many MCU SPI peripherals deassert CS between each byte when using hardware chip select. Most SPI devices expect CS held low for the entire transaction. Use GPIO-controlled CS unless you have verified the hardware CS behavior matches the device requirement.
- **I2C bus lockup is a real failure mode** — If a slave gets confused mid-transfer (due to noise, reset, or a firmware bug), it can hold SDA low indefinitely, locking up the entire bus. Recovery requires clocking SCL manually until the slave releases SDA — something the MCU's I2C peripheral usually cannot do automatically.
