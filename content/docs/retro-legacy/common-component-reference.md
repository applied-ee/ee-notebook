---
title: "Common Component Reference"
weight: 90
---

# Common Component Reference

A quick reference to building blocks commonly encountered in retro and legacy electronics, emphasizing discrete construction, era constraints, and aging effects. Key parameters include original specifications and substitution compatibility.

## Primitives

**Resistor** *(Primitive)*
Carbon composition resistors were common in vintage equipment — they drift with age and have higher noise than modern metal film. Power rating and tolerance matter when replacing. Modern metal film replacements are electrically superior but may not match the original appearance for restorations.

**Capacitor** *(Primitive)*
Electrolytic capacitors in vintage equipment commonly fail — ESR increases, capacitance drops, and eventually they leak. Paper and wax capacitors are almost always bad by now. Ceramic and mica types are usually reliable. Recapping is standard practice for any vintage equipment restoration.

**Inductor** *(Primitive)*
Chokes and filter inductors in power supplies, IF transformers in radios. Key parameters are inductance, DCR, and saturation current. Shorted turns from insulation breakdown are a common failure mode in old transformers.

**Transformer** *(Primitive)*
Power transformers, output transformers (audio), and IF/RF transformers. Key parameters are turns ratio, frequency response, and insulation condition. Old power transformers may have marginal insulation after decades of thermal cycling. Output transformers in tube amplifiers are critical to the sound character.

**Fuse** *(Primitive)*
Overcurrent protection — often undersized in vintage equipment by modern standards. Key parameters are current rating, voltage rating, and speed. Replacing a blown fuse with a higher value is a common beginner mistake that leads to more expensive failures.

**Diode** *(Primitive)*
Selenium rectifiers in old power supplies are prone to failure and can release toxic fumes. Silicon replacements require voltage consideration — silicon drops less voltage than selenium, so the output voltage rises. Germanium signal diodes are still needed in some detector circuits where 0.3 V forward drop matters.

**BJT** *(Primitive)*
Germanium transistors (2N404, 2N109) in vintage equipment have different characteristics than silicon. Key parameters are leakage current (higher in germanium), beta, and breakdown voltage. Many germanium types are obsolete but silicon equivalents can often substitute with bias adjustments.

**Potentiometer/trimmer** *(Primitive)*
Volume controls, tone controls, and calibration adjustments. Noisy or intermittent pots are a common problem in vintage equipment — contact cleaner sometimes restores function, but replacement is often needed. Original parts may have specific tapers that affect circuit behavior.

## Blocks

**Voltage divider** *(Block)*
Sets bias points and signal levels. In vintage equipment, resistor drift may have shifted division ratios from original values. Compare measured values to schematic when troubleshooting.

**Current source/sink** *(Block)*
Discrete current sources using transistors. See [Discrete-First Design Thinking]({{< relref "discrete-first-design-thinking" >}}) for biasing configurations common in vintage designs.

**Current mirror** *(Block)*
Matched transistor pair for current replication. In discrete designs, matching was achieved by selecting from a batch — replacement pairs may not match as well. Thermal coupling matters for accuracy.

**RC low-pass / high-pass filters** *(Block)*
First-order filters for signal conditioning and tone shaping. Capacitor value drift in old ceramics and paper types shifts cutoff frequencies.

**Common-emitter / common-source stage** *(Block)*
The workhorse discrete gain stage. See [Discrete-First Design Thinking]({{< relref "discrete-first-design-thinking" >}}) for biasing variants and schematic reading strategies.

**Emitter follower / source follower** *(Block)*
Buffer stage for impedance transformation. Common between stages in discrete designs to prevent loading. The VBE drop matters for bias point and signal swing calculations.

**Snubber** *(Block)*
Suppresses contact arcing in relay circuits and inductive load switching. Old snubber capacitors may have failed, leading to excessive contact wear or noise.

**RC oscillator** *(Block)*
Relaxation oscillator for timing and audio generation. Frequency stability is poor — RC tolerance and temperature cause drift. Adequate for non-critical timing but not precision applications.

**Monostable (one-shot)** *(Block)*
Produces a single timed pulse in response to a trigger. Classic implementation uses a 555 or discrete transistors. Key parameters are pulse width and retriggerable behavior. Common in pulse stretching and debounce circuits.

## Subsystems

**Linear regulator** *(Subsystem)*
Discrete regulators using pass transistors and Zener references preceded three-terminal ICs like the LM317. Key parameters are dropout, ripple rejection, and load regulation. Zener reference drift and pass transistor beta variation affect regulation over temperature.

**UART/serial** *(Subsystem)*
RS-232 was the standard serial interface. Key parameters are voltage levels (±12 V for RS-232), baud rate, and handshaking. Interfacing vintage RS-232 equipment to modern TTL serial requires level translation.

## Devices

**AC-DC power supply** *(Device)*
Linear power supplies dominate in vintage equipment — transformer, rectifier, filter capacitors, and regulator. Key failure modes are dried electrolytic capacitors, failed rectifiers (especially selenium), and open or shorted filter chokes. Recapping is often the first step in any vintage equipment restoration.

## Systems

**Closed-loop control system** *(System)*
Analog control loops in vintage equipment (motor speed control, temperature regulation, AGC). Key parameters are loop bandwidth and stability. Component drift over decades may require recalibration or cause oscillation in marginal designs.
