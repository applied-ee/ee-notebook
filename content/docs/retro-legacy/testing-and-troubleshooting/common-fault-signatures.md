---
title: "Common Fault Signatures by Technology"
weight: 60
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Common Fault Signatures by Technology

Different circuit technologies have characteristic failure modes that narrow the search before any measurement is taken.

## Discrete Transistor Boards

Found in equipment from the 1960s through the 1980s, and still in power stages of later designs.

- **Bias drift:** Carbon composition resistors in the bias network have drifted high, shifting the transistor's operating point. Symptoms: reduced gain, increased distortion, or a stage that is cut off or saturated when it should be in its linear region. Measure the bias voltages and compare to expected values (calculated from the schematic if available, or estimated from the topology)
- **Thermal runaway:** A transistor with insufficient thermal compensation draws increasing collector current as it heats, which heats it further. Symptoms: the transistor runs hot, the bias point shifts over minutes after power-up, and eventually the transistor fails short (collector-emitter short)
- **Open or shorted junctions:** A failed transistor may show a short from collector to emitter (device destroyed by overcurrent) or an open junction (wire bond failure, especially in older packaging). A DMM diode test quickly identifies these

## TTL and CMOS Logic Boards

Digital boards from the 1970s through the 1990s.

- **Stuck outputs:** A logic IC with an output stuck high or stuck low regardless of input changes. Causes: internal latch-up (especially in CMOS), output driver failure, or a short on the output trace to the power rail or ground. Isolate by checking whether the output changes when the IC is removed from its socket (if socketed) or by lifting the output pin
- **Bus contention:** Two or more outputs driving the same bus line to different states simultaneously. Symptoms: supply current spikes, output voltage at an intermediate level (not valid high or valid low), and the IC runs hot. Common cause: a bus driver's enable line is stuck active when it should be tri-stated
- **Supply issues:** TTL logic is sensitive to supply voltage — below 4.75 V, logic levels become marginal. Decoupling capacitor failure (dried electrolytics, cracked ceramics) causes local supply voltage sags on clock edges, producing intermittent logic errors that are difficult to catch without a scope

## Linear IC Boards

Op-amps, voltage regulators, and analog signal processing ICs.

- **Latch-up:** An input driven beyond the supply rails triggers a parasitic thyristor in the IC, creating a low-resistance path from VCC to ground. The IC draws excessive supply current and stops functioning. Power cycling may reset it, but the condition can recur whenever the input exceeds the supply. Common when replacing components if the power-up sequence allows an input signal to arrive before the supply stabilizes
- **Oscillation:** An amplifier or regulator oscillates at high frequency (kHz to MHz) due to inadequate phase margin. Symptoms: the output has a high-frequency ripple that was not present in the original design. Common cause: replacement of electrolytic capacitors with low-ESR types changes the loop compensation. A scope reveals the oscillation; a DMM averages it out and shows a normal reading
- **Thermal shutdown:** Power ICs with thermal protection (most modern voltage regulators) cycle between normal operation and shutdown when their junction temperature exceeds the limit. Symptoms: the output voltage drops out periodically, recovers, and drops out again. The cycling rate depends on the thermal time constant. Check for inadequate heatsinking, poor thermal interface compound, or an overloaded output

## Mixed-Signal Systems

Boards combining analog and digital sections (common from the 1980s onward).

- Digital noise contaminating analog sections through shared power rails or ground paths — visible as high-frequency hash on analog signals, especially at the digital clock frequency and its harmonics
- Clock failure or drift causing the digital section to misbehave while the analog section appears normal (or vice versa). Check the crystal oscillator or clock source first if the digital section is entirely non-functional

## Tips

- On socketed ICs, remove and reseat the IC before condemning it — oxidized socket contacts are a common cause of intermittent logic failures, and reseating wipes the contact surfaces

## Caveats

- **A "failed" IC identified by symptom matching may actually be a victim of an upstream fault** — a regulator that oscillates because of a bad output capacitor will cause logic errors throughout the board, and replacing the logic ICs does not fix the root cause
- **Replacing carbon composition resistors with modern metal film types changes the circuit's noise characteristics** — carbon comp resistors generate more thermal noise, and the lower noise of metal film replacements may reveal oscillation or interference problems that were previously masked

## In Practice

- **A TTL board where multiple ICs run hot and the supply current is well above normal** likely has bus contention — two or more outputs fighting over a shared bus line. Check enable and direction control signals on bus buffers and transceivers
- **A regulator output that is correct at idle but sags under load, with the regulator itself not overheating,** points to dried-out input filter capacitors that cannot supply the transient current the regulator demands — ESR measurement on the input capacitors confirms this
