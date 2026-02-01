---
title: "EMI/EMC Reality (When Your Circuit Becomes a Radio)"
weight: 5
---

# EMI/EMC Reality (When Your Circuit Becomes a Radio)

Every circuit is a radio. It transmits and receives whether you intended it to or not. EMI (electromagnetic interference) is the unwanted signal; EMC (electromagnetic compatibility) is the discipline of making circuits coexist without interfering with each other. In a compliance lab, these are measured against standards with calibrated antennas and receivers. At the bench, they're the reason your ADC reads garbage, your microcontroller resets when the relay fires, or your audio circuit buzzes when you plug in USB.

This page is the conceptual framework for the diagnostic procedures in the rest of this section. It explains why circuits behave like radios, what the symptoms look like, and what to try first — all without a compliance lab.

## Emissions vs. Susceptibility

EMI problems have two sides:

**Emissions:** Your circuit is the noise source. It radiates or conducts electromagnetic energy that interferes with other equipment. On the bench, you may not notice your own emissions — the victim is someone else's circuit, a nearby radio, or a pre-compliance test that fails.

**Susceptibility (immunity):** Your circuit is the victim. Someone else's electromagnetic energy (or your own, coupling back in) causes malfunction. On the bench, susceptibility shows up as symptoms: noise on measurements, resets, data corruption, audio artifacts.

Most bench debugging is susceptibility work — you're seeing symptoms in your circuit and trying to figure out what's causing them. But the fixes are symmetric: reducing emissions from a source and reducing susceptibility in a victim use the same techniques (filtering, shielding, layout, grounding). Fix one and you often fix both.

## The Four Coupling Mechanisms

Noise gets from source to victim through one of four paths. The existing page [Is This Conducted or Radiated?]({{< relref "conducted-radiated" >}}) covers how to distinguish them experimentally. Here's the physical intuition for each:

### Conducted (Galvanic)

The source and victim share a wire — usually a ground wire or power rail. Current from the source flows through the shared impedance, developing a voltage that the victim sees. This is the most common coupling mechanism at low frequencies.

**Mental model:** Two circuits sharing a garden hose. When one turns on its tap, the pressure changes for the other.

**Key variable:** Shared impedance. Even milliohms of ground resistance create millivolts of noise at amp-level currents. At high frequencies, shared inductance (nanohenries of shared trace) matters more than resistance.

### Capacitive (Electric Field)

A changing voltage on one conductor induces current in a nearby conductor through the capacitance between them. The coupling strength depends on the capacitance (set by area, distance, and dielectric) and the rate of voltage change (dV/dt).

**Mental model:** Two parallel wires are a capacitor. A fast voltage step on one wire pushes charge onto the other.

**Key variable:** dV/dt and coupling capacitance. A 3.3V signal switching in 1 ns has the same capacitive coupling strength as a 330V signal switching in 100 ns. Fast edges are the problem, not high voltages.

### Inductive (Magnetic Field)

A changing current in one conductor creates a changing magnetic field that induces voltage in a nearby loop. This is transformer action — the source conductor is the primary, and the victim loop is the secondary.

**Mental model:** Your circuit contains accidental transformers. Every current loop near another current loop is a loosely coupled transformer.

**Key variable:** dI/dt, loop area, and proximity. A switching converter's input loop (carrying amps of pulsed current) magnetically couples into a nearby analog signal loop. The induced voltage is proportional to the mutual inductance between the loops and the rate of current change.

### Radiated (Far-Field)

At distances greater than roughly a wavelength, the electric and magnetic fields detach from the conductors and propagate as electromagnetic waves. The source is an antenna; the victim is an antenna. Any conductor whose length is a significant fraction of the wavelength radiates (or receives) efficiently.

**Mental model:** Your PCB trace is an antenna. At 100 MHz, a wavelength is 3 meters. A 15 cm trace is 1/20 of a wavelength — already a non-trivial antenna. At 1 GHz, a 3 cm trace is a quarter-wave antenna.

**Key variable:** Frequency. Radiation efficiency increases with frequency. The same circuit that has no EMI problems at 1 MHz can be a significant radiator at 100 MHz — not because the fundamental frequency changed, but because the harmonics of a fast edge extend into the hundreds of MHz.

## Why Fast Edges Cause RF Behavior

This is the single most important concept in practical EMI. **It's not the clock frequency that causes EMI problems — it's the edge rate.**

A 10 MHz clock is not a 10 MHz sine wave. It's a trapezoidal wave with harmonics at 10, 30, 50, 70, 90, 110... MHz. The harmonic content extends up to roughly:

f_knee ≈ 0.35 / t_rise

Where t_rise is the 10–90% rise time. A signal with a 1 ns rise time has significant spectral content up to 350 MHz, regardless of the fundamental frequency. A 1 MHz signal with 1 ns edges produces more high-frequency EMI than a 100 MHz signal with 10 ns edges.

### What This Means in Practice

- A 3.3V GPIO toggling at 1 kHz with a 2 ns edge produces harmonics into the hundreds of MHz — radio frequencies
- SPI clock at 10 MHz with sub-nanosecond edges from a modern CMOS driver has spectral energy well past 500 MHz
- A switching converter at 500 kHz with 5 ns switching transitions has EMI content through 70 MHz

The fix is sometimes as simple as slowing the edges. Adding a series resistor (22–100 ohms) to a digital output creates an RC filter with the load capacitance that slows the edge without significantly affecting digital timing. The high-frequency harmonic content drops dramatically, and EMI problems diminish.

## Common Real-World Symptoms

Recognizing EMI symptoms is half the battle. The other half is covered in [Where Is This Noise Coming From?]({{< relref "noise-source" >}}).

### Random Resets or Lockups

The microcontroller resets, watchdog fires, or the processor locks up — but only when a specific load is activated (motor, relay, solenoid), when a cable is connected, or intermittently with no obvious trigger.

**What's happening:** A transient on the power rail, reset line, or clock line is disrupting the processor. The transient may be conducted (through shared power or ground) or radiated (EMI coupling into a trace that acts as an antenna).

**First check:** Monitor the power rail and reset pin on the scope during the triggering event. If you see a voltage dip or spike that correlates with the reset, the coupling is conducted through the power or reset path.

### ADC Noise Floor Increase

ADC readings become noisier, with increased standard deviation, when nearby equipment is active (switching supply, motor, display backlight).

**What's happening:** Noise is coupling into the ADC input, reference, or power supply. Common paths: capacitive coupling from a noisy digital trace into the analog input trace, conducted noise on the power rail feeding the ADC, or inductive coupling from a high-current switching loop into the analog signal loop.

**First check:** AC-couple the scope to the ADC input pin and look for switching-frequency components or broadband noise that correlates with the interfering equipment.

### Audio Hum, Buzz, or Whine

Hum at mains frequency (50/60 Hz or 100/120 Hz), buzz with harmonic content, or a whine at a switching frequency appearing in audio output.

**What's happening:** Ground loops are the usual suspect for hum. Switching power supply noise coupling into the audio signal path causes whine. The diagnostic flowchart is covered in [Is There a Ground Loop?]({{< relref "ground-loop" >}}).

### RF Desensitization

A radio receiver loses sensitivity (reduced range, higher noise floor) when nearby digital electronics are active.

**What's happening:** The digital circuit is emitting broadband RF noise that falls within the receiver's passband. Even microwatts of interference can desensitize a receiver designed to work with picowatts of signal. Fast digital edges on PCB traces, cables acting as antennas, and unshielded clock harmonics are the usual sources.

### "Touching It Fixes It"

A problem goes away when you touch the enclosure, cable, or a specific part of the board. Or it appears only when you remove your hand.

**What's happening:** Your body is either providing a ground path (draining charge or providing shielding) or adding capacitance that detunes an antenna or shifts a resonance. This is a strong indicator that the coupling is capacitive or radiative and that the circuit has an uncontrolled impedance (floating ground, unterminated cable, ungrounded shield).

**First check:** If touching the chassis fixes it, the chassis needs a better ground connection. If touching a cable fixes it, that cable is acting as an antenna and needs shielding or common-mode filtering.

### Intermittent Communication Errors

SPI, I2C, UART, or CAN communication occasionally drops bytes or produces framing errors — correlating with switching events or motor activity but not with data rate or bus loading.

**What's happening:** Transients on the communication lines are corrupting edges. A noise spike during a bit period flips a bit or triggers a false clock edge. The coupling path may be conducted (shared ground impedance) or capacitive/inductive (proximity of noisy traces to communication lines).

## The "First Moves" — What Usually Works

These are the high-value, low-effort changes that fix most bench-level EMI problems. More detailed measurement-based approaches are in [Does Shielding / Rerouting Fix It?]({{< relref "shielding-rerouting" >}}).

### Reduce Loop Area

The most powerful single technique. Both inductive coupling (as a victim) and magnetic radiation (as a source) are proportional to loop area. For every signal carrying current, the signal and its return should travel together — on adjacent traces, in twisted pairs, or as a trace over a ground plane.

**How to apply it:**
- Keep decoupling capacitors directly adjacent to the IC power pins — the loop is VDD pin → cap → GND pin → IC ground. Every millimeter of trace between the cap and pin is added loop area
- Route high-speed signals over a continuous ground plane. Slots or gaps in the plane force the return current to detour, creating a larger loop
- Twist power and ground wires together for off-board connections
- Keep the area of the switching converter's input loop (switch → input cap → ground → switch) as small as possible. This is the highest-dI/dt loop in most designs

### Improve the Return Path

Current takes the path of least impedance — at DC, that's least resistance. At high frequency, that's the path that minimizes loop inductance, which means the return current flows directly under the signal trace on the nearest ground plane.

If the return path is disrupted (split ground plane, missing ground pour, long ground wire), the current finds an alternate path that creates a larger loop and more radiation/coupling.

**How to apply it:**
- Don't split ground planes unless you have a specific reason and understand where the return currents flow
- For every cable leaving the board, provide a ground return in the same cable or connector
- On multi-layer boards, dedicate a full layer to ground — unbroken, no routing on it

### Decouple Correctly

Decoupling capacitors are high-frequency short circuits between VDD and GND at the IC. They supply transient current locally so it doesn't flow through long power traces (which radiate). But placement and trace routing matter more than capacitor value:

- **Close to the pin:** The trace from the cap to the IC pin is inductance that limits high-frequency effectiveness. Shorter is better. Under the IC (on the opposite side of the board, connected by vias directly to the pads) is ideal
- **Multiple values:** A 100 nF ceramic for mid-frequency decoupling and a 10 uF bulk cap for lower-frequency transients is a standard combination. Don't add too many values — multiple parallel caps can create parallel resonances with unexpected impedance peaks
- **Low-ESL package:** 0402 or 0201 capacitors have lower parasitic inductance than 0805. For high-frequency decoupling, the package matters more than the capacitance value

### Add Series Resistance on Fast Digital Outputs

A 22–100 ohm resistor in series with a digital output creates an RC low-pass with the trace and load capacitance. This slows the edge without affecting the logic level:

- A 33 ohm resistor on a GPIO driving 10 pF of trace capacitance gives a time constant of 330 ps — turning a 1 ns edge into roughly a 2 ns edge. The 350 MHz harmonic content drops to 175 MHz
- This is free EMI reduction. No cost, no complexity, no functional impact for most digital signals. Only avoid it on signals where edge timing is critical (high-speed data buses, precision clocks)

### Ferrites on Cables

Snap-on ferrite clamps or ferrite beads in series with cables attenuate common-mode high-frequency current. They don't affect differential-mode signals. Effective on:

- Power cables carrying conducted noise out of (or into) an enclosure
- USB, HDMI, and other data cables radiating or picking up noise
- Motor power leads carrying commutation noise

Choose the ferrite for the frequency range of your noise. Most snap-on ferrites are effective from ~1 MHz to ~300 MHz. Below 1 MHz, they're too small to matter. Above 300 MHz, they may be resonant or capacitive.

### Shielding

A grounded conductive enclosure (metal box) attenuates both emissions and susceptibility for electric-field coupling and radiated coupling above a few MHz. But shielding is only as good as its worst gap:

- Every opening in the shield (ventilation holes, cable entry points, seams) is a slot antenna that leaks at wavelengths comparable to the slot length
- Cables penetrating the shield must be filtered (ferrites, filtered connectors) or shielded themselves, with the cable shield bonded to the enclosure at the entry point
- A shield that's grounded at one point is effective against electric fields but not magnetic fields at low frequencies. For magnetic shielding below ~1 MHz, you need high-permeability material (mu-metal) or thick steel

### Filtering at the Board Edge

Every signal and power line entering or leaving the board is a potential EMI antenna. Filtering at the board edge (close to the connector) attenuates noise before it reaches the cable:

- LC or ferrite + cap filters on power inputs
- RC filters or common-mode chokes on signal lines
- TVS diodes for transient clamping (which also limits EMI-induced voltage spikes)

### Separation

Physical distance is the cheapest filter. Capacitive coupling falls as 1/distance. Inductive coupling falls as 1/distance² to 1/distance³ depending on geometry. Radiated coupling falls as 1/distance in the far field.

On a PCB: keep analog sections away from switching regulators and high-speed digital. On a bench: move the motor driver away from the sensitive sensor board. In a system: route noisy cables and quiet cables on different sides of the enclosure.

## Measurement Approaches with Basic Tools

You don't need a $200K EMC test setup to find and fix most problems. Here's what works with bench equipment.

### Scope Probing — and the Pitfalls

The oscilloscope is your primary tool, but probe technique determines whether you're measuring the circuit or measuring the probe:

**Ground lead length is everything.** The standard 6-inch ground clip lead adds ~15 nH of inductance. With 10 pF of probe capacitance, that resonates at ~400 MHz. Any energy near 400 MHz in the circuit rings in the probe, creating artifacts that aren't in the circuit. For EMI work:

- Use the ground spring (tip-and-barrel) accessory that came with the probe
- Or solder a short ground wire directly to a ground point within 1 cm of the measurement point
- If the ringing goes away when you shorten the ground, it was probe artifact, not circuit behavior

**Bandwidth limiting:** If you're looking for specific noise (e.g., switching frequency), use the scope's bandwidth limit to filter out higher frequencies that confuse the picture. If you're hunting broadband EMI, use full bandwidth.

**AC coupling:** Essential for seeing small noise riding on a large DC offset. But AC coupling has a low-frequency cutoff (typically ~10 Hz), so it filters out mains-frequency hum. Switch between AC and DC coupling to see different parts of the noise spectrum.

### Near-Field Probes

A near-field probe is a small loop or stub antenna connected to the scope or spectrum analyzer input. It picks up magnetic (loop) or electric (stub) fields in the immediate vicinity of the source — within millimeters.

**DIY near-field probes work fine for debugging:**
- **H-field (magnetic) probe:** A small loop of wire (5–15 mm diameter) with a coax cable to the scope. Hold it near traces, inductors, and ICs to find where magnetic field (current-loop radiation) is strongest. Orient the loop parallel to the surface for maximum sensitivity to horizontal currents
- **E-field (electric) probe:** A short wire stub (10–20 mm) on a coax cable. Detects electric field (voltage changes). Hold it near traces and components to find dV/dt sources

Move the probe across the board while watching the scope. The signal peaks when the probe is directly over the noise source. This localizes the source to a specific component or trace segment.

### SDR as a Noise Sniffer

A cheap SDR dongle (RTL-SDR, ~$25) with a small whip antenna or near-field probe gives you a wideband spectrum view from ~25 MHz to ~1.7 GHz:

- Set the SDR software to a waterfall or spectrum display
- Sweep across the frequency range and look for strong narrowband emissions (harmonics of clocks and switching frequencies) and broadband noise floors
- Use it for A/B comparison: take a spectrum snapshot, make a change, take another snapshot. If the emissions drop, the change worked
- Not calibrated, not a substitute for a real receiver, but enormously useful for identifying dominant emission frequencies and verifying that fixes reduce them

### The A/B Method

The most powerful technique for EMI debugging with basic tools:

1. Measure the symptom (scope waveform, SDR spectrum, functional behavior)
2. Make one change (add ferrite, add cap, reroute wire, add ground, add shield)
3. Measure again under the same conditions
4. Compare

If the symptom improved, the change addressed the coupling path. If it didn't, undo it and try something else. If it got worse, you've learned something valuable about the coupling mechanism.

This is empirical and unglamorous, but it works. The key discipline is changing only one thing at a time and measuring before and after.

## How to Avoid Making It Worse

Debugging EMI is easy to do wrong. Each probe, wire, and bodge you add to the circuit changes its electromagnetic behavior:

- **Long scope probe ground leads act as antennas.** They pick up radiated noise and inject it into the measurement, making the noise appear worse than it is. They can also radiate noise from the measurement point back into the circuit
- **Bodge wires are antennas.** A wire soldered to a board as a temporary fix adds an uncontrolled antenna. Keep bodge wires as short as possible and route them close to a ground return
- **Adding a filter cap in the wrong place can create a resonance.** A capacitor in parallel with an inductive path creates an LC resonant circuit. At the resonant frequency, the impedance peaks instead of filtering. This can amplify noise at a specific frequency while attenuating others
- **Moving cables during measurement changes the result.** Cable position affects both conducted and radiated coupling. If the noise changes when you move a cable, that cable is part of the coupling path — but the measurement is only valid for that specific cable position. Secure cables in a repeatable position for A/B comparisons
- **Your body is a conductor and a capacitor.** Standing near the DUT affects the electromagnetic environment. If the noise changes when you step back, your body was part of the coupling path. For repeatable measurements, maintain consistent physical position

## Gotchas

- **The loudest emission is not always the problem.** A circuit may radiate strongly at the switching frequency (which is normal and expected) while the actual functional problem is caused by a harmonic at 10x the switching frequency that happens to fall in a victim receiver's band. Don't assume the biggest spike on the spectrum analyzer is the one causing your symptom
- **Ground pour doesn't help if it's not connected.** Copper fill on a PCB only provides shielding or return path if it's well-connected to the ground net with many vias. An isolated copper island on the board is a parasitic capacitor and a potential antenna, not a shield
- **"It passes on the bench but fails in the enclosure" is common.** The enclosure changes every resonance, reflection, and coupling path. EMI behavior in the final enclosure can be significantly different from the open-bench case — sometimes better, sometimes worse
- **Ferrites are not magic.** They work by adding resistive impedance at high frequency. They don't help below their effective frequency range, they saturate if DC current is too high, and they can resonant with parasitic capacitance. Check the impedance-vs-frequency curve for the specific ferrite and verify it covers your noise frequency
- **Shielded cables with ungrounded shields are worse than unshielded.** A floating shield acts as a capacitive coupler, picking up fields and coupling them to the inner conductor. Connect the shield to ground at the appropriate end (usually the source end for audio, both ends for RF)
- **Adding more decoupling capacitors eventually stops helping.** Beyond a certain point, adding parallel capacitors creates parallel resonances (anti-resonances) where the impedance is higher than with fewer caps. Two well-placed caps beat ten poorly-placed ones
- **Switching frequency spread spectrum does not reduce total energy — it spreads it.** The peak emission at any single frequency drops, which helps pass narrowband compliance tests, but the total radiated energy is the same. If your problem is broadband susceptibility (like ADC noise), spread spectrum doesn't help
- **The problem often isn't where the symptom appears.** Noise on an ADC input may originate from a switching regulator on the other side of the board, coupling through the ground plane, through the power rail, through the air, or through a cable. Localizing the source is the first step — jumping to a fix at the symptom location often fails because the coupling path is elsewhere
