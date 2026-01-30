---
title: "Fault Isolation Strategies"
weight: 20
---

# Fault Isolation Strategies

Once triage tells you what kind of failure you're dealing with, isolation narrows down *where* the fault lives. The goal is to cut the search space in half with each step, not to wander through the circuit hoping to stumble on the problem.

## The Debugging Loop

Every effective debugging session follows this loop, whether you're aware of it or not:

1. **Hypothesize** — Based on what you know so far, what's the most likely fault?
2. **Predict** — If this hypothesis is correct, what should a specific measurement show?
3. **Measure** — Make that measurement
4. **Update** — Does the result confirm, refute, or modify your hypothesis?

Then repeat. The key discipline is step 2: *predict before you measure*. If you don't have a prediction, you don't have a hypothesis — you're just collecting data. A measurement that matches your prediction is confirming. A measurement that contradicts your prediction is *more* valuable — it eliminates a theory and forces you to think harder.

When you're stuck, it's almost always because you skipped step 2 and are taking measurements without clear expectations.

## Divide and Conquer

The most powerful general strategy. Split the signal path in half and determine which half contains the fault.

### How It Works

1. Identify the full signal chain from input to output (or source to load)
2. Pick a midpoint — a node you can probe or disconnect
3. Check: is the signal correct at the midpoint?
   - **Yes** → fault is in the second half. Probe between midpoint and output
   - **No** → fault is in the first half. Probe between input and midpoint
4. Repeat, halving the search space each time

This is O(log n) debugging — for a 16-stage signal chain, you need at most 4 measurements to find the faulty stage.

### Where It Applies

- Audio signal chain (source → preamp → tone → power amp → speaker)
- Digital data path (sensor → ADC → processor → DAC → output)
- Power distribution (supply → regulator → filter → load)
- Any linear or branching signal chain

## Known-Good Substitution

Swap a suspected component or module with one you know works. If the fault goes away, you found it.

**When it works well:**
- Socketed ICs, modules, cables, connectors
- Boards with identical channels (swap left/right, channel A/B)
- Bench equipment (try a different power supply, different probe)

**When it doesn't work:**
- Soldered components (destructive to test, use other methods first)
- When you don't actually have a known-good substitute
- When the fault might be in the interface between components, not the component itself

**The trap:** Make sure your "known-good" really is known-good. A spare from the same batch may have the same defect. A cable that "should work" might not.

## Disconnecting Subsystems

If a circuit has multiple loads or subsystems, disconnect them one at a time to see if the fault clears. This isolates which subsystem is dragging down or corrupting shared resources (power, buses, clocks).

### Approach

1. Identify shared resources: power rails, communication buses, clock lines
2. Disconnect one subsystem at a time (physically remove, cut a trace, pull a connector, or hold in reset)
3. Test after each disconnection
4. If the fault clears when subsystem X is disconnected, investigate X — it may be drawing too much current, loading a bus, or injecting noise

This is especially useful for current draw problems. See [Is current draw expected?]({{< relref "/docs/measurement/power-rails-supplies/current-draw" >}})

## Forcing Known States

When you can't observe a node passively, force it to a known state and see how the circuit responds.

| Technique | What it tells you |
|-----------|------------------|
| Tie an input HIGH or LOW | Does the downstream circuit respond correctly to a known logic level? Isolates the input source from the downstream logic |
| Inject a known signal (function generator) | Does the circuit process a clean, known input correctly? Eliminates the possibility that the input source is the problem |
| Force a voltage with a bench supply | Does the circuit work with clean, adjustable power? Isolates power supply issues from the rest of the circuit |
| Replace sensor with fixed voltage | Does the ADC/processor see the right value? Isolates the sensor from the signal chain |

Forcing a known state turns a "something is wrong somewhere" into a "this specific block does/doesn't work" — it converts a vague symptom into a localized fact.

## Working Without a Schematic

Sometimes you don't have documentation. Reverse-engineering enough to debug is a different skill from full reverse-engineering.

**Minimum you need to figure out:**
- Where does power come in, and what voltages are generated?
- What are the major ICs? (Read part numbers, look up datasheets)
- What connects to what? (Follow traces, check continuity)
- Where are the connectors, and what do they carry?

**Practical tips:**
- Start with the power section — find the input, regulators, and rails
- Large ICs with many pins are processors/FPGAs — find their power and clock pins first
- Connectors and test points are debugging gifts — they're access points the original designer left for you
- Take photos and make notes as you go — you'll forget which pin you already checked

## Working With a Schematic

Having a schematic doesn't mean trusting it blindly.

- **Verify the schematic matches the board** — revision mismatches, hand modifications, and errata are common
- **Read the schematic functionally**: trace the signal path from input to output, identify power distribution, find feedback loops
- **Mark up a printed copy** as you go — check marks on verified nodes, X on faults found, ? on things to investigate
- The schematic tells you what *should* happen. The board tells you what *does* happen. When they disagree, the board is always right.

## Cross-References

- Signal tracing procedures: [Signal Tracing]({{< relref "/docs/measurement/signals-waveforms/signal-tracing" >}})
- Current draw measurement: [Is Current Draw Expected?]({{< relref "/docs/measurement/power-rails-supplies/current-draw" >}})
