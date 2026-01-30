---
title: "Is the Device ACKing / Responding?"
weight: 30
---

# Is the Device ACKing / Responding?

Protocol-level verification. You can see bus activity, but is the device actually responding? I2C ACK/NACK, SPI response bytes, UART framing — using protocol decode to see what's actually on the wire, not just that something is toggling.

## Oscilloscope Protocol Decode

**Tool:** Oscilloscope with protocol decode feature (most modern scopes have I2C, SPI, UART decode)
**When:** You need to see actual data bytes, addresses, and ACK/NACK status

### I2C Decode

1. Connect CH1 to SCL, CH2 to SDA
2. Enable I2C protocol decode (set the correct channels for SCL and SDA)
3. Set the threshold voltage (typically VCC/2)
4. The scope overlays decoded bytes on the waveform: address, R/W bit, ACK/NACK, data bytes

### What to Look For

| Decoded element | Meaning |
|----------------|---------|
| Address byte + ACK | Device is present and responding at that address |
| Address byte + NACK | No device at that address, or device is busy/stuck |
| Data bytes with ACK | Successful data transfer |
| Repeated START | Master is chaining transactions (common for register read: write address, then read data) |
| STOP | Transaction complete |

### SPI Decode

1. Connect channels to SCK, MOSI, MISO, and CS (4 signals; if your scope has fewer channels, prioritize SCK + one data line + CS)
2. Enable SPI decode — set clock polarity (CPOL), clock phase (CPHA), bit order (MSB/LSB first), and word size (usually 8 bits)
3. The scope shows decoded bytes on MOSI and MISO, framed by CS

### What to Look For

| Decoded element | Meaning |
|----------------|---------|
| MOSI shows command bytes, MISO shows 0x00 or 0xFF | Device isn't responding (MISO is idle, not driven) |
| MOSI shows command, MISO shows data | Device is responding — check if the data makes sense |
| MISO always returns same value regardless of command | Device may be stuck, in reset, or in wrong mode |
| CS never goes low | Master isn't selecting the device — check CS line and firmware |

### UART Decode

1. Connect a channel to the TX or RX line
2. Enable UART decode — set baud rate, data bits (8), parity (usually none), stop bits (1)
3. The scope shows decoded characters or hex bytes

### What to Look For

| Decoded element | Meaning |
|----------------|---------|
| Readable ASCII or expected hex bytes | Communication is working at the correct baud rate |
| Garbled or random characters | Baud rate mismatch — try common rates: 9600, 115200, etc. |
| Framing errors in decode | Wrong baud rate, wrong number of data/stop bits, or noise on the line |
| No decode at all | No data on the line, or signal amplitude too low for the decode threshold |

### Gotchas

- Protocol decode settings must match the device configuration exactly. Wrong CPOL/CPHA on SPI, wrong baud rate on UART, or wrong threshold on I2C will give garbled or missing decode
- Some scopes have limited decode depth — they may only decode what's visible on screen. Zoom out or use a logic analyzer for long captures
- Protocol decode doesn't replace understanding the protocol. Seeing "ACK" on I2C means the device acknowledged the address byte, but it doesn't mean the data that follows is correct

## Logic Analyzer: Extended Capture

**Tool:** Logic analyzer (standalone or USB-based like Saleae, sigrok-compatible devices)
**When:** You need long captures, many channels, or detailed protocol analysis beyond what the scope provides

### When to Use a Logic Analyzer Instead of a Scope

| Scenario | Better tool |
|----------|------------|
| Quick check: is the bus active? | Scope |
| Check voltage levels and signal quality | Scope |
| Capture and decode an entire transaction sequence | Logic analyzer |
| Monitor bus activity over seconds or minutes | Logic analyzer |
| Decode multiple buses simultaneously | Logic analyzer |
| Check timing margins (setup/hold) with voltage detail | Scope |

### Procedure

1. Connect logic analyzer probes to the bus signals
2. Configure protocol decoder (I2C, SPI, UART, CAN, etc.)
3. Set trigger if needed (trigger on specific address, start condition, etc.)
4. Capture and analyze the decoded traffic

### What You Learn

- Full transaction-level view: every byte sent and received, with timestamps
- Error patterns: intermittent NACKs, retries, timeouts
- Timing between transactions — useful for diagnosing firmware state machine issues

### Gotchas

- Logic analyzers have fixed voltage thresholds (typically configurable: 1.8V, 3.3V, 5V). They don't show analog signal quality — a marginal signal that barely crosses the threshold looks the same as a clean one. Use a scope for signal quality, a logic analyzer for protocol content
- Sample rate must be at least 4× the clock frequency (10× preferred) for reliable decode. A 100 MHz SPI clock needs a 400 MHz+ logic analyzer
- Cheap logic analyzers may have limited sample rate or buffer depth. Check specs before assuming they can handle your bus speed

## Debugging Common Protocol Issues

### I2C: Device Not ACKing

1. Verify the correct 7-bit address (not shifted — some datasheets show 8-bit address with R/W bit included, some show 7-bit)
2. Check pull-up resistors — too high value gives slow rise times that may not reach VIH before the clock edge. Typical: 4.7 kOhm for 100 kHz, 2.2 kOhm for 400 kHz
3. Verify the device has power and is out of reset
4. Check for address conflicts — two devices at the same address can cause one to NACK

### SPI: No Response on MISO

1. Verify CS is actually going low (active low is the convention for most SPI devices)
2. Check CPOL and CPHA settings — wrong clock phase means the device samples data at the wrong time and sends garbage or nothing
3. Verify MISO is connected and not tri-stated (some devices tri-state MISO when CS is high — normal, but check it goes active when CS is low)
4. Some SPI devices need dummy bytes or wait cycles before they respond. Check the device datasheet for command-response timing

### UART: Garbled Data

1. Confirm baud rate matches on both ends — this is the #1 UART problem
2. Check voltage levels — RS-232 levels (±12V) are different from TTL/CMOS UART levels (0/3.3V or 0/5V). A level converter (MAX232 or equivalent) is needed between them
3. Verify TX connects to RX and vice versa (crossover). A straight connection between two devices requires TX→RX, not TX→TX
4. Check that both ends agree on data format: 8N1 (8 data bits, no parity, 1 stop bit) is the most common default
