---
title: "ESD & Transient Protection Basics"
weight: 50
---

# ESD & Transient Protection Basics

ESD and electrical transients are two different failure mechanisms that produce frustratingly similar results: circuits that work intermittently, ICs that die without visible damage, and analog peripherals that slowly degrade. This page covers what's actually happening, how to avoid causing damage at the bench, and how to design protection into circuits.

## What ESD Actually Is

Electrostatic discharge is a fast, high-voltage, low-energy event. Your body accumulates charge from friction with clothing, chairs, carpeting — the triboelectric effect. Typical body voltages range from 2–15 kV depending on humidity and materials, though you won't feel a discharge below about 3 kV. That means you can destroy a MOSFET gate oxide at 100V and never know it happened.

The discharge itself is extremely fast — nanosecond rise times, peak currents of several amps for microseconds. The total energy is small (microjoules), so it won't burn a trace off a board. Instead, it punches through thin oxide layers, damages junction structures, or creates latent defects that degrade over time.

### The Human Body Model (HBM)

The standard ESD model for handling damage: your body is roughly a 100 pF capacitor discharged through a 1.5 kOhm resistor (your skin and finger). A 2 kV HBM event delivers about 1.3A peak with a ~10 ns rise time. Most modern ICs are rated to survive 1–2 kV HBM at their pins. Exceed that, or hit an unprotected internal node, and you're into damage territory.

### Why ICs Die Without Sparks

You need ~3 kV to feel it, ~5 kV to see/hear a spark. CMOS gate oxide can fail at 30–100V. The gap between "I felt nothing" and "I just killed that chip" is enormous. This is why ESD damage is so insidious — there's no sensory feedback.

## ESD vs. Electrical Transients

These are different problems that need different solutions, but both kill electronics:

| | ESD | Electrical Transient |
|---|---|---|
| **Source** | Human body, charged objects, triboelectric buildup | Inductive kick, relay coils, motor commutation, hot-plug, lightning coupling |
| **Voltage** | 1–25 kV typical | Tens to hundreds of volts (or kV for lightning) |
| **Duration** | Nanoseconds | Microseconds to milliseconds |
| **Energy** | Microjoules | Millijoules to joules |
| **Repetition** | Random, per-touch | Can be periodic (motor commutation) or event-driven (relay cycling) |
| **Primary damage** | Oxide punch-through, junction degradation | Overvoltage breakdown, overcurrent, thermal damage |
| **Protection strategy** | Handling discipline, ESD structures on-chip | External protection components in the circuit |

### Common Transient Sources at the Bench

- **Relay coils:** Deenergizing a relay without a flyback diode produces a voltage spike of 10–100× the supply voltage. A 12V relay can kick back 200V+
- **Motor commutation:** Brush motors generate continuous transient noise. Every commutator segment transition is a small inductive event
- **Hot-plug connectors:** Plugging in a USB device, sensor, or module while powered creates a current inrush and potential ground bounce. Connectors with staggered pin lengths (ground-first) exist for a reason
- **Solenoids and actuators:** Same physics as relays — any inductive load that gets switched produces a flyback spike
- **Long cable runs:** Act as antennas, coupling nearby transients or lightning-induced surges into your circuit

## What's Already Inside the IC

Most modern ICs include ESD protection diodes on every pin — typically a pair of diodes clamping the pin to VDD and VSS:

```
VDD ──┐
      ▼ (diode)
Pin ──┤
      ▼ (diode)
VSS ──┘
```

These diodes are designed to clamp fast ESD events (nanoseconds) and shunt the current to the supply rails. They are **not** designed for:

- Sustained overvoltage — they'll conduct and overheat if you apply 6V to a 3.3V input
- High-energy transients — they're tiny structures with limited current handling
- Voltages significantly beyond the supply rails when the IC is unpowered (since VDD is floating, the clamp has nowhere to shunt current)
- Analog pins where even small leakage currents through the clamp diodes affect measurement accuracy

### Latch-up

CMOS ICs contain parasitic thyristor structures (PNPN paths between VDD and VSS). If an overvoltage event forward-biases one of the internal junctions enough to trigger this parasitic SCR, the chip enters latch-up: a low-impedance path from VDD to VSS that draws excessive current and usually destroys the chip unless power is removed within milliseconds. Latch-up is triggered by voltages on I/O pins that exceed VDD or go below VSS — exactly what happens during transient events or when powering on modules in the wrong sequence.

## Bench Handling Checklist

These are habits, not occasional procedures. Do them every time.

### Minimum Viable ESD Discipline

- [ ] **Wrist strap connected to a known ground point** — the strap's 1 MOhm resistor limits current if you accidentally contact a live circuit while providing a bleed path for static charge. Test the strap occasionally with a wrist strap tester; open straps are common
- [ ] **ESD mat on the work surface** — connected to the same ground as the wrist strap. The mat is slightly conductive (10^6 to 10^9 ohms surface resistance), draining charge slowly rather than creating a discharge event
- [ ] **Touch a grounded surface before handling boards** — if you don't have a mat/strap setup, this is the absolute minimum. Touch chassis ground, bench frame, or a grounded tool before picking up a board
- [ ] **Handle boards by the edges** — avoid touching pins, exposed traces, connector contacts, or IC packages directly
- [ ] **Keep ICs in anti-static packaging until ready to solder/install** — the pink/silver bags and black foam are conductive or dissipative; regular plastic bags are triboelectric charge generators
- [ ] **Don't slide boards across surfaces** — sliding generates triboelectric charge. Pick up, place down
- [ ] **Power down and discharge before connecting/disconnecting anything** — hot-plugging without designed-in protection is asking for latch-up or pin damage

### Environment Factors

- **Humidity matters.** Below 30% RH, static charge accumulates readily. Above 50% RH, surface moisture provides a natural bleed path. Winter in heated buildings is peak ESD season
- **Synthetic clothing is the enemy.** Polyester, nylon, and fleece are triboelectric nightmares. Cotton or ESD-rated garments are better
- **Carpet is the enemy.** Walking across carpet in dry air can generate 10–15 kV. Hard floors or anti-static mats on the floor help
- **Plastic bins, bags, and bubble wrap** generate and hold charge. Use anti-static versions for electronics storage

### When the Full Setup Isn't Available

If you're doing quick bench work without a mat and strap:

1. Ground yourself by touching a grounded metal object before each time you touch the board
2. Work on a metal surface (even a bare aluminum sheet connected to earth ground is better than nothing)
3. Keep the board on its anti-static bag while working on it — note: the *outside* of the bag is where the conductive layer is; this only helps if the board is sitting *on* the bag, not *in* it with the bag open
4. Avoid reaching for components directly after rolling your chair back (chair + clothing = charge generation)

## Design Protection Checklist

For circuits you're building, these are the standard defenses against overvoltage and transient damage.

### ESD and Overvoltage on Signal Lines

- **TVS (Transient Voltage Suppressor) diodes** on any pin that connects to the outside world: connectors, cables, user-accessible I/O. TVS diodes clamp fast (sub-nanosecond) and handle real energy. Place them as close to the connector as possible, with short, wide traces to ground. Bidirectional TVS for AC or signals that swing both polarities; unidirectional for DC-only lines
- **Series resistors** on I/O pins — even 100–470 ohms limits peak current during an ESD event and works with the IC's internal clamp diodes. The series resistor + clamp diode combination is often enough for board-level ESD
- **Input clamp diodes to the supply rails** — if the IC's internal clamps aren't sufficient or you want explicit protection, add external Schottky diodes from the signal line to VDD and VSS. Use Schottky for lower forward voltage and faster clamping

### Inductive Load Protection

- **Flyback diodes across every relay coil, solenoid, and motor** — a 1N4148 or 1N4007 reverse-biased across the coil. When the driver turns off, the inductor's voltage reversal forward-biases the diode and the stored energy dissipates through it instead of spiking the driver's output pin. For faster release, use a Zener + diode combination to clamp at a controlled voltage above the supply
- **RC snubbers across relay contacts** — the contact arc when a relay opens generates broadband RF noise and erodes the contacts. A series RC (typically 100 ohm + 100 nF) across the contacts damps the arc. Values depend on load current and contact rating

### Power Supply and Connector Protection

- **TVS on power input** — catches transients that make it past upstream regulation
- **Bulk capacitance at connectors** — absorbs inrush current during hot-plug events
- **Ferrite beads on power and signal lines** entering or leaving the board — ferrites are resistive at high frequencies, attenuating fast transient edges without affecting DC or low-frequency signals
- **Connector pin sequencing** — on custom connectors, make ground pins longer so they mate first and break last. This prevents signal pins from seeing undefined ground during plug/unplug
- **Reverse polarity protection** — a series Schottky or a P-MOSFET in the high side prevents damage when someone inevitably connects power backwards

### General Layout Principles

- Protection components go at the board edge, as close to the connector as possible. The trace between the connector and the TVS/clamp should be as short as possible — every millimeter of trace between the connector and the clamp is inductance that reduces the clamp's effectiveness
- Ground pour under protection components — they need a low-impedance return path
- Keep protected and unprotected traces separated — don't route an unprotected signal next to a protected one, or the transient couples right past your protection

## Common Failure Patterns

Recognizing ESD and transient damage at the bench:

### Dead or Flaky GPIO

**Symptom:** One or two GPIO pins on a microcontroller stop responding, or work intermittently. The rest of the chip functions normally.

**What happened:** ESD or overvoltage on that specific pin punched through the internal clamp diode or damaged the output driver. The die-level damage is localized to the I/O cell for that pin.

**Diagnostic clue:** Measure the pin's leakage current with the pin configured as high-impedance input. A healthy CMOS pin draws picoamps to nanoamps. A damaged pin may show microamps of leakage. Also check if the pin is shorted to VDD or VSS.

### Increased ADC Noise or Offset

**Symptom:** ADC readings become noisier or show a consistent offset that wasn't there before. Sometimes one channel degrades while others stay clean.

**What happened:** The analog input structure or the ESD clamp diode on that pin has been partially damaged. The junction now has increased leakage current, which injects noise and pulls the input voltage slightly toward a rail.

**Diagnostic clue:** Measure the input with nothing connected (pin floating or driven to a known voltage from a low-impedance source). Compare noise floor and DC offset against a known-good channel or a known-good part.

### Mysterious Partial Failures

**Symptom:** A board "mostly works" but has intermittent glitches — communication errors, occasional resets, wrong readings from one sensor. These symptoms may be temperature-dependent (heat increases leakage through damaged junctions).

**What happened:** Latent ESD damage. The initial event weakened a junction or thinned an oxide layer without causing immediate failure. Over time (hours to months), the damaged structure degrades further. This is the worst kind of ESD damage because it passes initial testing and fails in the field.

**Diagnostic clue:** Latent failures are hard to distinguish from other intermittent bugs. If you see a board that worked fine initially and is degrading over time, especially if the failures are in I/O or analog functions, ESD damage is a likely suspect. Swapping the IC (if socketed) or comparing against a known-good board is often the fastest path.

### Latch-up in the Field

**Symptom:** A board suddenly draws excessive current (often saturating the regulator or blowing a fuse), gets hot, and stops functioning. Power-cycling restores normal operation — until the next triggering event.

**What happened:** A transient on an I/O pin triggered CMOS latch-up. The parasitic SCR latched on and created a VDD-to-VSS short through the substrate. If power is removed fast enough, the chip survives. If not, thermal damage finishes it off.

**Diagnostic clue:** Monitor supply current. If you see sudden jumps from normal operating current (milliamps) to hundreds of milliamps or amps, followed by recovery on power cycle, that's classic latch-up. Check what external event correlates with the trigger — it's usually a cable being connected, a relay switching, or a motor starting.

## Gotchas

- **Anti-static bags are not insulating pouches.** The pink poly bags are dissipative; the metalized silver bags are shielding (Faraday cage). Neither is a substitute for an ESD mat. Don't use them as work surfaces unless the board is sitting *on top* of the bag's conductive exterior
- **Wrist straps fail silently.** The coiled cord breaks internally, the snap corrodes, the banana plug oxidizes — and you're ungrounded without knowing it. Test regularly with a wrist strap checker
- **"I've never had an ESD problem" usually means "I've had ESD problems I didn't diagnose."** Latent damage is the default outcome, not immediate death. Most ESD-damaged parts pass basic functional testing and fail later
- **Unpowered ICs are more vulnerable, not less.** With no power applied, the internal ESD clamp diodes have nowhere to shunt current (VDD is floating). An ESD strike on an unpowered board is more likely to cause damage than on a powered one
- **Latch-up can be triggered by power sequencing.** If an I/O pin is driven high by an external device before the IC's own VDD rail comes up, the pin voltage exceeds VDD, forward-biases the clamp diode into the substrate, and can trigger latch-up on power-up. This is a common problem with multi-rail designs and modular systems
- **Flyback diodes slow down relay release.** The diode clamps the voltage across the coil, which means the current decays slowly through the coil's L/R time constant. If you need fast relay release (for timing-critical applications), use a Zener + diode clamp instead — it allows the voltage to rise to the Zener voltage before clamping, which drives the current down faster
- **TVS diodes have capacitance.** High-speed signal lines (USB, SPI at tens of MHz, HDMI) need low-capacitance TVS arrays specifically designed for those protocols. A garden-variety TVS diode on a USB data line will roll off the signal edges and cause communication errors
- **The chassis is not always ground.** On plastic-enclosure products, there's no chassis to reference protection components to. On metal-enclosure products, chassis ground and circuit ground may be connected through an intentional impedance (common on audio equipment to avoid ground loops). Understand your grounding topology before deciding where to connect TVS and clamp returns
