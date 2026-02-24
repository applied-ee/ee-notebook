---
title: "Worked Mini-Patterns"
weight: 50
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Worked Mini-Patterns

These are conceptual interface patterns -- not complete schematics, but the essential topology for each common scenario.

## MCU ↔ RS-232

**The problem:** RS-232 swings ±3V to ±15V. MCU UART is 0/3.3V. Direct connection destroys the MCU.

**Pattern:**

```
RS-232 device                              MCU (3.3V)
  TX ────── MAX3232 R_IN ─── R_OUT ──── UART RX
  RX ────── MAX3232 T_OUT ── T_IN ───── UART TX
  GND ───── MAX3232 GND ─────────────── GND
                     │
                   3.3V (C1–C4 charge pump caps per datasheet)
```

- The MAX3232 handles all level translation and generates ±6V from 3.3V using internal charge pumps
- For one-off bench connections, a USB-serial adapter (FTDI, CP2102) replaces all of this with a single module
- If the RS-232 device is far away (long cable), add TVS diodes (SM712 or similar) on the RS-232 side for transient protection

**Watch for:** Null modem vs. straight-through wiring. If the legacy device is DTE (like a PC), it expects DCE pinout on the cable -- TX on pin 3, RX on pin 2. If both devices are DTE, a null modem crossover (TX to RX, RX to TX) is needed. Getting this wrong does not cause damage but nothing communicates.

## MCU ↔ 4–20 mA Current Loop

**The problem:** The legacy transmitter outputs a current proportional to a measured value (4 mA = 0%, 20 mA = 100%). The MCU needs to read this as a voltage with its ADC.

**Pattern (receiver, loop-powered by external supply):**

```
Loop supply (+24V) ─── 4–20 mA transmitter ──┬── MCU ADC input
                                               │
                                          250Ω (precision)
                                               │
                                              GND (shared)
```

- The 250 ohm sense resistor converts 4-20 mA to 1-5V, which is within the ADC's input range on a 3.3V MCU with a 5V-tolerant ADC (or use a lower value resistor for lower voltage range)
- Use a precision resistor (0.1% tolerance, low tempco) -- the resistor's accuracy directly affects measurement accuracy
- Add a TVS diode across the sense resistor to clamp voltage if the loop faults to a higher current
- Add an RC filter (1 kohm + 100 nF) before the ADC input if the loop picks up noise from long cable runs

**For isolated measurement:** Insert an analog isolation amplifier (AMC1200, ISO124) between the sense resistor and the ADC, with an isolated DC-DC module powering the loop side. This breaks the ground loop between the MCU and the loop supply.

**Watch for:** A broken wire in a 4-20 mA loop reads 0 mA. The 4 mA "zero" is specifically chosen so that a broken wire (0 mA) is distinguishable from a minimum reading (4 mA). Firmware that treats 0 mA as a valid reading instead of a fault will miss broken sensors.

## MCU ↔ Relay Outputs / Contact Closures

**The problem:** Legacy industrial equipment uses relay contacts to indicate status (dry contacts) or drives relay coils at 12–24V. The MCU needs to read contact closures as digital inputs and drive relay coils as digital outputs.

**Reading a contact closure:**

```
MCU 3.3V ─── 10kΩ pull-up ──┬── MCU GPIO (input, no internal pull-up)
                              │
                         Contact ── GND (legacy side)
```

- The pull-up holds the GPIO high when the contact is open. The contact pulls it low when closed
- If the contact is on a separate ground from the MCU, use an optocoupler instead of a direct connection -- the legacy side drives the opto LED, the MCU reads the phototransistor
- Add hardware debouncing (100 nF cap from GPIO to ground) or debounce in firmware (10-50 ms timer after first edge). Mechanical contacts bounce for 1-20 ms

**Driving a relay coil:**

```
MCU GPIO ── 1kΩ ── NPN base ──┐      Relay coil
                               │         │
                            NPN (2N2222)  │
                               │         │
                              GND    +12V/+24V
                                         │
                              Flyback diode (1N4148)
                              across coil (cathode to +V)
```

- The NPN transistor (or N-channel MOSFET) switches the relay coil. The MCU GPIO drives the base/gate through a series resistor
- The flyback diode is non-negotiable. Without it, the inductive kick when the relay opens will exceed the transistor's voltage rating and potentially couple back into the MCU
- For MOSFET drive: a logic-level MOSFET (IRLML2502 or similar) does not need the base resistor. Gate direct from GPIO (or through a small series resistor for EMI), source to ground, drain to relay coil

## MCU ↔ Unbalanced Audio Input/Output

**The problem:** Legacy audio equipment uses unbalanced (single-ended) connections at line level (~1V RMS, ±1.4V peak) referenced to chassis ground. The MCU's ADC is DC-coupled, unipolar (0 to VDD), and sensitive to overvoltage.

**Audio input to MCU ADC:**

```
Audio source ── AC coupling cap (1µF) ──┬── voltage divider ── MCU ADC
                                         │
                                    bias resistors
                                    (set DC midpoint
                                     at VDD/2)
```

- The coupling capacitor blocks DC offset from the audio source
- A resistive divider from VDD to GND sets the DC bias point at VDD/2 (e.g., two 10 kohm resistors from 3.3V to GND with the midpoint connected to the signal through a 10 kohm resistor)
- The audio signal swings symmetrically around VDD/2. At ±1.4V peak into a 2:1 divider (if needed), the signal stays within 0–3.3V
- Add a Schottky clamp to VDD and GND if overvoltage is possible (guitar amp output, legacy mixer with high output levels)

**MCU DAC/PWM to audio output:**

```
MCU DAC ── series resistor (1kΩ) ── AC coupling cap (10µF) ── output jack
                                                                    │
                                                                   GND
```

- The series resistor sets the output impedance. Legacy equipment expects either 600 ohm or high-impedance (>10 kohm) output
- The coupling capacitor blocks DC. For audio-frequency response down to 20 Hz into a 10 kohm load, the cap needs to be at least 1/(2pi x 20 x 10000) = 0.8 uF -- so 1-10 uF is typical
- If using PWM, add an LC low-pass filter after the series resistor to remove the PWM carrier. Cutoff frequency above 20 kHz, well below the PWM frequency

**Watch for:** Audio grounds between equipment. Connecting the MCU ground to the audio equipment ground may create a ground loop with audible hum. If so, insert an audio isolation transformer (Triad Magnetics TY-250P or similar 600:600 ohm) in the signal path.

## In Practice

- **An MCU that resets or locks up when a relay on the same board switches** likely has insufficient flyback protection or poor ground routing -- check the flyback diode polarity and placement, and monitor the supply rail on a scope during relay switching
- **A 4-20 mA reading that shows 0 mA** indicates a broken wire or dead transmitter, not a zero measurement -- firmware should flag any reading below 4 mA as a fault condition
- **Audible hum (50/60 Hz) appearing when the MCU ground connects to legacy audio equipment** indicates a ground loop -- an isolation transformer in the audio path or a USB isolator on the digital side breaks the loop
