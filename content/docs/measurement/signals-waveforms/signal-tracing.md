---
title: "Where in the Chain Does It Go Wrong?"
weight: 30
---

# Where in the Chain Does It Go Wrong?

Stage-by-stage signal tracing. The input looks fine and the output doesn't — so you walk through the circuit node by node until you find where the signal breaks. This is the fundamental debugging technique for any multi-stage signal path: audio chains, filter cascades, data pipelines, power conversion stages.

## Oscilloscope: Stage-by-Stage Tracing

**Tool:** Oscilloscope (2+ channels preferred), 10x probes
**When:** Output is wrong and you need to localize which stage is at fault

### The Method

Signal tracing is divide-and-conquer applied to a signal path:

1. Identify the signal chain from input to output (schematic is essential here)
2. Probe the input — confirm it's correct
3. Probe the output — confirm it's wrong
4. Probe the midpoint of the chain — is it correct or wrong?
5. The fault is between the last correct node and the first wrong node
6. Continue halving until you've isolated the faulty stage

### Two-Channel Technique

With two scope channels, you can watch input and output of each stage simultaneously:

1. CH1 on the input of a stage, CH2 on the output
2. Trigger on CH1 (the input)
3. Compare: Is the output the expected transformation of the input?
   - Amplifier: output should be a scaled version of the input
   - Filter: output should be the input with certain frequencies removed
   - Buffer: output should equal input (within loading limits)
   - Comparator: output should switch state when input crosses threshold
4. Move both probes to the next stage and repeat

### What You Learn

- Which specific stage is not performing its function
- Whether the fault is total (no output) or partial (wrong amplitude, distorted shape, unexpected delay)
- Whether the fault is in the stage itself or in the connection between stages

### Gotchas

- Moving probes between stages changes loading. A stage that works with the probe on it may oscillate when you remove the probe (or vice versa). If you suspect probe loading is affecting the circuit, note whether behavior changes when you connect or disconnect the probe
- AC-coupled stages need time to settle when you first probe them. Wait for the DC blocking capacitor to charge before judging the waveform
- Feedback paths mean that a fault in a later stage can manifest as wrong behavior in an earlier stage. If something doesn't make sense, consider whether downstream loading or feedback is pulling the earlier stage off its expected operating point
- Some stages (like oscillators or PLLs) don't have an "input" in the signal-chain sense. These need to be checked for correct operation independently

## Signal Injection: Working Backwards

**Tool:** Signal generator (function generator or audio oscillator) + oscilloscope
**When:** No input signal available, or you want to test one stage in isolation

### Procedure

1. Start at the output stage — inject a known signal at its input
2. If the output is correct, the output stage is good — move the injection point one stage earlier
3. Continue working backward until the output stops being correct
4. The faulty stage is the one that doesn't pass the injected signal correctly

### What You Learn

- Each stage's transfer function in isolation — gain, frequency response, distortion
- Independence from the actual input signal source, which might itself be the problem

### Gotchas

- Match the injected signal to what the stage expects. Don't inject 1Vpp into a stage that normally sees 10 mV — you'll overdrive it and see distortion that isn't a fault
- Impedance matching matters. A function generator with 50 Ohm output driving a high-impedance input is fine; driving a low-impedance input may need a different approach
- Injecting signal into feedback amplifiers can be tricky — the feedback path may fight your injected signal. Break the loop (carefully) or inject at a summing point
- DC bias: if the stage requires a specific DC bias point to operate, your injected AC signal needs to ride on the correct DC level. Use the generator's DC offset function, or AC-couple into the stage

## DMM: Stage-by-Stage DC Bias Check

**Tool:** DMM, V⎓ (DC Volts)
**When:** Quick check that each stage is biased correctly before worrying about the signal

### Procedure

1. With the circuit powered but no input signal (or with a known DC input), measure DC voltages at key nodes:
   - Supply pins of each active device (op-amp, transistor, IC)
   - Bias points (transistor base/gate, op-amp inputs)
   - Output DC levels
2. Compare to schematic expected values or datasheet typical operating conditions
3. A stage with wrong DC bias voltages won't process signals correctly — fix the bias first

### What You Learn

- Whether each stage is powered and biased in its active region
- A transistor with 0V on its collector is saturated or has no power — it can't amplify
- An op-amp with both inputs at the same voltage as the negative rail has lost its positive supply

### Gotchas

- DC bias is necessary but not sufficient. A stage can have correct DC bias and still not pass the signal (open coupling capacitor, bad feedback component, oscillation eating up headroom)
- Some circuits have signal-dependent DC bias (class-B output stages, PWM circuits). Measure with no signal first to get the quiescent point
- CMOS logic stages don't have traditional "bias points" — they're either high, low, or switching. Check for stuck pins (always high or always low when they should toggle)
