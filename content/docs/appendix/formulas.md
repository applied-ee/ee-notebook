---
title: "Formula Reference"
weight: 10
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---
{{< katex >}}{{< /katex >}}

# Formula Reference

A catalog of the key equations used throughout this notebook, organized by domain. Each entry gives the formula, its variables, and the practical insight it encodes. The main notebook pages describe these relationships in behavioral terms — this page collects the math in one place for quick reference.

---

## Circuit Fundamentals

**Ohm's law**

$$V = IR$$

Voltage equals current times resistance. The most-used relationship in electronics.

**Kirchhoff's voltage law (KVL)**

$$\sum V_{loop} = 0$$

The sum of all voltages around any closed loop is zero.

**Kirchhoff's current law (KCL)**

$$\sum I_{in} = \sum I_{out}$$

The sum of all currents entering a node equals the sum leaving it.

**Power dissipation**

$$P = IV = I^2 R = \frac{V^2}{R}$$

Power dissipated in a resistive element, in three equivalent forms.

**Voltage divider**

$$V_{out} = V_{in} \times \frac{R_2}{R_1 + R_2}$$

Unloaded output voltage of a resistive divider.

**Voltage divider loading**

$$V_{out} = V_{source} \times \frac{Z_{in}}{Z_{out} + Z_{in}}$$

Actual signal voltage at a stage's input, accounting for the output impedance of the driving stage and the input impedance of the receiving stage. When $Z_{in} \gg Z_{out}$ (bridging), nearly all the voltage transfers. When $Z_{in} = Z_{out}$ (matching), half the voltage transfers but maximum power transfers.

**Thevenin equivalent output**

$$V_{out} = V_{th} \times \frac{R_{load}}{R_{th} + R_{load}}$$

Loaded output voltage of any linear source reduced to its Thevenin equivalent. Small $R_{th}/R_{load}$ ratio means stiff regulation.

**Norton equivalent**

$$I_N = \frac{V_{th}}{R_{th}}$$

Norton current source equivalent. Thevenin and Norton representations are interchangeable.

**Maximum power transfer**

$$P_{max} = \frac{V_{th}^2}{4 R_{th}}$$

Occurs when $R_{load} = R_{th}$. Important in RF (50 ohm matching) and audio, but NOT the goal in power delivery where efficiency ($R_{load} \gg R_{th}$) matters more.

**Output impedance measurement**

$$R_{out} = R_{load} \times \frac{V_{oc} - V_{loaded}}{V_{loaded}}$$

Bench method: measure open-circuit voltage, measure with a known load, calculate. Non-destructive way to characterize any source.

---

## Passive Components

**Inductor voltage-current relationship**

$$V = L \frac{dI}{dt}$$

Voltage is proportional to the rate of current change. Constant current means zero voltage; sudden current interruption means very large voltage.

**Transformer voltage scaling**

$$V_{secondary} = V_{primary} \times \frac{N_{secondary}}{N_{primary}}$$

Voltage scales with turns ratio. A 10:1 step-down converts 120 V to 12 V.

**Transformer current scaling**

$$I_{secondary} = I_{primary} \times \frac{N_{primary}}{N_{secondary}}$$

Current scales inversely. Step down voltage and available current goes up proportionally. Power in equals power out minus losses.

**Impedance reflection through a transformer**

$$Z_{reflected} = Z_{load} \times \left(\frac{N_{primary}}{N_{secondary}}\right)^2$$

Impedance transforms by the square of the turns ratio. A 4:1 ratio transforms 50 ohm to 800 ohm as seen from the primary.

**RC filter cutoff frequency**

$$f_c = \frac{1}{2\pi RC}$$

The -3 dB frequency where output drops to 70.7% of input.

---

## AC & Impedance

**Capacitive reactance**

$$X_C = \frac{1}{2\pi fC}$$

Impedance of a capacitor at frequency $f$. Halves every time frequency doubles; infinite at DC (open circuit), approaches zero at high frequency.

**Inductive reactance**

$$X_L = 2\pi fL$$

Impedance of an inductor at frequency $f$. Doubles every time frequency doubles; zero at DC (short circuit), increases without bound at high frequency.

**Complex impedance**

$$Z = R + jX$$

Impedance as a complex number. $R$ (real part) is resistance (energy dissipated); $X$ (imaginary part) is reactance (energy stored and returned).

**Impedance of a capacitor**

$$Z_C = \frac{-j}{2\pi fC}$$

Negative imaginary because current leads voltage by 90°.

**Impedance of an inductor**

$$Z_L = j2\pi fL$$

Positive imaginary because current lags voltage by 90°.

**Impedance magnitude**

$$|Z| = \sqrt{R^2 + X^2}$$

What an impedance analyzer or LCR meter displays. Combines resistive and reactive parts into a single scalar.

**Phase angle**

$$\theta = \arctan\!\left(\frac{X}{R}\right)$$

Angle between voltage and current. 0° is purely resistive; ±90° is purely reactive.

**Ohm's law (AC)**

$$V = IZ$$

Ohm's law generalized to AC. $V$, $I$, and $Z$ are complex phasors; magnitude gives amplitude, phase gives timing.

**Series impedance**

$$Z_{total} = Z_1 + Z_2 + Z_3 + \cdots$$

Impedances in series add directly, same rule as DC but with complex arithmetic.

**Parallel impedance**

$$\frac{1}{Z_{total}} = \frac{1}{Z_1} + \frac{1}{Z_2} + \cdots$$

Reciprocal rule, same as parallel resistors but with complex arithmetic.

---

## Resonance & Q Factor

**LC resonant frequency**

$$f_0 = \frac{1}{2\pi\sqrt{LC}}$$

The frequency where inductive and capacitive reactance exactly cancel. Energy shuttles between the magnetic field of $L$ and the electric field of $C$ with no net reactive impedance.

**Q factor (energy definition)**

$$Q = 2\pi \times \frac{\text{energy stored}}{\text{energy dissipated per cycle}}$$

The universal definition. High Q means energy circulates many times before being lost; low Q means heavy damping.

**Q of series RLC**

$$Q = \frac{1}{R}\sqrt{\frac{L}{C}} = \frac{X_L}{R} \text{ at } f_0$$

Resistance is in the circulation path, so lower $R$ means higher $Q$.

**Q of parallel RLC**

$$Q = R\sqrt{\frac{C}{L}} = \frac{R}{X_L} \text{ at } f_0$$

Resistance shunts energy out of the tank, so higher $R$ means higher $Q$.

**Bandwidth from Q**

$$BW = \frac{f_0}{Q}$$

The 3 dB bandwidth of a resonant circuit. A 1 MHz resonance with $Q = 100$ passes a 10 kHz window.

**Loaded Q**

$$\frac{1}{Q_L} = \frac{1}{Q_U} + \frac{1}{Q_{external}}$$

External loading always reduces Q. A high-Q inductor in a tank circuit can drop from $Q_U = 200$ to $Q_L = 20$ once coupled to a real load.

---

## Magnetics

**Copper loss (DC)**

$$P_{copper} = I^2 \times DCR$$

Basic resistive loss in a winding. At 100°C, DCR is roughly 30% higher than the 25°C datasheet spec.

**Copper loss (AC + DC)**

$$P_{copper} = I_{dc}^2 \times R_{dc} + I_{ac(rms)}^2 \times R_{ac}$$

Total copper loss includes both DC and AC components. At switching frequencies, $R_{ac}$ can be 5–50x higher than $R_{dc}$ due to skin effect and proximity effect.

**Core loss (Steinmetz equation)**

$$P_{core} = k \cdot f^a \cdot B_{peak}^b \cdot \text{Volume}$$

Where $k$, $a$, $b$ are material constants from the datasheet (typically $a = 1.1\text{–}1.8$, $b = 2.0\text{–}2.8$). Core loss scales aggressively with both frequency and flux density; doubling either more than doubles the loss.

**Leakage inductance**

$$L_{leakage} = L_{winding} \times (1 - k)$$

Where $k$ is the coupling coefficient (0.95–0.999 for real transformers). Even small leakage inductance causes serious voltage spikes during fast current transitions.

**Leakage spike voltage**

$$V_{spike} = L_{leak} \frac{dI}{dt}$$

When current through a transformer is interrupted, leakage inductance produces a voltage spike. 10 µH leakage with 2 A interrupted in 50 ns produces a 400 V spike.

**Parasitic ringing frequency**

$$f_{ring} = \frac{1}{2\pi\sqrt{L_{leak} \times C_{stray}}}$$

Leakage inductance and stray capacitance form an LC tank that rings at this frequency whenever excited by a fast transition. Typical range: 1–50 MHz.

---

## Semiconductors & Amplifiers

**MOSFET transconductance**

$$g_m = \frac{\Delta I_D}{\Delta V_{GS}}$$

How much drain current changes per volt of gate voltage change. In saturation: $g_m = 2 I_D / (V_{GS} - V_{th})$. Higher bias current increases $g_m$ but increases power dissipation.

**BJT voltage divider bias**

$$I_E = \frac{V_B - V_{BE}}{R_E}$$

The workhorse biasing topology. Two resistors set $V_B$, emitter resistor $R_E$ converts it to stable current independent of beta. Design rule: divider current should be at least 10x expected base current.

**Loaded amplifier gain**

$$A_{loaded} = A_v \times \frac{Z_{load}}{Z_{out} + Z_{load}}$$

Actual gain after accounting for loading by the next stage. If $Z_{load}$ is comparable to $Z_{out}$, significant gain is lost.

---

## Noise & Signal Quality

**Thermal noise voltage (Johnson-Nyquist)**

$$V_{noise} = \sqrt{4kTRB}$$

Where $k = 1.38 \times 10^{-23}$ J/K (Boltzmann's constant), $T$ = temperature in Kelvin, $R$ = resistance in ohms, $B$ = bandwidth in Hz. Every resistor generates this white noise from random electron motion. Proportional to $\sqrt{R}$ and $\sqrt{B}$; narrowing bandwidth helps significantly.

**Shot noise current**

$$I_{noise} = \sqrt{2qI_{DC}B}$$

Where $q = 1.6 \times 10^{-19}$ C (electron charge), $I_{DC}$ = DC current, $B$ = bandwidth. Caused by the discrete nature of charge carriers crossing a junction. Dominates in photodiode and low-current circuits.

**Signal-to-noise ratio**

$$SNR = 10 \log_{10}\!\left(\frac{P_{signal}}{P_{noise}}\right) \text{ dB}$$

Or equivalently for voltage across the same impedance: $SNR = 20 \log_{10}(V_{signal} / V_{noise})$ dB. Always measured over a defined bandwidth — wider bandwidth means more observed noise.

**Friis noise figure (cascaded stages)**

$$F_{total} = F_1 + \frac{F_2 - 1}{G_1} + \frac{F_3 - 1}{G_1 G_2} + \cdots$$

Where $F$ is noise factor (linear, not dB) and $G$ is power gain. Shows why the first stage dominates: if $G_1$ is large, all subsequent noise contributions are divided by it. A noisy stage after high gain is harmless; a noisy stage with no preceding gain is catastrophic.

**Total harmonic distortion**

$$THD = \frac{\sqrt{V_2^2 + V_3^2 + V_4^2 + \cdots}}{V_1}$$

Ratio of harmonic content to fundamental. Below 0.001% is state-of-the-art audio; above 1% is clearly audible. THD alone doesn't capture character (even harmonics sound warm, odd harmonics sound harsh).

**Signal-to-noise-and-distortion ratio**

$$SINAD = \frac{\text{Signal}}{\text{Noise} + \text{Distortion}} \text{ (in dB)}$$

The most comprehensive single-number metric for converter performance. Combines noise and distortion into one figure.

**Effective number of bits**

$$ENOB = \frac{SINAD - 1.76}{6.02}$$

Actual resolution achieved by an ADC accounting for noise and distortion. A 16-bit ADC with 84 dB SINAD has ENOB of ~13.7 bits, wasting more than 2 bits on real-world impairments.

**Jitter-limited SNR**

$$SNR_{jitter} = -20 \log_{10}(2\pi f \cdot \Delta t_{rms})$$

Where $f$ = signal frequency, $\Delta t_{rms}$ = RMS jitter. Jitter matters more at higher signal frequencies. A DC signal is unaffected. For 20 kHz audio at 16-bit quality (96 dB SNR), the clock needs less than 1 ns RMS jitter.

**dBFS spectral level**

$$\text{Level}_{dBFS} = 20 \log_{10}\!\left(\frac{|X[k]|}{N/2}\right)$$

For a single tone at bin center. In digital audio, spectral levels are referenced to full scale; a full-scale sine reads 0 dBFS, everything else reads negative. Different FFT implementations normalize differently — always verify with a known signal.

---

## Digital & Signal Integrity

**Dynamic switching current (CMOS)**

$$I_{dynamic} = C_{load} \times V_{DD} \times f_{switch}$$

Every gate draws current during transitions. Thousands switching simultaneously create cumulative current transients with high-frequency harmonics determined by edge rate, not clock frequency.

**Ground bounce**

$$V_{bounce} = L \frac{dI}{dt}$$

Current transients through parasitic ground inductance shift all logic levels. 10 pins switching 10 mA each in 1 ns through 5 nH gives 0.5 V of bounce — serious on a 3.3 V system with 0.4 V noise margin.

**PDN target impedance**

$$Z_{target} = \frac{\Delta V}{I_{transient}}$$

The power distribution network impedance must stay below this from DC to hundreds of MHz. For 1.0 V core with 5% tolerance and 10 A transient: $Z_{target} = 5\text{ m}\Omega$.

**Reflection coefficient**

$$\Gamma = \frac{Z_L - Z_0}{Z_L + Z_0}$$

Ranges from -1 to +1. $\Gamma = 0$ means matched (no reflection). $\Gamma = +1$ means open circuit (voltage doubles). $\Gamma = -1$ means short circuit (voltage cancels). Applies to both PCB signal integrity and RF transmission lines.

**Metastability MTBF**

$$MTBF = \frac{1}{f_{clk} \cdot f_{data} \cdot T_w \cdot e^{-t_r / \tau}}$$

Where $f_{clk}$ = destination clock frequency, $f_{data}$ = asynchronous data transition rate, $T_w$ = metastability vulnerability window, $t_r$ = available resolution time (clock period minus setup time), $\tau$ = flip-flop resolution time constant. Higher clock frequencies dramatically reduce MTBF. A two-stage synchronizer in modern 28 nm at 200 MHz easily achieves MTBF $\gg 10^{15}$ years; at 1 GHz the same synchronizer might yield MTBF of only months.

---

## Radio & RF

**VSWR from reflection coefficient**

$$VSWR = \frac{1 + |\Gamma|}{1 - |\Gamma|}$$

Ratio of voltage maximum to minimum on a standing wave. VSWR 1:1 is perfect; 2:1 is acceptable for many systems (~11% reflected power); 3:1 indicates poor match (~25% reflected).

**Return loss**

$$\text{Return loss (dB)} = -20 \log_{10}(|\Gamma|)$$

Reflected power in decibels. Higher is better. 20 dB means 1% reflected; 10 dB means 10%.

**Power delivered to load**

$$P_{load} = P_{forward} \times (1 - |\Gamma|^2)$$

Forward power minus reflected power.

**Bandwidth from loaded Q**

$$BW = \frac{f_{center}}{Q_{loaded}}$$

Bandwidth over which a matching network stays acceptable. Higher Q means sharper resonance and narrower usable bandwidth.

**Antenna radiation efficiency**

$$\eta = \frac{R_{rad}}{R_{rad} + R_{loss}}$$

Where $R_{rad}$ is radiation resistance and $R_{loss}$ is ohmic loss. Half-wave dipoles achieve ~98%; electrically small antennas can drop to 0.5%. Loading coils add loss resistance that further reduces efficiency.

**Antenna effective aperture**

$$A_e = \frac{G \lambda^2}{4\pi}$$

Where $G$ = antenna gain (linear), $\lambda$ = wavelength. Describes how much area the antenna captures from incoming radiation. Larger than physical size at low frequencies, which is why satellite dishes must be physically larger at lower frequencies for the same gain.

**Half-wave dipole length (with end-effect correction)**

$$L = \frac{0.95 \times c}{2f} = \frac{142.5}{f_{MHz}} \text{ (meters, for each arm)}$$

The 0.95 factor accounts for charge accumulation at wire tips making the antenna electrically longer than its physical length. Thicker wires have stronger end effects and need to be shorter.

**Characteristic impedance from velocity and capacitance**

$$Z_0 = \frac{1}{v \times C_{per\ length}}$$

Bench technique for estimating $Z_0$ when TDR or VNA is not available.

**TDR initial voltage step**

$$V_{initial} = V_{source} \times \frac{Z_0}{Z_{source} + Z_0}$$

The voltage that first appears on the line before reflections return.

---

## DSP & Sampling

**DFT frequency bin**

$$f_k = \frac{k \cdot f_s}{N}$$

Where $k$ = bin index (0 to $N-1$), $f_s$ = sample rate, $N$ = transform length. Bin spacing (frequency resolution) is $f_s / N$. Higher frequency resolution requires longer FFT, trading off against time resolution.
