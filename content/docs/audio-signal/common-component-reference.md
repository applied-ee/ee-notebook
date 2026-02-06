---
title: "Common Component Reference"
weight: 80
---

# Common Component Reference

A quick reference to building blocks commonly encountered in audio and signal processing circuits, emphasizing signal quality, frequency response, and dynamic range. Key parameters include THD, SNR, latency, and headroom.

## Primitives

**Resistor** *(Primitive)*
Sets gain, bias, and filter characteristics. Metal film types offer low noise and tight tolerance for precision paths. Resistor noise (Johnson noise) matters in high-gain, low-level signal chains.

**Capacitor** *(Primitive)*
AC coupling, filtering, and timing. Film capacitors for signal path (low distortion, no microphonics), ceramic C0G for precision filters, electrolytic for bulk decoupling. Dielectric absorption in some types can affect signal integrity.

**Inductor** *(Primitive)*
LC filters and chokes in power supplies. DCR (DC resistance) and saturation current matter. Air-core inductors avoid saturation and nonlinearity for signal applications.

**Op-amp** *(Primitive)*
The core of most audio signal chains. Key parameters for audio are low noise, low distortion, adequate slew rate, and sufficient output current. GBW must exceed the highest signal frequency by a comfortable margin to maintain low distortion.

**Potentiometer/trimmer** *(Primitive)*
Volume controls, balance adjustments, and calibration. Logarithmic taper for volume controls (matches perceived loudness); linear for most other applications. Audio-grade pots have low noise and smooth tapers.

**Transformer** *(Primitive)*
Isolation, impedance matching, and balanced-to-unbalanced conversion. Audio transformers are specified by frequency response, distortion at low frequencies, and shielding. High-quality audio transformers are expensive but provide true galvanic isolation.

## Blocks

**RC low-pass filter** *(Block)*
First-order filter for anti-aliasing and high-frequency rolloff. 20 dB/decade slope is often insufficient for anti-alias duty — typically cascaded or followed by an active filter.

**RC high-pass filter** *(Block)*
AC coupling and DC blocking. Cutoff frequency sets the low-frequency rolloff — lower cutoff means better bass response but slower settling after transients.

**Active filter (Sallen-Key / MFB)** *(Block)*
Op-amp-based second-order filter for shaping frequency response. Key parameters are cutoff, Q, filter type (Butterworth for flat passband, Bessel for linear phase). MFB provides inverting gain and better high-frequency rejection.

**Inverting amplifier** *(Block)*
Op-amp configuration with gain = -Rf/Rin. Virtual ground at the inverting input provides constant input impedance regardless of gain. Input impedance equals Rin.

**Non-inverting amplifier** *(Block)*
Op-amp configuration with gain = 1 + Rf/Rg. Very high input impedance — the default buffer for high-impedance sources like guitar pickups or piezo transducers.

**Differential amplifier / instrumentation amplifier** *(Block)*
Rejects common-mode interference (hum, ground loops). Key parameters are CMRR and noise. Instrumentation amps with high CMRR are essential for low-level sensors and balanced inputs.

**Voltage clamp** *(Block)*
Limits signal excursion to protect inputs from overload. Key parameters are clamp voltage and clamping impedance. Soft clipping (gradual limiting) sounds better than hard clipping in audio applications.

## Subsystems

**ADC front end** *(Subsystem)*
Conditioning between analog source and A/D converter: anti-alias filter, buffer, level adjustment. Key parameters are bandwidth, noise floor, THD, and headroom. The anti-alias filter must be designed for the actual sample rate — aliased noise cannot be removed after digitization.

**DAC output stage** *(Subsystem)*
Conditioning after D/A converter: reconstruction filter, buffer, level adjustment. Key parameters are output impedance, settling time, and glitch suppression. The reconstruction filter removes images at multiples of the sample rate.

**Audio amplifier** *(Subsystem)*
Drives speakers or headphones from line-level signals. Key parameters are output power, THD+N, efficiency class (A, AB, D), and output impedance. Class D dominates for efficiency but requires output filtering and EMI management.

**PLL** *(Subsystem)*
Phase-locked loop for clock recovery, sample rate conversion, and jitter reduction. Key parameters are lock range, phase noise, and loop bandwidth. Jitter in audio clocks causes sampling uncertainty that appears as noise.

**Sensor interface** *(Subsystem)*
Conditions signals from microphones, pickups, or transducers. Each type has specific requirements — dynamic microphones need low-noise gain, phantom power for condensers, high input impedance for piezos.

## Devices

**Camera module** *(Device)*
Image sensor for audio-visual applications. Key parameters are resolution, frame rate, and interface type. Data throughput can overwhelm small processors — most designs use capable SoCs or capture single frames.

## Systems

**Recording/playback signal chain** *(System)*
Complete audio path: microphone through preamp, ADC, processing, DAC, amplifier, and speaker. Key parameters are SNR, THD+N, frequency response, and latency. The weakest link determines system performance — a high-quality ADC downstream of a noisy preamp captures the preamp's noise with great fidelity.

**Sensor-to-display chain** *(System)*
Complete path from transducer through conditioning, digitization, processing, and display. Error budget allocation across stages determines whether the system meets specification.
