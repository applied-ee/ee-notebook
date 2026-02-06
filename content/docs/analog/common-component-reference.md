---
title: "Common Component Reference"
weight: 70
---

# Common Component Reference

A quick reference to building blocks commonly encountered in analog circuit design, emphasizing biasing, operating regions, transfer functions, and the parameters that matter most: gain, bandwidth, impedance, and noise.

## Primitives

**Resistor** *(Primitive)*
Sets bias currents, divides voltages, and defines gain through feedback networks. Power rating and temperature coefficient matter when the resistor carries significant current or sits in a precision path.

**Capacitor** *(Primitive)*
AC coupling, filtering, and energy storage. Dielectric type determines behavior under voltage and temperature — C0G for precision, X5R/X7R for bulk decoupling. ESR matters in power filtering; voltage derating matters in ceramic types.

**Inductor** *(Primitive)*
Energy storage and filtering, primarily in power and RF applications. Saturation current is the critical limit — once exceeded, inductance collapses.

**Diode** *(Primitive)*
Rectification, clamping, and voltage references (Zener). Forward drop varies by type (silicon ~0.6 V, Schottky ~0.3 V). Junction capacitance matters at high frequencies.

**BJT** *(Primitive)*
Current-controlled current amplifier. Key parameters are hFE (current gain), VCE(sat), and fT (transition frequency). Base current is not a free input — it sets the operating point and affects the source impedance.

**MOSFET** *(Primitive)*
Voltage-controlled switch or amplifier. Key parameters are Vth (threshold), RDS(on), and gate charge. The gate is capacitive — drive current affects switching speed but not static dissipation. Logic-level vs. standard threshold matters for direct MCU drive.

**Op-amp** *(Primitive)*
The universal analog building block. Key parameters are GBW (gain-bandwidth product), input offset voltage, input bias current, slew rate, and output drive. Treated as a primitive when selecting from a catalog; becomes a block when analyzing its internal compensation.

**Comparator** *(Primitive)*
High-speed differential amplifier designed for switching, not linear operation. Key parameters are propagation delay, input offset, and output type (push-pull, open-drain). Using an op-amp as a comparator fails at speed; using a comparator as an op-amp fails immediately.

**Voltage reference** *(Primitive)*
Precision voltage source for ADC references, bias networks, and calibration. Key parameters are initial accuracy, temperature coefficient, noise, and load regulation.

**Transformer** *(Primitive)*
Transfers energy and provides galvanic isolation. Key parameters are turns ratio, inductance, leakage inductance, and isolation voltage. Frequency-dependent behavior makes RF and audio transformers very different devices.

**Potentiometer/trimmer** *(Primitive)*
Variable resistor for adjustable bias, calibration, or user controls. Linear vs. logarithmic taper matters for audio applications.

## Blocks

**Voltage divider** *(Block)*
Two resistors producing a fraction of the input voltage. Key parameters are division ratio, output impedance, and power dissipation. Loading the output with an impedance comparable to the divider shifts the ratio.

**Current source/sink** *(Block)*
Maintains constant current regardless of load voltage. Key parameters are output current, compliance range, and output impedance. Built from a transistor and sense resistor, or an op-amp with a sense resistor.

**Current mirror** *(Block)*
Matched transistor pair where one sets the reference current and the other replicates it. Key parameters are mirror ratio, output impedance, and matching accuracy. Discrete mirrors rarely match as well as integrated ones.

**RC low-pass filter** *(Block)*
First-order filter with -3 dB cutoff at 1/(2πRC). 20 dB/decade rolloff is often insufficient for serious filtering but adequate for deglitching and basic conditioning.

**RC high-pass filter** *(Block)*
AC coupling configuration. Key parameters are cutoff frequency and settling time. Low cutoff means slow settling after transients.

**Active filter (Sallen-Key / MFB)** *(Block)*
Op-amp-based second-order filter avoiding inductors. Key parameters are cutoff, Q, gain, and filter type (Butterworth, Bessel, Chebyshev). Op-amp GBW must be well above the filter frequency.

**Decoupling/bypass network** *(Block)*
Capacitors near IC power pins for local charge storage and high-frequency noise suppression. ESR, ESL, and placement matter more than raw capacitance value.

**Inverting amplifier** *(Block)*
Op-amp configuration with gain = -Rf/Rin. Input impedance equals Rin, which can be inconveniently low for high gain.

**Non-inverting amplifier** *(Block)*
Op-amp configuration with gain = 1 + Rf/Rg. Very high input impedance makes it the default for buffering high-impedance sources.

**Differential amplifier / instrumentation amplifier** *(Block)*
Rejects common-mode signals and amplifies the difference. Key parameters are CMRR, gain accuracy, and noise. Instrumentation amps add input buffers for high input impedance.

**Common-emitter / common-source stage** *(Block)*
Single-transistor amplifier with moderate gain. Key parameters are voltage gain, input/output impedance, and bias stability. Biasing is the hard part — gain depends on the operating point, which drifts with temperature.

**Emitter follower / source follower** *(Block)*
Unity voltage gain, current gain, impedance transformation. Output sits one VBE or VGS below the input.

**Voltage clamp** *(Block)*
Limits signal excursion using diodes to supply rails or a Zener. Key parameters are clamp voltage and clamping current capacity.

## Subsystems

**Linear regulator** *(Subsystem)*
Drops input voltage to a lower, regulated output by dissipating excess as heat. Key parameters are dropout voltage, PSRR, line/load regulation, and thermal limits. Efficiency is limited to Vout/Vin.

**ADC front end** *(Subsystem)*
Signal conditioning chain between sensor and ADC: anti-alias filter, buffer, level shifting. Key parameters are bandwidth, noise floor, THD, and settling time.

**DAC output stage** *(Subsystem)*
Conditioning after DAC: reconstruction filter, buffer, possibly voltage-to-current conversion. Key parameters are output impedance, settling time, and glitch energy.

**Sensor interface** *(Subsystem)*
Signal chain from raw sensor to usable signal: excitation, amplification, linearization. Each sensor type has specific interface requirements — thermocouples need cold-junction compensation, bridges need stable excitation.

**PLL** *(Subsystem)*
Phase-locked loop for frequency synthesis, clock multiplication, and jitter reduction. Key parameters are lock range, lock time, phase noise, and loop bandwidth.

## Devices

**Sensor module/breakout board** *(Device)*
Sensor IC plus minimal support circuitry on a prototyping-friendly board. Convenient but layout and grounding may not match production requirements.

## Systems

**Sensor-to-display chain** *(System)*
Complete path from physical quantity through sensing, conditioning, digitization, processing, and display. The error budget exercise — allocating allowable error across each stage — is the system-level discipline.

**Closed-loop control system** *(System)*
Sensor measures process variable, controller computes correction, actuator acts on the process. Key parameters are loop bandwidth, stability margins, and disturbance response. Stability is the fundamental concern.
