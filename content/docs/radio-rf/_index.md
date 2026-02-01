---
title: "📡 Radio & RF"
weight: 6
bookCollapseSection: true
---

# Radio & RF

Where wavelength starts to matter.

Radio and RF engineering deal with signals whose physical size is comparable to the circuits that carry them. At these frequencies, energy moves through fields as much as through wires — PCB traces become components, parasitics dominate behavior, and intuition built from low-frequency circuits often breaks down.

Entries cover antennas, impedance matching, transmission lines, RF layout, propagation, and software-defined radio — with an emphasis on practical RF work: what actually happens on real boards and in real space, why measurements are hard, and how small physical details can make or break a design.

## Sections

- **[RF Fundamentals & Wavelength Thinking]({{< relref "rf-fundamentals" >}})** — Rewiring intuition for high-frequency design: wavelength, phase, distributed effects, and why RF problems feel non-local.
  *What this section covers:* Frequency vs Wavelength · Lumped vs Distributed · Phase, Delay & Timing · Skin Effect & Proximity Effect · When Short Wires Stop Being Short · Why RF Problems Feel Non-Local

- **[Transmission Lines]({{< relref "transmission-lines" >}})** — Characteristic impedance, reflections, VSWR, termination, and the behavior of cables and traces as RF components.
  *What this section covers:* What Makes a Transmission Line · Characteristic Impedance · Reflections, Standing Waves & VSWR · Termination Strategies · Cables as Components · Coax, Twisted Pair, Microstrip & Stripline · Measuring Lines

- **[Impedance Matching]({{< relref "impedance-matching" >}})** — Making power go where you want it: matching networks, Smith chart techniques, and real-world compromises.
  *What this section covers:* Why Mismatches Cause Reflections · Power Transfer vs Voltage Transfer · Simple Matching Networks · Smith Chart Intuition · Broadband vs Narrowband Matching · Matching Antennas, Amplifiers & Filters · Real-World Compromises & Tolerances

- **[Antennas]({{< relref "antennas" >}})** — Where circuits meet physics: antenna types, radiation resistance, polarization, tuning, and measurement.
  *What this section covers:* What an Antenna Actually Does · Common Antenna Types · Radiation Resistance & Efficiency · Ground Planes & Counterpoise · Polarization & Orientation · Antennas in Enclosures · Antenna Tuning & Trimming · Measuring Antennas

- **[RF Layout & PCB Design]({{< relref "rf-layout-and-pcb-design" >}})** — Copper geometry as circuit design: controlled impedance, return paths, via stitching, decoupling, and shielding.
  *What this section covers:* Why RF Layout Matters · Return Paths & Ground Strategy · Controlled Impedance Traces · Via Placement & Stitching · Decoupling at RF · RF Connectors & Transitions · Shielding & Isolation · Common Layout Mistakes

- **[RF Components & Building Blocks]({{< relref "rf-components" >}})** — The real behavior behind ideal symbols: RF passives, amplifiers, mixers, filters, oscillators, and datasheets.
  *What this section covers:* RF Capacitors & Inductors · RF Amplifiers & Gain Blocks · Filters · Oscillators & Phase Noise · Mixers & Frequency Translation · Switches, Attenuators & Couplers · Reading RF Datasheets

- **[Propagation & the Real World]({{< relref "propagation" >}})** — What happens after the signal leaves the board: path loss, multipath, materials, weather, and placement effects.
  *What this section covers:* Near-Field vs Far-Field · Free-Space Path Loss · Antenna Height & Placement · Frequency-Dependent Behavior · Reflections, Diffraction & Multipath · Indoor vs Outdoor Propagation · Weather, Materials & Environment

- **[Software-Defined Radio (SDR)]({{< relref "software-defined-radio" >}})** — Where the analog front end meets the digital back end: I/Q signals, sampling, architectures, and SDR as a learning tool.
  *What this section covers:* What SDR Replaces · I/Q Signals & Quadrature · Sampling Theory at RF · Common SDR Architectures · Dynamic Range & Front-End Limits · Practical SDR Tools & Workflows · SDR as a Learning Instrument

- **[Measurement, Debugging & RF Tools]({{< relref "measurement-debugging-and-rf-tools" >}})** — Seeing invisible problems: spectrum analyzers, VNAs, probing techniques, and debugging oscillations.
  *What this section covers:* Spectrum Analyzers · Vector Network Analyzers · Directional Couplers & Power Meters · Using Oscilloscopes at RF · Probing Without Destroying · Field Expedient Measurement · Debugging Oscillations, Noise & Spurs

- **[Practical RF Projects & Case Studies]({{< relref "practical-rf-projects" >}})** — Learning by building and breaking: transmitters, antenna experiments, SDR explorations, and postmortems.
  *What this section covers:* Simple Transmitters & Receivers · Antenna Tuning Experiments · SDR-Based Experiments · Enclosure Effects on RF · RF Mistakes & Postmortems · Lessons Learned
