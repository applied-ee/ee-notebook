---
title: "Common Component Reference"
weight: 110
---

# Common Component Reference

A quick reference to building blocks commonly encountered in RF circuit design, emphasizing frequency-dependent behavior, matching, and parasitics. Key parameters include S-parameters, noise figure, IP3, and VSWR.

## Primitives

**Capacitor** *(Primitive)*
At RF, a capacitor is a resonant circuit — above its self-resonant frequency (SRF), it becomes inductive. Key parameters are SRF, ESR at frequency, and Q factor. See [RF Capacitors & Inductors]({{< relref "rf-components" >}}) for detailed SRF behavior and component selection.

**Inductor** *(Primitive)*
Energy storage, impedance matching, and filtering. Key parameters are SRF, Q factor, and saturation current. Above SRF, the inductor becomes capacitive. See [RF Capacitors & Inductors]({{< relref "rf-components" >}}) for frequency-dependent behavior.

**Ferrite bead** *(Primitive)*
Lossy element that dissipates high-frequency energy as heat. Characterized by impedance at 100 MHz. Used for EMI filtering, not where true inductance is needed — the impedance curve is resistive, not reactive, at RF.

**Crystal/resonator** *(Primitive)*
Precision frequency reference. Key parameters are frequency, ESR, and temperature stability. Crystal oscillator phase noise affects receiver performance.

**Transformer** *(Primitive)*
Impedance matching and balun function. Key parameters are frequency range, insertion loss, and balance. RF transformers are specified very differently from audio or power transformers.

**Connector** *(Primitive)*
SMA, N-type, BNC — each has a frequency range and VSWR specification. Connector choice matters at GHz frequencies. Poorly terminated connectors cause reflections that distort measurements.

**PCB** *(Primitive)*
At RF, the PCB is a circuit element. Key parameters are dielectric constant (Er), loss tangent, and controlled impedance. Trace geometry defines transmission line behavior — 50 Ω microstrip or stripline requires specific width-to-height ratios.

## Blocks

**LC filter** *(Block)*
Second-order filter with steep rolloff. Key parameters are cutoff frequency, Q factor, and insertion loss. Resonance can amplify rather than suppress signals near the cutoff if Q is too high.

**Decoupling/bypass network** *(Block)*
Multiple capacitors in parallel covering different frequency ranges. ESL matters — the bypass capacitor's leads form an inductor that limits high-frequency effectiveness. See [RF Capacitors & Inductors]({{< relref "rf-components" >}}) for bypass strategies.

**Common-emitter / common-source stage** *(Block)*
RF amplifier building block. Key parameters are gain, noise figure, and stability factor (K). Unconditionally stable amplifiers don't oscillate regardless of source/load impedance; conditionally stable amplifiers require careful matching.

**Snubber** *(Block)*
Suppresses voltage spikes in switching circuits. Key parameters are snubber capacitor value and power dissipation. Design is largely empirical — calculations give a starting point, final values come from observing ringing on a scope.

**Crystal oscillator circuit** *(Block)*
Crystal plus oscillator circuit for stable frequency generation. Key parameters are frequency accuracy, phase noise, and startup time. See [Oscillators & Phase Noise]({{< relref "rf-components/oscillators-and-phase-noise" >}}) for phase noise considerations.

**ESD/TVS clamp** *(Block)*
Protects antenna inputs and RF ports. Key parameters are capacitance (which affects RF performance) and clamping voltage. Low-capacitance parts are essential for high-frequency ports.

## Subsystems

**PLL** *(Subsystem)*
Phase-locked loop for frequency synthesis. Key parameters are lock range, lock time, phase noise, and spurious output levels. Loop bandwidth trades reference tracking against output phase noise.

**Receiver front end** *(Subsystem)*
Antenna to baseband: band-pass filter, LNA, mixer, IF filter. Key parameters are noise figure, sensitivity, selectivity, and dynamic range. The LNA's noise figure dominates — Friis' formula shows subsequent stages contribute decreasingly to total noise figure. See [RF Amplifiers & Gain Blocks]({{< relref "rf-components/rf-amplifiers-and-gain-blocks" >}}).

**Transmitter output stage** *(Subsystem)*
Baseband to antenna: upconversion, driver amp, power amp, output filter, matching network. Key parameters are output power, efficiency, linearity, and harmonic suppression. Impedance mismatch reflects power back into the PA, reducing efficiency and potentially damaging the output transistor.

## Devices

**Wireless module** *(Device)*
RF transceiver (often with integrated MCU) for Wi-Fi, Bluetooth, LoRa, or other protocols. Key parameters are protocol, frequency band, range, power consumption, and antenna type. Antenna selection and placement have outsized effect on real-world range.

**GPS/GNSS module** *(Device)*
Satellite receiver with serial output. Key parameters are constellation support, position accuracy, and time-to-first-fix. Needs clear sky view — the RF signal is extremely weak.

## Systems

**SDR system** *(System)*
Software-defined radio where signal processing happens in software. Key parameters are frequency range, instantaneous bandwidth, dynamic range, and processing latency. The SDR front end's noise figure and dynamic range set hard limits — no software processing recovers signals below the hardware noise floor.
