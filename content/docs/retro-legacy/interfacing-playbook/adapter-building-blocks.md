---
title: "Common Adapter Building Blocks"
weight: 30
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Common Adapter Building Blocks

These are the standard circuit patterns for bridging domains. Mix and match based on the [domain identification checklist]({{< relref "domain-identification-checklist" >}}) results.

## Level Shifting

**Unidirectional, high-to-low (most common case):**
- Resistive voltage divider — two resistors, works for signals up to ~1 MHz. Output impedance is the parallel combination of the two resistors, which limits drive strength and bandwidth
- Series resistor + clamp to VDD -- a resistor (1-10 kohm) in series with the signal, relying on the MCU's internal clamp diode to limit voltage to VDD + 0.3V. The resistor limits clamp current to safe levels. Simple and effective for slow signals where the clamp current is milliamps or less

**Unidirectional, low-to-high:**
- Open-drain/open-collector output with pull-up to the higher voltage. The MCU pulls the line low; the pull-up brings it to the legacy system's voltage when the MCU releases it. Works for slow signals and buses
- Buffer IC powered from the higher voltage with input compatible to the lower voltage (e.g., 74HCT series accepts 3.3V inputs when powered from 5V, because TTL input thresholds are low enough)

**Bidirectional:**
- BSS138 MOSFET translator circuit — the standard for I2C and other open-drain bidirectional buses. One MOSFET per signal line, pull-ups on both sides. Documented in NXP AN10441
- Dedicated bidirectional translator ICs (TXB0108, TXS0102) — faster and smaller than the MOSFET circuit, but problematic with open-drain buses and slow edges

## Buffering and Impedance Bridging

When the source impedance and load impedance are mismatched, or when the source cannot drive the load without distortion:

- **Unity-gain buffer (op-amp voltage follower):** High input impedance, low output impedance. Useful for bridging a high-impedance legacy source (like a tube-era audio output) to a low-impedance modern input. Choose the op-amp for the voltage range (rail-to-rail if the signal approaches the supply rails) and bandwidth
- **Emitter follower / source follower:** The discrete equivalent. A single transistor provides current gain without voltage gain. Simple, fast, and works at voltages above the MCU's supply
- **Series termination resistor:** For transmission-line effects on long cables (>1/10 wavelength at the signal's edge frequency). A resistor at the source matching the cable impedance prevents reflections. Standard on RS-485, Ethernet, and any interface designed for cable runs

## Galvanic Isolation

Use when the [domain identification checklist]({{< relref "domain-identification-checklist" >}}) reveals safety risk, ground loops, or ground offsets that exceed the MCU's tolerance range.

**Optocouplers:** Cheapest isolation for slow signals (<50 kHz for standard parts, up to 25 Mbaud for high-speed types). LED drive current degrades over time, so design with CTR margin. Best for: relay replacement, slow digital signals, isolated status indicators.

**Digital isolators (capacitive or magnetic):** Faster (up to 150 Mbaud), lower power, no degradation over time. Available with integrated isolated power. Best for: SPI, I2C, UART, CAN, any high-speed digital interface where isolation is needed.

**Isolation transformers:** Best for AC signals (audio) and power transfer. Provide inherent common-mode rejection for the signal. Audio isolation transformers break ground loops without digitizing the signal. Best for: audio interfaces, Ethernet (already transformer-isolated by design), pulse/gate drive.

**Isolated DC-DC converters:** When the isolated side needs its own power rail, a small isolated DC-DC module (Murata NME, Recom R1S) generates a secondary supply with galvanic isolation. Common in RS-485 interfaces, 4-20 mA loop adapters, and anywhere an interface circuit needs an isolated power domain. Without a truly separate supply, isolation is only on paper -- if both sides of an optocoupler share the same non-isolated DC-DC, the ground paths reconnect through the shared supply.

**When to use which:**

| Criterion | Optocoupler | Digital Isolator | Transformer |
|---|---|---|---|
| Signal type | Digital only | Digital only | AC analog or digital pulses |
| Speed | < 50 kHz (std), < 25 Mbaud (fast) | Up to 150 Mbaud | Up to ~100 MHz (pulse), 20 Hz–100 kHz (audio) |
| Needs isolated power? | Yes (external) | Optional (some have integrated) | Provides power transfer inherently |
| Lifetime concern | LED degrades (CTR drops) | No degradation | No degradation |
| Cost | Lowest | Moderate | Varies (audio: expensive; pulse: cheap) |
| Best for | Slow legacy signals, status I/O | Fast digital buses | Audio, power, pulse |

## Protection Components

At the interface boundary -- between the connector and the translation/isolation circuit:

- **Series resistors (100 ohm - 10 kohm):** Limit current during fault conditions. A 1 kohm resistor between a legacy output and an MCU input limits current to 12 mA at 12V -- within the ESD clamp's tolerance for most parts. Also forms an RC filter with the pin capacitance, slowing edges and reducing EMI. This is the single most effective protection component and should be on every interface pin
- **TVS diodes:** Clamp fast transients (ESD, inductive kick) that the series resistor cannot absorb alone. Bidirectional TVS for signals that swing both ways; unidirectional for DC-only. Place between the signal line and ground, after the series resistor, as close to the connector as possible
- **Schottky clamp diodes to supply rails:** External versions of the MCU's internal ESD clamps, but rated for higher current. Schottky diodes (BAT54S in a dual common-cathode package) from the signal to VDD and VSS clamp overvoltage and undervoltage
- **Fuses or PTC resettable fuses:** On power lines at the interface boundary. Protect the MCU's supply from a short or overvoltage on the legacy side. A PTC resets automatically after the fault clears
- **RC snubbers:** Across relay contacts or any mechanical switch interface. A series RC (100 ohm + 100 nF typical) damps contact arcing and reduces the EMI transient that the MCU sees

## Protocol Conversion

Physical-layer and framing differences between legacy and modern:

| Legacy Protocol | Physical Difference | Framing Difference | Standard Conversion |
|---|---|---|---|
| RS-232 | ±3V to ±15V, single-ended | Start/stop/parity bits (same as UART) | MAX3232 or USB-serial adapter |
| RS-485 | Differential ±1.5V to ±6V | Application-dependent (Modbus, custom) | RS-485 transceiver IC (MAX485, SN65HVD72) |
| 4–20 mA | Current loop, not voltage | Analog (continuous) or HART (FSK modulated) | 250 ohm sense resistor → ADC |
| Relay/contact | Dry contact or voltage output | On/off (no framing) | Optocoupler input or voltage divider + GPIO |
| Analog audio | AC, ±1V to ±10V, 600 ohm or high-Z | Continuous analog | Op-amp buffer + ADC, or isolation transformer |
| Parallel port | TTL levels, active-low strobes, directly memory-mapped I/O | Handshake-based (nStrobe/nAck) | FTDI FT232H in MPSSE mode or MCU bit-bang. Legacy software often assumes direct I/O port access (inb/outb), which USB adapters do not support -- may require a shim driver or a dedicated PCIe parallel card |
| MIDI | 5V current loop, opto-isolated by spec | 31.25 kbaud serial | UART at 31250 baud + optocoupler on input |

## Tips

- Place a series resistor (100 ohm to 1 kohm) on every interface pin as a default practice -- this single component limits fault current, forms an RC low-pass with pin capacitance, and reduces EMI coupling at negligible performance cost for signals under 1 MHz
- Use isolated DC-DC modules (Murata NME, Recom R1S) to power the isolated side of optocoupler or digital isolator circuits -- without a truly separate supply, isolation is only on paper

## Caveats

- **Isolation does not help if the isolated supply shares the same input power** -- If both sides of an optocoupler are powered from the same wall adapter through a non-isolated DC-DC module, there is no real isolation -- the ground paths connect through the shared supply. True isolation requires separate power domains or a certified isolated DC-DC converter
- **Auto-direction level translators (TXB0108 family) fail on buses with pull-ups** -- They interpret the pull-up as a driven signal and fight it, causing oscillation or stuck states. Use MOSFET-based translators for I2C, 1-Wire, or any open-drain bus
- **Optocoupler CTR degrades over time** -- A circuit that works with a new optocoupler may fail in 3-5 years as the internal LED dims. Design the drive current with 2-3x margin over the minimum required, or use digital isolators, which have no degradation mechanism

## In Practice

- **A level-translated I2C bus that shows erratic behavior or stuck lines** often indicates an auto-direction translator (TXB0108 family) fighting the bus pull-ups -- replacing with a BSS138 MOSFET translator circuit resolves the issue
