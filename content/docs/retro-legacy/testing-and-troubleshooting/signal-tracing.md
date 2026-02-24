---
title: "Signal Tracing"
weight: 30
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Signal Tracing

Signal tracing is the core diagnostic technique for analog and mixed-signal legacy equipment. The idea is to follow a signal through the circuit from input to output (or inject a signal and trace forward) until the point where it stops, distorts, or changes character.

## Audio-Frequency Signal Tracing

For audio equipment (amplifiers, receivers, tape decks):

- Inject a 1 kHz sine wave at the input from a signal generator or function generator
- Use an oscilloscope to follow the signal through each gain stage, coupling network, and output stage
- A simple audio signal tracer (amplifier with a speaker) can substitute for a scope in the field — touch the probe to each stage and listen for the signal. When the signal disappears or distorts, the fault is at or between that stage and the previous one

## RF Signal Injection

For RF equipment (receivers, transmitters, two-way radios):

- Inject a modulated RF signal at the antenna input at the equipment's operating frequency
- Trace through the RF front end, mixer, IF stages, and detector using a scope or signal tracer
- Alternatively, inject at each stage in reverse order (detector first, then IF, then mixer, then RF) — the stage where injection first fails to produce output is the failing stage

## Half-Split Strategy

For long signal chains or complex boards, the half-split method minimizes the number of measurements:

- Probe the signal path at the midpoint of the circuit
- If the signal is correct at the midpoint, the fault is downstream — probe the midpoint of the remaining downstream section
- If the signal is incorrect at the midpoint, the fault is upstream — probe the midpoint of the upstream section
- Each measurement eliminates half the remaining circuit, reaching the failing stage in log2(N) steps

## Tips

- Use the half-split strategy to narrow the failing stage before deep-diving into component-level testing — this prevents wasting time measuring good components in a working section of the circuit

## Caveats

- **Loading the circuit with the probe can change the signal** — a 10x scope probe presents ~10 Mohm and ~15 pF, which is negligible for most low-frequency circuits but can detune or load RF stages significantly. If probing changes the symptom, the probe is part of the problem
- **An audio signal tracer will not reveal distortion above the audible range** — harmonic distortion, high-frequency oscillation, and ultrasonic ringing all require a scope to detect. A stage that sounds clean through a tracer may still be oscillating at hundreds of kilohertz

## In Practice

- **An amplifier stage that clips asymmetrically** (one half of the waveform clips before the other) has a shifted bias point — the DC voltage at the collector or drain has moved away from its midpoint, reducing swing in one direction
