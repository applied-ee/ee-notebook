---
title: "In-Circuit vs. Out-of-Circuit Testing"
weight: 50
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# In-Circuit vs. Out-of-Circuit Testing

Measuring a component while it is still soldered into the circuit is faster than removing it, but the results are often misleading because parallel paths through other components affect the reading.

## When In-Circuit Measurements Work

- **ESR measurement of electrolytic capacitors:** ESR meters inject a small AC signal at 100 kHz, where the capacitor's impedance is dominated by ESR rather than capacitance. At this frequency, most parallel paths on the board have high enough impedance that the ESR reading is reliable. This is the single most useful in-circuit measurement for legacy troubleshooting
- **Diode junction test on semiconductors:** A DMM's diode test mode forward-biases one junction at a time. If the reading matches a normal silicon junction (~0.6 V) or germanium junction (~0.3 V), the junction is likely intact. Reversed readings or very low readings indicate a shorted device
- **DC voltage measurements:** Measuring the voltage at a node is inherently an in-circuit measurement and is always valid (assuming the meter's input impedance is high enough not to load the circuit — 10 Mohm is sufficient for nearly all legacy circuits)

## When In-Circuit Measurements Mislead

- **Resistance measurements:** A resistor that reads 470 ohm out of circuit might read 320 ohm in circuit because a parallel path through another resistor, a semiconductor junction, or a feedback network lowers the apparent resistance. In-circuit resistance readings are only useful as a rough check — if the reading is wildly wrong, something is shorted, but a "reasonable" reading is not confirmation that the component is good
- **Capacitance measurements:** A capacitance reading in circuit includes the capacitance of every component in parallel with the one being measured. In-circuit capacitance readings are unreliable for anything other than large filter capacitors (where the cap under test dominates the total capacitance at the node)

## Lifting a Lead

When an in-circuit reading is ambiguous, desoldering one lead of the suspect component isolates it from the rest of the circuit. This is less work than fully removing the component and allows a valid measurement while the component is still partially in place.

- Lift the lead that disconnects the component from the most parallel paths
- Measure the component
- Resolder the lead, adding flux — the original flux is long exhausted on a vintage joint

## Tips

- **Always compare in-circuit readings to the expected value** from the schematic or to the same node on a working unit — an in-circuit reading in isolation is meaningless because the parallel paths are unknown
- **When an in-circuit resistance reading is lower than expected, check for a semiconductor junction in parallel** — reverse the meter leads and see if the reading changes, which indicates a diode or transistor junction in the measurement path

## Caveats

- **In-circuit ESR readings are reliable, in-circuit capacitance readings are not** — the high test frequency of an ESR meter makes parallel paths negligible, but a capacitance measurement at lower frequency picks up every parallel capacitor on the node
- **A component that tests good out of circuit may still fail in circuit** — temperature, voltage, and loading conditions on the board differ from bench test conditions. A transistor that passes all DMM tests may still fail under the specific bias and signal conditions of its circuit

## In Practice

- **An electrolytic capacitor that reads 2-3x its expected ESR value in circuit** is failing even if its capacitance still reads near nominal — ESR rises before capacitance drops, making ESR the early warning indicator for electrolytic aging
- **A resistor that reads correct in circuit but the stage still misbehaves** may have a hairline crack that opens under thermal stress — the resistance is correct at room temperature on the bench but drifts or opens when the board reaches operating temperature
