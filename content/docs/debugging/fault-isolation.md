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

Sometimes you don't have documentation — or you have a partial schematic that covers the block you're *not* debugging. Reverse-engineering enough to debug is a different skill from full reverse-engineering. You don't need to understand the whole board. You need to understand enough to isolate the fault.

### Step 1: Identify Power Rails

Power is the skeleton of the board. Everything else hangs off it.

- Find the power input (barrel jack, USB, battery connector, header pins)
- Follow the input to voltage regulators — look for 3-terminal devices (SOT-223, TO-220, SOT-23-5) near the input, often with chunky capacitors on both sides
- Measure each regulator's output to identify the rails (common: 5 V, 3.3 V, 1.8 V, 1.2 V)
- Trace where each rail goes. On a 2-layer board, one side is often a ground pour — check with continuity
- Label the rails on a photo or sketch as you go. Everything downstream depends on knowing which rail feeds which section

### Step 2: Find Grounds

- Check for a ground plane (continuity from the input ground to exposed copper, mounting holes, or shield cans)
- Identify ground pins on connectors — often the outermost pin or the shell/shield
- On 2-layer boards, the bottom layer is frequently a ground pour. Confirm with continuity between multiple exposed points
- Mark your ground reference — you'll need a reliable ground connection for every measurement from here on

### Step 3: Identify Clocks and Crystals

Clocks tell you what the board does and whether the brain is running.

- Look for crystals and crystal oscillators — silver cans, small 4-pin packages, or 2-pin HC49 packages
- Read the frequency marking on the crystal (e.g., 8.000 MHz, 12.000, 25.000, 32.768 kHz for RTC)
- Trace which IC the crystal connects to — that's your processor, FPGA, or communications chip
- Probe the crystal/oscillator output with a scope. If the clock isn't running, nothing downstream will work. This is one of the highest-value early measurements on an unknown board

### Step 4: Find Reset Pin Behavior

A processor held in reset looks dead but isn't broken.

- Look up the main IC's datasheet (read the part number, search online). Find the reset pin
- Trace the reset pin back — it usually connects to an RC network, a reset supervisor IC, or a pushbutton
- Measure the reset pin: is it at the active level (held in reset) or the inactive level (running)?
- If the reset pin is stuck active, trace upstream to find out why — voltage supervisor seeing a low rail, external signal holding it down, or a missing pull-up

### Step 5: Signal Tracing With Continuity and Visual Routing

When you need to know "what connects to what" without a netlist.

- **Visual tracing** — Follow traces on the board surface. A magnifying lamp or USB microscope helps. Traces on the top layer are usually visible; bottom-layer traces may be partially hidden by solder mask
- **Continuity meter** — Set your DMM to continuity mode. Touch one probe to the pin you're tracing, then systematically check candidate destinations (nearby IC pins, connector pins, test points). The beep tells you the connection exists; silence rules it out
- **IC datasheets as cheat sheets** — Once you've identified a part number, the datasheet's pinout tells you what *should* be connected to each pin. Use continuity to confirm the actual connections match, and to figure out where each net goes on the board
- **Connector pinouts** — Connectors are the board's external interface. Identifying what each pin carries (power, ground, data, clock) often reveals the board's functional blocks. Standard connectors (USB, JTAG, SPI headers, UART) have known pinouts — check against the standard first

**Practical tips:**
- Take photos and annotate them as you go — you'll forget which pin you already checked
- Connectors and test points are debugging gifts — access points the original designer left for you
- You don't need a complete schematic to debug. You need to know: where is power, is the clock running, is reset released, and is the signal present at the boundary of the block you're investigating

## Working With a Schematic

Having a schematic doesn't mean trusting it blindly.

- **Verify the schematic matches the board** — revision mismatches, hand modifications, and errata are common
- **Read the schematic functionally**: trace the signal path from input to output, identify power distribution, find feedback loops
- **Mark up a printed copy** as you go — check marks on verified nodes, X on faults found, ? on things to investigate
- The schematic tells you what *should* happen. The board tells you what *does* happen. When they disagree, the board is always right.

## Cross-References

- Signal tracing procedures: [Signal Tracing]({{< relref "/docs/measurement/signals-waveforms/signal-tracing" >}})
- Current draw measurement: [Is Current Draw Expected?]({{< relref "/docs/measurement/power-rails-supplies/current-draw" >}})
