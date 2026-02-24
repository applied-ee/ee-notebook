---
title: "Bench Bring-Up Checklist"
weight: 60
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Bench Bring-Up Checklist

Follow this sequence when connecting a modern MCU to a legacy system for the first time.

## Before Connecting Anything

- [ ] Complete the [domain identification checklist]({{< relref "domain-identification-checklist" >}}) -- know the voltage range, current levels, impedance, and grounding model of both sides
- [ ] Measure the voltage between the two systems' grounds (with both powered, if applicable). If it exceeds 1V, plan for isolation
- [ ] Verify the interface circuit has protection on every pin that connects to the legacy system (series resistors at minimum, TVS/clamps if the fault analysis warrants it)
- [ ] Verify flyback diodes are installed on every inductive load the MCU drives
- [ ] Verify the power supply for the interface can handle the current required by isolation components, relay coils, LED drive, etc.

## First Connection — Unpowered

- [ ] Connect the cable with both systems powered off
- [ ] Visually verify the wiring -- especially that power and ground are on the correct pins. RS-232 pin numbering is a common source of errors (DB-9 pin 5 is ground, but DTE and DCE have different pin assignments for TX/RX)
- [ ] Measure continuity on each signal path from the legacy connector through the interface circuit to the MCU pin. Confirm no shorts between adjacent pins

## Power-Up Sequence

- [ ] Power up the MCU side first. Verify the interface circuit is alive and all protection voltages are correct (3.3V rail, any isolated supply rails)
- [ ] Monitor the MCU's power supply current. If it spikes when the legacy system is connected (even unpowered), current is backfeeding through the interface
- [ ] Power up the legacy system. Monitor the MCU's I/O pins on the scope during legacy power-up -- look for voltage excursions outside the MCU's supply rails
- [ ] Verify signal levels at the MCU's I/O pins match expectations (correct voltage range, correct logic polarity, no unexpected noise)

## Functional Test

- [ ] Start with read-only operations. When reading a sensor, verify the value makes sense. When reading a contact closure, toggle the contact and watch the GPIO
- [ ] Add write operations one at a time. Drive one relay, send one RS-232 character, output one DAC sample. Verify the legacy system responds correctly
- [ ] Monitor power supply current and temperature throughout. A gradually increasing current or a warm component indicates a problem developing
- [ ] Test fault scenarios: disconnect the cable while running (if hot-plug is a possibility in the field), toggle power on the legacy side, and verify the MCU recovers without damage or lockup

## Tips

- **Test the complete interface with realistic cable lengths** and with both systems powered from their intended sources, not just bench supplies -- field conditions differ significantly from bench conditions

## Caveats

- **"Works on the bench" does not mean it works in the field** -- Bench conditions are clean: short cables, common ground through the bench supply, no nearby motors or relays. In the field, cables are long, grounds differ, and EMI is real. Test with realistic cable lengths and with both systems powered from their intended sources

## In Practice

- **A supply current spike at the moment the legacy system's cable is plugged in (even with the legacy system powered off)** indicates parasitic capacitance in the cable or connector charging through the MCU's I/O protection — this is normal for short cables but a series resistor limits the inrush on longer ones
- **An interface that passes all functional tests on the bench but fails intermittently in the field** almost always has a ground or shielding issue — the bench environment provides a common ground through the bench supply and short, unshielded connections that do not exist in the field installation
