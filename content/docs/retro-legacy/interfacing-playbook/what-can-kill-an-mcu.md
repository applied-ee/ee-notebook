---
title: "What Can Kill an MCU?"
weight: 20
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# What Can Kill an MCU?

Modern CMOS microcontrollers are fragile compared to the legacy systems they connect to. The following conditions destroy I/O pins or create latent damage.

## Overvoltage on Any Pin

CMOS inputs have ESD clamp diodes to VDD and VSS. If a signal exceeds VDD + 0.3V (or goes below VSS - 0.3V), the clamp diode forward-biases and injects current into the supply rail. If the current exceeds the clamp diode's rating (typically 1-10 mA), the diode fails or heats the die locally.

**How it happens at the interface:** An RS-232 signal at +12V drives directly into a 3.3V MCU pin. A 24V industrial signal connected through a voltage divider where the divider was omitted and the signal connected directly. A legacy system with a pull-up to 5V connected to a 3.3V MCU that is not 5V-tolerant.

## Negative Voltage on Any Pin

Worse than overvoltage. A negative voltage forward-biases the substrate diode from the pin to VSS, injecting current into the substrate. This can trigger latch-up -- a parasitic SCR that creates a low-impedance path from VDD to VSS, drawing excessive current that destroys the chip unless power is removed in milliseconds.

**How it happens at the interface:** RS-232 negative swings (-3V to -15V). Inductive kick from a relay coil or motor. A legacy signal that swings below ground as part of its normal operation (bipolar signals, AC-coupled signals). Ground offset between systems -- if the legacy system's ground is 2V below the MCU's ground, a 0V signal from the legacy system appears as -2V to the MCU.

## Ground Offset

If the two systems do not share a common ground, or if the shared ground has significant impedance, the "ground" potential of the legacy system may differ from the MCU's ground by hundreds of millivolts to volts. Every signal from the legacy system appears shifted by this offset when measured by the MCU.

**What it damages:** If the offset is large enough that legacy signals go below the MCU's VSS, negative voltage appears on the input pins (see above). Even if it does not cause direct damage, the offset shifts all signal levels and can push nominally-valid signals outside the MCU's input thresholds.

## Inductive Kick

Any inductive load (relay coil, solenoid, motor) produces a voltage spike when current is interrupted. If the MCU is driving the load (through a transistor or MOSFET) and the flyback protection fails or is absent, the spike appears at the drain/collector and can couple into the MCU through parasitic capacitance, through the ground plane, or directly if the layout is poor.

**How it happens at the interface:** MCU drives a relay through an N-channel MOSFET. The flyback diode is absent, wrong polarity, or placed too far from the coil. The relay's inductive kick generates a 200V spike that exceeds the MOSFET's drain rating, avalanches the MOSFET, and couples into the MCU.

## Backfeeding Through I/O Pins

When the MCU is unpowered but an external signal is applied to an I/O pin, current flows through the pin's ESD clamp diode into the VDD rail. This partially powers the MCU through an unintended path — some internal circuits power up, others do not, and the resulting state can cause latch-up or permanent damage.

**How it happens at the interface:** The legacy system is powered on before the MCU. A cable is connected while the MCU is off. The MCU's supply is disabled by a regulator enable signal, but signals from the legacy system are still present on the I/O pins.

**Prevention:** Add series resistors (1-10 kohm) on I/O pins to limit backfeed current below the clamp diode's rating. Alternatively, use interface circuits with their own supply enable that tri-state outputs when the MCU is unpowered.

## Tips

- **Add a series resistor (1-10 kohm) on every MCU pin that connects to a legacy system** as a default practice — this single component limits clamp diode current during overvoltage, limits backfeed current when the MCU is unpowered, and costs essentially nothing in performance for signals under 100 kHz
- **Check the MCU datasheet for absolute maximum ratings on each pin** — some pins (like ADC inputs or USB D+/D-) have lower tolerances than general-purpose I/O, and a "5V-tolerant" label on a GPIO does not necessarily extend to every pin on the chip

## Caveats

- **"5V-tolerant" pins are only tolerant when the MCU is powered** — when VDD is 0V, the clamp diode to VDD forward-biases at 0.3V, and any signal above 0.3V backfeeds into the unpowered supply rail. The 5V tolerance rating assumes VDD is at its nominal value
- **Latch-up from negative voltage or overvoltage may not destroy the MCU immediately** — it can cause high current draw, erratic behavior, or partial function loss that looks like a firmware bug. If an MCU starts behaving strangely after a wiring error, cycle power immediately and inspect for damage before assuming a software issue

## In Practice

- **An MCU pin that reads the correct voltage with a DMM but does not respond to signal changes** likely has a damaged input buffer from a previous overvoltage event — the pin may still conduct through the clamp diodes but the CMOS input gate is destroyed
- **An MCU that draws significantly more supply current than its datasheet idle figure, even with all peripherals disabled,** has likely suffered latch-up damage — compare the measured supply current to the datasheet typical value as a quick health check after any wiring incident
