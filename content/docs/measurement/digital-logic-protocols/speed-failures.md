---
title: "Why Does It Fail at Higher Speeds?"
weight: 50
---

# Why Does It Fail at Higher Speeds?

The circuit works at 1 MHz but fails at 10 MHz. The prototype works on a breadboard at low speed but falls apart when you push the clock rate. This is signal integrity — where digital signals stop behaving like ideal voltage levels and start behaving like analog waveforms subject to transmission line effects, reflections, and crosstalk.

## The Transition Point

**Concept:** When does signal integrity start to matter?

A rough rule of thumb: signal integrity becomes important when the signal's rise time is shorter than about 6× the one-way propagation delay of the trace.

**Propagation delay on FR4 PCB:** ~6.5 ns per meter (~170 ps per cm)

| Rise time | Critical trace length | Typical scenario |
|-----------|---------------------|------------------|
| 100 ns | > 15 m | Not a concern on any normal PCB |
| 10 ns | > 1.5 m | Might matter on long backplane buses |
| 1 ns | > 15 cm | Matters on most PCBs — modern CMOS |
| 200 ps | > 3 cm | Matters on almost every trace — high-speed digital |

If your trace is longer than the critical length, unterminated signals reflect and cause ringing, overshoot, and false triggering.

## Oscilloscope: Seeing the Problem

**Tool:** Oscilloscope, 10x probe with spring-tip ground, bandwidth ≥ 5× signal edge content
**When:** The bus works at low speed but fails at high speed

### What to Look For

| Symptom on scope | Likely cause | Fix |
|-----------------|-------------|-----|
| Ringing after edges (damped oscillation) | Unterminated transmission line — reflection | Add series or parallel termination |
| Overshoot > 10–20% of swing | Same as ringing, plus potentially damaging to receiver | Termination, slower driver (series resistor) |
| Rounded, slow edges (signal never fully reaches high/low) | Excessive capacitive loading, weak driver | Reduce load capacitance, use stronger driver, reduce speed |
| Stair-step or double-edge on transitions | Reflection from impedance discontinuity (via, connector, stub) | Improve impedance matching, shorten stubs |
| Glitches on adjacent signals coinciding with transitions | Crosstalk from aggressive edges | Increase trace spacing, add ground between traces, slow edges |
| Eye diagram closes at higher speed | Combined SI effects reducing timing/voltage margin | Multiple fixes depending on dominant effect |

### Procedure

1. Measure the signal at low speed (where it works) — capture the "good" waveform for reference
2. Increase speed to the failing rate
3. Compare waveforms: what changed? Ringing appeared? Edges don't reach full swing? Glitches on other signals?
4. Probe at the receiver end of the trace, not the driver — the receiver is where signal quality matters
5. Use spring-tip ground — at the speeds where SI matters, the ground clip artifact will obscure real problems

### Gotchas

- Make sure your scope bandwidth is sufficient. A 50 MHz scope looking at a 50 MHz clock with 1 ns edges can't show you the ringing and overshoot because it can't see the fast edge content. You need at least 5× the edge frequency content: for 1 ns edges, that's ~2.5 GHz of content, needing a 500 MHz–1 GHz scope
- Probe capacitance (~15 pF passive, ~1 pF active) adds to the trace loading. At high speeds, the probe itself can change the behavior. If the circuit works with the probe connected and fails without it (or vice versa), probe loading is a factor

## Reflections and Termination

### Why Reflections Happen

When a signal reaches the end of an unterminated trace, the energy doesn't just stop — it reflects back toward the source. The reflection coefficient depends on the impedance mismatch:

**ρ = (Z_load - Z_trace) / (Z_load + Z_trace)**

| Load condition | Reflection | Effect |
|---------------|-----------|--------|
| Z_load = Z_trace (matched) | 0 | No reflection — clean signal |
| Z_load = open (high-Z input) | +1 | Full positive reflection — voltage doubles at receiver, then rings |
| Z_load = short | -1 | Full negative reflection — inverts |
| Z_load = 2 × Z_trace | +0.33 | Partial reflection |

Most CMOS inputs are high impedance → nearly full reflection on unterminated traces.

### Termination Options

| Termination type | Where | How | Trade-off |
|-----------------|-------|-----|-----------|
| Series (source) | Resistor at driver output | R = Z_trace - Z_driver (typically 22–47 Ohm) | Low power, one receiver only, signal reaches full swing only after round-trip |
| Parallel (end) | Resistor at receiver end to ground or VCC | R = Z_trace (50 Ohm typical) | Clean signal at receiver, but burns DC power continuously |
| Thevenin (end) | Two resistors at receiver, to VCC and GND | R_eq = Z_trace | DC bias set by divider ratio, burns power |
| AC (end) | Series R-C at receiver end | R = Z_trace, C blocks DC | No DC power, but more complex |

### Gotchas

- Series termination is the simplest and most common for point-to-point connections. But if you have multiple receivers on the same trace (bus topology), only the last receiver sees a clean signal — intermediate receivers see the initial half-swing and later correction
- Termination resistor value depends on the trace impedance, which depends on the PCB stackup. A microstrip on standard FR4 is typically 50–70 Ohm; a stripline is typically 50 Ohm. Get the stackup specs from your PCB fab

## Crosstalk

### What It Is

Capacitive and inductive coupling between adjacent traces. When one trace switches (aggressor), it induces a voltage on a nearby trace (victim). The coupled voltage increases with:

- Faster edges (higher dV/dt)
- Longer parallel run
- Closer trace spacing
- Higher driver impedance on the victim

### Measuring Crosstalk

1. Probe the victim trace while the aggressor is switching
2. The victim should be idle (held at a static level) to see the coupled noise clearly
3. Measure the amplitude and timing of the coupled pulse

### Reducing Crosstalk

| Technique | Effect |
|-----------|--------|
| Increase trace spacing (3× trace width minimum) | Reduces coupling by distance |
| Route a ground trace between aggressor and victim | Shields the coupling path |
| Use ground planes | Provides consistent return path, reduces both capacitive and inductive coupling |
| Slow down the aggressor's edges (series resistor) | Reduces dV/dt, directly reducing coupled energy |
| Shorten parallel run | Less coupling length = less coupled energy |

### Gotchas

- Breadboards have terrible signal integrity — long leads, no ground plane, massive crosstalk between adjacent rows. If your breadboard prototype fails at speed, try a PCB before debugging further. The breadboard may be the entire problem
- Ribbon cables and flat flex have adjacent signals running in parallel for long distances — worst case for crosstalk. Use ground wires between signal wires, or use twisted-pair/shielded cables for high-speed signals
- Return current path matters. If the return current can't flow directly underneath the signal trace (broken ground plane, slot in the plane), it detours, enlarging the current loop and increasing both EMI and crosstalk
