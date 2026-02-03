---
title: "Is Voltage Correct Under Load?"
weight: 20
---

# Is Voltage Correct Under Load?

A rail can measure perfectly at no load and sag badly when the circuit draws current. Static regulation, load-dependent droop, and dropout are caught by measuring under real operating conditions.

## Static Regulation Check

Measure rail voltage with circuit idle (minimal load), then again with circuit running its heaviest load (transmitting, motors running, LEDs on). The difference is load regulation error.

- **Small drop (< 1–2% of nominal):** Regulation is healthy
- **Noticeable drop:** Regulator may be near dropout, trace resistance may be high, or bulk capacitance insufficient
- **Rail collapses:** Supply can't handle the load, or a fault is drawing excess current

## Load Transient Response

Use an oscilloscope to see dynamic response — how fast the rail recovers after a load step. This reveals transient droop that a DMM averages out, and is essential for pulsed loads (RF transmitter, motor PWM, LED strobe).

DC-couple the channel, set vertical scale to see expected droop (e.g., 100 mV/div), set timebase to capture the transient (10–100 µs/div for switcher transients, 1–10 ms/div for bulk cap effects). Trigger on the load event or use single-shot.

Observe initial droop depth, recovery time, and any ringing or overshoot.

## Tips

- Measure at the load, not at the regulator output — trace and connector resistance drops voltage between the two points
- Use a short ground lead or spring-tip ground to avoid ringing artifacts from the probe
- For pulsed loads, trigger the scope on the load event to capture the transient reliably

## Caveats

- "Idle" isn't always low-load — some MCUs draw significant current even when firmware looks idle
- If the rail has a sense line (remote sensing), measure what the regulator sees vs what the load sees separately
- AC coupling can zoom in on transients but distorts slow events due to coupling time constant
- If the load step is very fast (nanoseconds), probe artifacts may dominate — compare with a slower load step

## Bench Relevance

- Rail that measures correct at idle but sags under load indicates regulation or capacity problem — check regulator headroom
- Large transient droop with slow recovery suggests insufficient output capacitance or high ESR
- Ringing after load step indicates LC resonance — check output capacitor ESL and PCB inductance
- Rail that collapses completely under load suggests supply is undersized for the load or a fault is drawing excess current
- Voltage measured at regulator output that's correct while voltage at load is low indicates trace or connector resistance drop
