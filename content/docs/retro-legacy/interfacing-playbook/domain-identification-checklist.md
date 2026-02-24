---
title: "Domain Identification Checklist"
weight: 10
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Domain Identification Checklist

Before designing any interface circuit, characterize both sides of the connection. Skipping this step is how MCU pins get destroyed.

## Voltage and Current

- [ ] **What is the signal voltage range?** Measure or look up the output levels -- not just the nominal, but the worst case. RS-232 can swing +/-15V. A "24V" industrial output may be 28V with the relay off and 0V with the relay on. A "5V logic" signal from a 1970s system may actually sit at 4.2V due to TTL output characteristics
- [ ] **What is the supply voltage range?** Including startup, shutdown, and fault conditions. A "12V" automotive system is [6-80V in practice]({{< relref "/docs/measurement/safety-high-energy/automotive-power-domain" >}})
- [ ] **What current does the signal source deliver?** A 4-20 mA loop is a current source. An RS-232 driver can source/sink 10-20 mA. A relay contact closure delivers whatever the pull-up circuit provides. The interface must not load the source beyond its capability
- [ ] **What current does the load expect?** A relay coil draws 50-150 mA. A legacy input may expect milliamps of drive current (TTL inputs sink ~1.6 mA in the low state). A CMOS input draws essentially zero DC current

## Impedance

- [ ] **Source impedance?** A 4-20 mA transmitter is a high-impedance current source. An RS-232 driver is a low-impedance voltage source (typically 300-600 ohms output impedance). A thermocouple is a very low voltage source with ~10 ohm source impedance
- [ ] **Load impedance?** The input impedance of the receiving circuit determines how much it loads the source. A CMOS microcontroller input is >10 Mohm at DC. An ADC input may have a sample-and-hold capacitor that draws transient current. A legacy audio input may be 10 kohm or 600 ohm depending on era and design

## Grounding Model

- [ ] **Is the signal ground-referenced, floating, or differential?** A ground-referenced signal has one terminal at system ground and the signal on the other. A floating signal (like a thermocouple or a current loop) has no inherent ground reference. A differential signal (RS-485, balanced audio) is defined as the voltage between two wires, independent of ground
- [ ] **Where is ground?** Legacy equipment may reference to chassis (connected to mains earth), to a signal ground bus (which may or may not be connected to earth), or to nothing identifiable (floating). Measure the voltage between the signal ground of the legacy system and the ground of the modern system before connecting them. If there is more than a volt or two, isolation is required
- [ ] **Is the chassis grounded through the mains plug?** Equipment with a 3-prong plug has its chassis at earth potential. Equipment with a 2-prong plug may float. Battery-powered equipment floats. This determines whether connecting signal grounds creates a ground loop through the mains wiring

## Isolation Requirements

- [ ] **Safety isolation?** If the legacy system operates at mains voltage or connects to mains-referenced circuits, safety-rated isolation (reinforced insulation, creepage/clearance, certified components) is required. This is non-negotiable
- [ ] **Noise isolation?** If connecting the grounds of both systems creates unacceptable noise (hum, buzzing, data errors), galvanic isolation breaks the ground loop. This is the most common reason for isolation in audio and instrumentation interfaces
- [ ] **Fault protection?** If miswiring, a component failure, or a surge could put dangerous voltage on the interface, isolation protects the modern equipment from destruction and the operator from shock

## Fault Scenarios

- [ ] **What happens if the cable is connected backwards?** RS-232 TX and RX swapped is annoying but harmless (the drivers can handle it). Power and ground swapped on a sensor cable will destroy the sensor and possibly the MCU
- [ ] **What happens if the cable is connected while both systems are powered?** Hot-plugging creates transient currents through connector capacitance and can inject spikes through ESD and charge redistribution. If the connector does not have ground-first/signal-last pin sequencing, every hot-plug is an ESD event
- [ ] **What happens if one system is powered and the other is not?** If the MCU is off and the legacy system drives a signal into an MCU pin, the signal may backfeed through the MCU's ESD clamp diodes into the unpowered VCC rail, potentially powering part of the MCU through an unintended path (parasitic power-up). This causes latch-up and damage
- [ ] **What happens during a fault on the legacy side?** A shorted winding in a transformer, a stuck relay, a failed voltage regulator -- these can put unexpected voltages on the interface. The interface circuit must survive the worst-case fault voltage without passing damage through to the MCU

## Tips

- Complete the domain identification checklist before building any interface circuit -- characterizing both sides of the connection up front prevents the most common MCU-destroying mistakes

## Caveats

- **Legacy systems assume their loads are passive** -- A legacy 4-20 mA transmitter expects to drive a passive 250 ohm load, not an active circuit that might inject current back into the loop. A tube-era audio output expects a resistive load, not a capacitive one that reflects energy. Respect the legacy system's assumptions about what it is driving
- **Hot-plugging legacy connectors is never safe unless designed for it** -- DB-9, DB-25, DIN, screw terminals, banana plugs -- none of these guarantee ground-first contact. Every hot-plug event is a potential ESD event, ground bounce event, and signal transient. Power down before connecting
