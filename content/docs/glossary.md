---
title: "📖 Glossary"
weight: 100
---

# Glossary

A working reference of terms, abbreviations, and phrases used throughout this notebook. Organized alphabetically.

---

## Symbols & Numbers

<a id="0-ohm-resistor"></a>
**0 Ω resistor** — SMD jumper wire in resistor form. Used for routing convenience on PCBs, selectable configurations, and bridging traces. Resistance is extremely low but not zero.

<a id="3db-point"></a>
**3 dB point** — The frequency at which a filter's output drops to 70.7% of its passband value, corresponding to half power. Defines the boundary between passband and stopband.

<a id="10x-probe"></a>
**10× probe** — Oscilloscope probe with 10:1 attenuation. Presents roughly 10 MΩ ∥ 10 pF at the tip, significantly reducing probe loading compared to a 1× probe. The standard choice for most measurements.

---

## A

<a id="ac-coupling"></a>
**AC coupling** — Signal path that blocks DC and passes only the time-varying component. Implemented with a series capacitor. The low-frequency cutoff depends on the capacitor value and load impedance. See [Measurement Basics]({{< relref "/docs/fundamentals/units-notation-measurement/measurement-basics" >}}).

<a id="accuracy"></a>
**Accuracy** — How close a measurement reading is to the true value. Distinct from precision, which describes repeatability. A meter can be precise (consistent readings) but inaccurate (consistently wrong).

<a id="active-probe"></a>
**Active probe** — Oscilloscope probe with a built-in amplifier at the tip, presenting very low capacitance (~1 pF) and high impedance. Necessary for measuring fast signals where passive probe loading would distort the waveform.

<a id="aging"></a>
**Aging** — Logarithmic capacitance loss over time in Class II and III ceramic capacitors. Most of the change occurs shortly after manufacturing; the rate slows but never stops.

<a id="ampere"></a>
**Ampere (A)** — SI unit of electric current. One ampere equals one coulomb of charge per second.

<a id="angular-frequency"></a>
**Angular frequency (ω)** — Frequency expressed in radians per second: ω = 2πf. Appears in impedance formulas and transfer functions.

<a id="apparent-power"></a>
**Apparent power (S)** — The product of RMS voltage and RMS current, measured in volt-amperes (VA). Equals real power only when power factor is unity.

<a id="auto-ranging"></a>
**Auto-ranging** — Instrument feature that automatically selects the measurement range. Convenient but can be slow when the signal crosses range boundaries.

---

## B

<a id="bga"></a>
**BGA (Ball Grid Array)** — IC package with solder balls on the bottom surface instead of pins on the perimeter. High pin density but impossible to hand-solder and susceptible to thermal cycling fatigue.

<a id="boost-converter"></a>
**Boost converter** — Switching power supply topology that produces an output voltage higher than its input. Uses an inductor, switch, diode, and output capacitor.

<a id="branch"></a>
**Branch** — A path between two nodes in a circuit, carrying a single current.

<a id="buck-boost-converter"></a>
**Buck-boost converter** — Switching topology that can produce an output voltage either higher or lower than the input, with inverted polarity in the basic form.

<a id="buck-converter"></a>
**Buck converter** — Switching power supply topology that produces an output voltage lower than its input. The most common switching regulator topology.

<a id="burden-voltage"></a>
**Burden voltage** — Voltage drop across an ammeter's internal shunt resistor. Inserted into the circuit being measured, which means the measurement itself affects the circuit. Lower burden voltage means less measurement error.

<a id="bypass-capacitor"></a>
**Bypass capacitor** — Capacitor placed near an IC's power pins to supply instantaneous current demands and shunt high-frequency noise to ground. Functionally identical to a decoupling capacitor.

---

## C

<a id="c0g"></a>
**C0G / NP0** — Class I ceramic dielectric. The most stable ceramic capacitor type: negligible voltage coefficient, minimal temperature drift, no aging. Limited to small capacitance values. Use wherever stability matters.

<a id="carbon-composition"></a>
**Carbon composition resistor** — Vintage resistor type made from a solid slug of carbon and binder. Loose tolerance, high noise, poor stability — but handles surges well. Rarely used in new designs.

<a id="chassis-ground"></a>
**Chassis ground** — Electrical connection to the equipment's metal chassis or frame. May or may not be connected to earth ground. Distinct from signal ground.

<a id="clamp"></a>
**Clamp** — Protection component (typically a diode or TVS) that limits voltage excursions by conducting when a threshold is exceeded. Prevents overvoltage damage.

<a id="common-mode-rejection"></a>
**Common-mode rejection** — An instrument's or amplifier's ability to ignore voltage that appears equally on both inputs. Measured as CMRR in decibels.

<a id="composite-core"></a>
**Composite core** — Inductor core made from magnetic particles suspended in resin. Exhibits soft saturation — inductance decreases gradually with current rather than collapsing suddenly.

<a id="conventional-current"></a>
**Conventional current** — The standard engineering convention: current flows from positive to negative. Opposite to actual electron flow but universally used in circuit analysis and schematics.

<a id="copper-pour"></a>
**Copper pour** — Large area of copper on a PCB layer, typically connected to ground or power. Serves as a heatsink, improves return current paths, and reduces EMI.

<a id="coulomb"></a>
**Coulomb (C)** — SI unit of electric charge. One coulomb equals one ampere flowing for one second.

<a id="coupling-capacitor"></a>
**Coupling capacitor** — Capacitor in series with a signal path that passes AC while blocking DC. Sets the low-frequency cutoff of the coupled path.

<a id="curie-temperature"></a>
**Curie temperature** — Temperature above which a ferroelectric ceramic's dielectric properties degrade sharply. Relevant to Class II/III ceramic capacitors.

<a id="cut-trace"></a>
**Cut trace** — A PCB trace intentionally severed, usually as a rework modification. Should be documented on the schematic and in revision notes.

---

## D

<a id="dc-bias-effect"></a>
**DC bias effect** — Loss of capacitance in ceramic capacitors when DC voltage is applied. Class II ceramics (X5R, X7R) can lose 50% or more of their rated capacitance at rated voltage. Widely underappreciated.

<a id="dc-coupling"></a>
**DC coupling** — Signal path that passes all frequencies including DC. The default coupling mode on oscilloscopes.

<a id="dcr"></a>
**DCR (DC Resistance)** — The resistance of an inductor's winding measured at DC. Determines I²R power loss and heat generation.

<a id="dbm"></a>
**dBm** — Power measured in decibels referenced to 1 milliwatt. 0 dBm = 1 mW. Used extensively in RF and communication systems.

<a id="dbu"></a>
**dBu** — Voltage in decibels referenced to 0.775 V (the voltage that produces 1 mW into 600 Ω). Standard in professional audio.

<a id="dbv"></a>
**dBV** — Voltage in decibels referenced to 1 volt.

<a id="decoupling"></a>
**Decoupling** — Placing capacitors near IC power pins to supply instantaneous switching current and prevent noise from propagating through the power distribution network.

<a id="delta-wye"></a>
**Delta-wye (Δ-Y) transformation** — Mathematical technique for converting between triangle (delta) and star (wye) resistor network topologies. Necessary for analyzing circuits that aren't purely series or parallel.

<a id="dependent-source"></a>
**Dependent source** — A voltage or current source whose value is controlled by another voltage or current elsewhere in the circuit. Models transistors and op-amps in circuit analysis.

<a id="derating"></a>
**Derating** — Reducing a component's maximum allowable power, voltage, or current as operating conditions (typically temperature) move away from the rated conditions. Essential for reliable designs.

<a id="dielectric"></a>
**Dielectric** — Insulating material between capacitor plates that determines capacitance, voltage rating, stability, and loss characteristics.

<a id="dmm"></a>
**DMM (Digital Multimeter)** — The fundamental bench instrument for measuring voltage, current, and resistance. Quality varies enormously — input impedance, accuracy, and safety ratings all matter.

<a id="dot-convention"></a>
**Dot convention** — Dots placed on transformer winding symbols to indicate which terminals have the same instantaneous polarity. Essential for understanding phase relationships.

<a id="dynamic-resistance"></a>
**Dynamic resistance** — The slope of the I-V curve at a specific operating point (dV/dI). Describes the small-signal behavior of nonlinear devices like diodes and LEDs.

---

## E

<a id="earth-ground"></a>
**Earth ground** — Physical connection to the earth, typically through the building's grounding system. Provides a safety reference and a path for fault current.

<a id="eddy-current-loss"></a>
**Eddy current loss** — Power dissipated by circulating currents induced in a conductive core by the changing magnetic field. Increases with frequency. Reduced by using laminated or ferrite core materials.

<a id="electrolytic-capacitor"></a>
**Electrolytic capacitor** — Polarized capacitor using a liquid or polymer electrolyte to achieve high capacitance density. Limited lifetime — the electrolyte dries out over time, especially at elevated temperature.

<a id="electron-flow"></a>
**Electron flow** — The physical movement of electrons from negative to positive. Opposite to conventional current direction. Used in some educational contexts but not in standard engineering practice.

<a id="equivalent-resistance"></a>
**Equivalent resistance** — A single resistance value that replaces a series, parallel, or complex combination of resistors for analysis purposes.

<a id="esr"></a>
**ESR (Equivalent Series Resistance)** — The effective series resistance inside a capacitor, representing all resistive losses. Causes real power dissipation and limits the capacitor's ability to supply fast transients.

<a id="esl"></a>
**ESL (Equivalent Series Inductance)** — Parasitic inductance in a capacitor from its leads, internal connections, and plate geometry. Determines the self-resonant frequency.

---

## F

<a id="farad"></a>
**Farad (F)** — SI unit of capacitance. One farad stores one coulomb at one volt. Practical capacitors range from picofarads to millifarads.

<a id="ferrite-core"></a>
**Ferrite core** — Ceramic magnetic core material made from iron oxide compounds. Low eddy current loss at high frequencies but exhibits hard saturation — inductance drops abruptly when the current limit is exceeded.

<a id="film-capacitor"></a>
**Film capacitor** — Capacitor using plastic film as the dielectric. Excellent stability, low losses, self-healing under minor breakdown. Larger than ceramics for equivalent capacitance.

<a id="flat-schematic"></a>
**Flat schematic** — Multi-sheet schematic where all sheets exist at the same hierarchy level, connected by net names. Simpler than hierarchical schematics but harder to navigate in large designs.

<a id="flyback"></a>
**Flyback** — The voltage spike produced when current through an inductor is suddenly interrupted. The inductor's stored energy must go somewhere — without a defined path (flyback diode, snubber), it creates destructive voltage spikes.

<a id="flyback-diode"></a>
**Flyback diode** — Diode placed across an inductive load to provide a safe current path when the driving switch opens. Prevents voltage spikes from damaging the switch. Also called a freewheeling or snubber diode.

---

## G

<a id="global-net"></a>
**Global net** — A named electrical connection that spans multiple schematic sheets without explicit wiring. Power nets (VCC, GND) are the most common example.

<a id="ground-bounce"></a>
**Ground bounce** — Voltage spike on the ground network caused by fast-changing currents flowing through resistive and inductive ground paths. Can cause false logic transitions and measurement errors.

<a id="ground-plane"></a>
**Ground plane** — A continuous copper layer on a PCB dedicated to ground. Provides low-impedance return current paths, reduces EMI, and improves signal integrity.

---

## H

<a id="henry"></a>
**Henry (H)** — SI unit of inductance. One henry produces one volt when current changes at one ampere per second. Practical inductors range from nanohenries to millihenries.

<a id="hertz"></a>
**Hertz (Hz)** — SI unit of frequency. One hertz equals one cycle per second.

<a id="hidden-power-pin"></a>
**Hidden power pin** — IC power connection that exists in the component model but is not explicitly shown on the schematic symbol. Can cause confusion if the pin isn't properly connected in the netlist.

<a id="hierarchical-schematic"></a>
**Hierarchical schematic** — Schematic organized as a top-level block diagram with each block implemented as a separate sheet. Scales better than flat schematics for complex designs.

<a id="holdup-time"></a>
**Holdup time** — Duration that a capacitor can sustain a load after the input power is removed. Determined by capacitance, load current, and allowable voltage droop.

<a id="hysteresis-loss"></a>
**Hysteresis loss** — Energy dissipated in a magnetic core each AC cycle as magnetic domains reverse direction. Proportional to frequency and the area of the B-H loop.

---

## I

<a id="impedance"></a>
**Impedance (Z)** — Generalized resistance for AC circuits, including both resistive (real) and reactive (imaginary) components. Measured in ohms. Varies with frequency.

<a id="impedance-matching"></a>
**Impedance matching** — Selecting load impedance to maximize power transfer from a source. Critical in RF and audio systems. Maximum power transfer occurs when load impedance equals the complex conjugate of the source impedance.

<a id="inductance"></a>
**Inductance (L)** — The property of a component that resists changes in current by storing energy in a magnetic field. V = L × (dI/dt).

<a id="input-impedance"></a>
**Input impedance (Z_in)** — The impedance presented by a circuit or instrument at its input terminals. High input impedance means less loading of the signal source.

<a id="inrush-current"></a>
**Inrush current** — The large transient current drawn when power is first applied to a circuit, typically caused by charging initially discharged capacitors. Can be many times the steady-state current.

<a id="insulation-breakdown"></a>
**Insulation breakdown** — Degradation of a dielectric material leading to increased leakage current or catastrophic short circuit. Accelerated by temperature, voltage stress, and contamination.

<a id="instantaneous-power"></a>
**Instantaneous power** — The product of voltage and current at any specific moment: P(t) = V(t) × I(t).

<a id="ir-thermometer"></a>
**IR thermometer** — Non-contact temperature measurement tool that reads infrared radiation from a surface. Quick for spot checks but accuracy depends on the emissivity of the target surface.

---

## J

<a id="johnson-noise"></a>
**Johnson noise (thermal noise)** — Random voltage noise generated by thermal motion of electrons in any resistor. Proportional to resistance, temperature, and bandwidth. Present in all resistors regardless of construction.

<a id="joule"></a>
**Joule (J)** — SI unit of energy. One joule equals one watt for one second. One watt-hour equals 3,600 joules.

---

## K

<a id="kcl"></a>
**KCL (Kirchhoff's Current Law)** — The sum of currents entering a node equals the sum of currents leaving. A direct consequence of conservation of charge. See [Kirchhoff's Laws]({{< relref "/docs/fundamentals/laws-first-principles/kirchhoffs-laws" >}}).

<a id="kvl"></a>
**KVL (Kirchhoff's Voltage Law)** — The sum of voltage drops around any closed loop equals zero. A direct consequence of conservation of energy. See [Kirchhoff's Laws]({{< relref "/docs/fundamentals/laws-first-principles/kirchhoffs-laws" >}}).

---

## L

<a id="leakage-current"></a>
**Leakage current** — Small, undesired current flowing through a nominally insulating path. Present in capacitor dielectrics, semiconductor junctions, and PCB surface contamination.

<a id="linear-circuit"></a>
**Linear circuit** — A circuit that obeys the superposition principle: the response to a sum of inputs equals the sum of the individual responses. Resistors, capacitors, and inductors are linear; diodes and transistors are not.

<a id="loading"></a>
**Loading** — The effect of connecting a load to a circuit, which draws current and may change the operating voltage. Voltage dividers and high-impedance signal sources are particularly susceptible.

<a id="loop"></a>
**Loop** — Any closed path through a circuit, traversing one or more branches and returning to the starting node.

---

## M

<a id="magnetic-field"></a>
**Magnetic field** — The energy storage medium in inductors and transformers. Created by current flow through a conductor and concentrated by magnetic core materials.

<a id="magnetostriction"></a>
**Magnetostriction** — Physical deformation of a magnetic core material caused by the changing magnetic field. Produces audible buzzing or humming in inductors and transformers, especially at power-line frequencies.

<a id="mah"></a>
**Milliamp-hour (mAh)** — Unit of electric charge commonly used to rate battery capacity. To estimate energy in watt-hours: mAh × nominal voltage ÷ 1000.

<a id="maximum-power-transfer"></a>
**Maximum power transfer** — Occurs when load resistance equals the source's Thevenin resistance. Transfers maximum power to the load but at only 50% efficiency.

<a id="mesh-analysis"></a>
**Mesh analysis** — Systematic circuit analysis method that applies KVL around independent loops (meshes) and solves for loop currents. Complementary to nodal analysis.

<a id="metal-film-resistor"></a>
**Metal-film resistor** — Through-hole resistor type with low noise, tight tolerance, and good temperature stability. A solid general-purpose choice when precision matters.

<a id="microphonic"></a>
**Microphonic** — The property of some components (especially ceramic capacitors) to generate voltage when subjected to mechanical vibration, due to the piezoelectric effect.

<a id="mutual-inductance"></a>
**Mutual inductance** — Magnetic coupling between adjacent inductors or windings. The mechanism behind transformer operation and also a source of unwanted crosstalk.

---

## N

<a id="net-name"></a>
**Net name** — A label applied to a wire or connection in a schematic that replaces physical wire drawings. All points sharing the same net name are electrically connected.

<a id="no-connect"></a>
**No connect (NC)** — An explicit marker on a schematic indicating that an IC pin is intentionally left unconnected. Distinguishes deliberate non-connection from a wiring error.

<a id="node"></a>
**Node** — A junction where two or more branches meet in a circuit. All points connected by zero-resistance conductors form a single node.

<a id="nodal-analysis"></a>
**Nodal analysis** — Systematic circuit analysis method that applies KCL at each node and solves for node voltages. Generally preferred over mesh analysis for circuits with many parallel branches.

<a id="nominal-voltage"></a>
**Nominal voltage** — The approximate or stated voltage under no-load or standard conditions. Actual voltage varies with load and source regulation.

<a id="norton-equivalent"></a>
**Norton equivalent** — Any linear circuit reduced to a current source (I_n) in parallel with a resistance (R_n). Mathematically equivalent to the Thevenin equivalent. See [Thévenin & Norton]({{< relref "/docs/fundamentals/circuit-analysis/thevenin-norton" >}}).

<a id="ntc-thermistor"></a>
**NTC thermistor** — Negative Temperature Coefficient resistor whose resistance decreases with temperature. Used for temperature sensing and soft-start inrush current limiting.

---

## O

<a id="ohm"></a>
**Ohm (Ω)** — SI unit of electrical resistance. One ohm passes one ampere under one volt of potential difference.

<a id="ohms-law"></a>
**Ohm's Law (V = IR)** — The fundamental relationship between voltage, current, and resistance. Applies directly to resistive elements and as an approximation for many practical calculations. See [Ohm's Law]({{< relref "/docs/fundamentals/laws-first-principles/ohms-law" >}}).

<a id="open-circuit-voltage"></a>
**Open-circuit voltage (V_oc)** — The voltage measured across a source's terminals when no load is connected. Equal to the Thevenin voltage.

<a id="oscilloscope-probe"></a>
**Oscilloscope probe** — Coupling device between the circuit under test and the oscilloscope input. Passive probes (1× and 10×) are most common; active probes provide lower loading.

<a id="output-impedance"></a>
**Output impedance (Z_out)** — The impedance a circuit presents at its output terminals. Determines how much the output voltage droops under load. Lower output impedance means stiffer voltage regulation.

---

## P

<a id="parasitic-capacitance"></a>
**Parasitic capacitance** — Unintended capacitance present between conductors, component leads, PCB traces, and other structures. Negligible at low frequencies but dominates behavior at high frequencies.

<a id="parasitic-inductance"></a>
**Parasitic inductance** — Unintended inductance in component leads, wires, and PCB traces. Causes ringing on fast edges and limits high-frequency performance.

<a id="piezoelectric-effect"></a>
**Piezoelectric effect** — Generation of voltage from mechanical stress in certain materials. The mechanism behind microphonic noise in ceramic capacitors and the operating principle of crystal oscillators.

<a id="pin-name"></a>
**Pin name** — The functional label of an IC pin (e.g., CLK, RESET, VCC). Describes what the pin does.

<a id="pin-number"></a>
**Pin number** — The physical location identifier of an IC pin on the package. Needed for PCB layout and manual probing.

<a id="polarity"></a>
**Polarity** — The directional convention for voltage measurement. The terminal marked + is assumed to be at higher potential. Critical for electrolytic capacitors, which can be damaged by reverse polarity.

<a id="port"></a>
**Port** — A connection point in a hierarchical schematic that links a sub-sheet to the level above. Analogous to a function parameter in software.

<a id="potentiometer"></a>
**Potentiometer** — A three-terminal variable resistor with an adjustable wiper contact. Acts as an adjustable voltage divider.

<a id="powdered-iron-core"></a>
**Powdered iron core** — Inductor core made from iron particles with insulating gaps between them. Exhibits soft saturation and lower permeability than ferrite, useful for power inductors.

<a id="power-budget"></a>
**Power budget** — An accounting of all power sources, conversions, and dissipation in a system. Reveals whether the power supply can support the design and where heat will be generated.

<a id="power-derating"></a>
**Power derating** — Reduction in a component's maximum allowable power dissipation at temperatures above a specified threshold. Defined on datasheets as a derating curve or factor.

<a id="power-factor"></a>
**Power factor (PF)** — The ratio of real power to apparent power. Ranges from 0 to 1. A power factor of 1 (unity) means voltage and current are perfectly in phase.

<a id="power-flag"></a>
**Power flag / symbol** — Schematic symbols (VCC, VDD, 3V3, 5V, GND) representing global power connections. Connected by net name rather than by drawn wires.

<a id="power-rating"></a>
**Power rating** — The maximum continuous power a component can dissipate at a specified ambient temperature without exceeding its maximum operating temperature.

<a id="ppm"></a>
**ppm (Parts Per Million)** — A measure of ratio. 1% = 10,000 ppm. Used for specifying temperature coefficients (e.g., 25 ppm/°C) and precision tolerances.

<a id="precision"></a>
**Precision (Repeatability)** — The consistency of repeated measurements. A precise instrument gives the same reading each time, though that reading may not be accurate.

<a id="probe-loading"></a>
**Probe loading** — The distortion introduced by a measurement probe's input impedance (resistive and capacitive) on the circuit being measured. Most significant on high-impedance and high-frequency nodes.

<a id="ptc"></a>
**PTC (Positive Temperature Coefficient)** — A component whose resistance increases with temperature. Used as resettable fuses (polyfuses) and in self-regulating heater elements.

---

## Q

<a id="quiescent-current"></a>
**Quiescent current** — The current drawn by a circuit when no signal is applied and no load is driven. Represents the standing power consumption of the circuit.

---

## R

<a id="reactive-power"></a>
**Reactive power (Q)** — Power that cycles back and forth between source and reactive components (capacitors, inductors) without performing net work. Measured in VAR (volt-amperes reactive).

<a id="real-power"></a>
**Real power (P)** — The power actually dissipated as heat or converted to useful work, measured in watts. The only component of power that shows up on your electricity bill.

<a id="reference-designator"></a>
**Reference designator** — The unique identifier for each component on a schematic and PCB: R1, C2, U3, Q4, etc. The letter indicates the component type; the number distinguishes instances.

<a id="reference-direction"></a>
**Reference direction** — The assumed direction of current flow or voltage polarity assigned during circuit analysis. If the calculated value is negative, the actual direction is opposite to the assumed reference.

<a id="reference-node"></a>
**Reference node (ground)** — The node assigned zero volts in nodal analysis. All other node voltages are measured relative to this reference.

<a id="regulation"></a>
**Regulation** — How well a voltage source maintains its output under varying load conditions. Expressed as a percentage change from no-load to full-load.

<a id="resolution"></a>
**Resolution** — The smallest change in a measured quantity that an instrument can display. A 4.5-digit DMM has finer resolution than a 3.5-digit DMM, but resolution is not the same as accuracy.

<a id="rework"></a>
**Rework** — Manual modifications to a PCB after assembly: bodge wires, component swaps, cut traces. A normal part of prototype development but should be documented and incorporated into the next revision.

<a id="ripple-current"></a>
**Ripple current** — AC current superimposed on DC current flowing through a capacitor. Causes heating through ESR dissipation. Capacitor lifetime depends on keeping ripple current within ratings.

<a id="rms"></a>
**RMS (Root Mean Square)** — The effective value of a time-varying signal. The RMS voltage of an AC waveform produces the same heating in a resistor as a DC voltage of the same value.

---

## S

<a id="saturation-current"></a>
**Saturation current** — The current at which an inductor's core can no longer store additional magnetic energy, causing inductance to drop. Exceeding saturation current in a switching converter causes current to ramp uncontrollably.

<a id="self-resonant-frequency"></a>
**Self-resonant frequency (SRF)** — The frequency at which a capacitor's ESL resonates with its capacitance, causing impedance to reach a minimum. Above the SRF, the capacitor behaves as an inductor.

<a id="sense-resistor"></a>
**Sense resistor** — A low-value precision resistor placed in a current path so that the voltage drop across it indicates the current flowing. Also called a current shunt.

<a id="series"></a>
**Series** — Components connected end-to-end so that the same current flows through each. Voltages add; resistances add.

<a id="parallel"></a>
**Parallel** — Components connected across the same two nodes so that they share the same voltage. Currents add; conductances add.

<a id="shielded-inductor"></a>
**Shielded inductor** — Inductor with a magnetic structure that contains the magnetic field, reducing radiated interference and susceptibility to external fields.

<a id="short-circuit-current"></a>
**Short-circuit current (I_sc)** — The current that flows when a source's output terminals are connected together. Equal to the Norton current.

<a id="shunt-resistor"></a>
**Shunt resistor** — Low-value resistor used inside ammeters and current-sense circuits to convert current into a measurable voltage. Same as sense resistor.

<a id="si-prefixes"></a>
**SI prefixes** — Standard multipliers for units: tera (10¹²), giga (10⁹), mega (10⁶), kilo (10³), milli (10⁻³), micro (10⁻⁶), nano (10⁻⁹), pico (10⁻¹²), femto (10⁻¹⁵).

<a id="signal-ground"></a>
**Signal ground** — The reference point for signal voltages in a circuit. May or may not be connected to earth ground or chassis ground.

<a id="smd-resistor-marking"></a>
**SMD resistor marking** — Three or four digit code: for a 3-digit code, the first two digits are significant and the third is the exponent (e.g., "472" = 47 × 10² = 4,700 Ω).

<a id="snubber"></a>
**Snubber** — An RC or RCD network that absorbs energy from switching transients, reducing voltage spikes and ringing. Placed across switches, relays, and inductive loads.

<a id="source-deactivation"></a>
**Source deactivation** — The process of "turning off" independent sources to find equivalent resistance: voltage sources become short circuits; current sources become open circuits.

<a id="stiff-source"></a>
**Stiff source** — A voltage source with low output impedance, meaning its voltage barely changes when load current varies. The opposite of a high-impedance source.

<a id="supercapacitor"></a>
**Supercapacitor** — Very high-capacitance component (farads to hundreds of farads) used for energy storage and extended holdup time. Much lower energy density than batteries but can charge and discharge rapidly.

<a id="supernode"></a>
**Supernode** — In nodal analysis, a boundary drawn around a voltage source and its two adjacent nodes, allowing KCL to be applied to the combined region.

<a id="supermesh"></a>
**Supermesh** — In mesh analysis, a combined loop formed when a current source appears in a branch shared by two meshes. Allows KVL to be applied to the combined path.

<a id="superposition"></a>
**Superposition** — The principle that in a linear circuit, the total response equals the sum of the responses due to each independent source acting alone. Valid only for linear circuits.

---

## T

<a id="tantalum-capacitor"></a>
**Tantalum capacitor** — Polarized capacitor with high volumetric efficiency. Failure mode can be a low-impedance short circuit, potentially violent. Derate voltage significantly or use polymer tantalum types.

<a id="tank-circuit"></a>
**Tank circuit** — A resonant LC circuit that stores energy by oscillating between the electric field of the capacitor and the magnetic field of the inductor. Used in RF tuning and impedance matching.

<a id="temperature-coefficient"></a>
**Temperature coefficient (tempco)** — The change in a component's value per degree Celsius, usually expressed in ppm/°C. A 100 ppm/°C resistor changes 0.01% per degree.

<a id="thermal-camera"></a>
**Thermal camera** — Imaging device that maps surface temperatures across a PCB or system. Reveals hot spots, thermal gradients, and components operating near their limits.

<a id="thermal-coupling"></a>
**Thermal coupling** — Heat transfer between physically adjacent components. Can cause unintended parameter shifts when one component's dissipation heats a neighbor.

<a id="thermal-paste"></a>
**Thermal paste** — Interface material applied between a component and heatsink to fill microscopic air gaps and reduce thermal resistance.

<a id="thermal-resistance"></a>
**Thermal resistance (θ)** — Resistance to heat flow, analogous to electrical resistance. Temperature rise equals power times thermal resistance: ΔT = P × θ. Measured in °C/W.

<a id="thermal-runaway"></a>
**Thermal runaway** — A positive feedback loop where increased temperature causes increased current (or power), which further increases temperature. Destructive if not interrupted.

<a id="thevenin-equivalent"></a>
**Thévenin equivalent** — Any linear circuit reduced to a voltage source (V_th) in series with a resistance (R_th). Simplifies analysis of how the circuit interacts with different loads. See [Thévenin & Norton]({{< relref "/docs/fundamentals/circuit-analysis/thevenin-norton" >}}).

<a id="thick-film-resistor"></a>
**Thick-film resistor** — The most common SMD resistor type. Moderate tolerance (1–5%), moderate tempco. Adequate for most applications; specify thin-film when precision is needed.

<a id="thin-film-resistor"></a>
**Thin-film resistor** — Precision SMD resistor with tight tolerance (0.1% or better), excellent tempco, and low noise. Costs more than thick-film. Use for instrumentation, precision voltage references, and sensor signal conditioning.

<a id="three-digit-cap-code"></a>
**Three-digit capacitor code** — Marking system where the first two digits are significant and the third is a power-of-ten multiplier in picofarads. E.g., "104" = 10 × 10⁴ pF = 100 nF.

<a id="time-constant"></a>
**Time constant (τ)** — The characteristic response time of an RC or RL circuit. τ = RC for capacitive circuits; τ = L/R for inductive circuits. After one time constant, the circuit reaches 63% of its final value.

<a id="tolerance"></a>
**Tolerance** — The allowable deviation of a component's actual value from its nominal value, expressed as a percentage. A 10 kΩ ±1% resistor can range from 9,900 Ω to 10,100 Ω.

<a id="tolerance-stacking"></a>
**Tolerance stacking** — The accumulated uncertainty when multiple components' tolerances combine in a circuit. Worst-case analysis adds tolerances; statistical analysis uses root-sum-square.

<a id="transient"></a>
**Transient** — A brief, fast-changing voltage or current event. Switching events, power-on surges, and ESD are common transient sources.

<a id="trigger"></a>
**Trigger** — Oscilloscope feature that synchronizes the display to a specific event on the waveform, producing a stable, readable display.

---

## U

<a id="unity-power-factor"></a>
**Unity power factor** — Power factor of 1, meaning voltage and current are perfectly in phase. All power delivered is real power; no reactive power is present.

<a id="unshielded-inductor"></a>
**Unshielded inductor** — Inductor with an open magnetic path that allows the magnetic field to radiate. Can couple interference to nearby circuits and is susceptible to external fields.

---

## V

<a id="volt"></a>
**Volt (V)** — SI unit of electric potential difference. One volt drives one ampere through one ohm of resistance.

<a id="voltage-coefficient"></a>
**Voltage coefficient** — The change in a component's value with applied voltage. Most significant in ceramic capacitors (DC bias effect) and carbon composition resistors.

<a id="voltage-divider"></a>
**Voltage divider** — A network (typically two resistors in series) that produces an output voltage proportional to the input voltage. Output depends on the ratio of the resistances and is affected by load current. See [Voltage Dividers & Loading]({{< relref "/docs/fundamentals/circuit-analysis/voltage-dividers-and-loading" >}}).

---

## W

<a id="watt"></a>
**Watt (W)** — SI unit of power. One watt equals one joule per second. One watt equals one volt times one ampere in a DC circuit.

<a id="watt-hour"></a>
**Watt-hour (Wh)** — Unit of energy. One watt-hour equals 3,600 joules. Used to rate battery capacity alongside milliamp-hours.

<a id="wiper"></a>
**Wiper** — The moving contact in a potentiometer that slides along the resistive element, creating a variable voltage divider.

<a id="wirewound-resistor"></a>
**Wirewound resistor** — Precision resistor made by winding resistance wire on a form. Excellent tolerance and tempco but significant parasitic inductance. Not suitable for high-frequency circuits.

---

## X

<a id="x5r"></a>
**X5R** — Class II ceramic dielectric rated for -55°C to +85°C with ±15% capacitance variation over temperature. Common in decoupling applications but susceptible to DC bias and aging effects.

<a id="x7r"></a>
**X7R** — Class II ceramic dielectric rated for -55°C to +125°C with ±15% capacitance variation over temperature. The most common general-purpose ceramic for applications where C0G/NP0 values are insufficient.

---

## Y

<a id="y5v"></a>
**Y5V** — Class III ceramic dielectric with very high capacitance density but terrible stability: +22% / -82% over temperature range. Avoid for anything where the actual capacitance matters.
