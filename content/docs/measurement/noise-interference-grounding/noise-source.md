---
title: "Where Is This Noise Coming From?"
weight: 10
---

# Where Is This Noise Coming From?

The circuit should be quiet but isn't. There's something on the signal that shouldn't be there — a hum, a buzz, a whine, broadband hash, or periodic spikes. Before you can fix noise, you have to identify what's generating it and how it's getting in. This is detective work: observe, hypothesize, test.

## Oscilloscope: Characterizing the Noise

**Tool:** Oscilloscope, AC-coupled (to see just the noise), 10x probe
**When:** First look at unwanted signal content — what does the noise look like?

### Procedure

1. AC-couple the channel to remove the DC component and focus on the noise
2. Set the bandwidth limit to 20 MHz to reduce probe-artifact noise (unless you suspect high-frequency content)
3. Set vertical scale to zoom in on the noise — start at 10 mV/div and adjust
4. Set timebase based on what you suspect:
   - 5 ms/div: mains hum (50/60 Hz)
   - 1 µs/div: switching converter noise (100 kHz–2 MHz)
   - 100 ns/div: digital clock crosstalk (MHz range)
5. Look at the waveform shape and frequency — this is your first clue to the source

### Identifying by Waveform Character

| What you see | Likely source |
|-------------|--------------|
| Smooth sine wave at 50/60 Hz | Mains magnetic coupling (transformer, ground loop) |
| Sine at 100/120 Hz (double mains frequency) | Full-wave rectifier ripple — power supply issue |
| Periodic spikes or sawtooth at 100 kHz–2 MHz | Switching converter noise |
| Bursts of ringing at irregular intervals | Digital bus activity coupling in |
| Broadband hash, no clear frequency | RF interference, oscillation, or thermal noise |
| Signal that correlates with motor speed or load | Motor/drive interference (commutation, PWM) |

### What You Learn

- The character and frequency of the noise — essential for identifying the source
- Whether the noise is periodic (easier to track) or random (harder to track)

### Gotchas

- The scope probe itself picks up noise. Before blaming the circuit, disconnect the probe tip and observe the noise floor with just the ground clip connected. If you still see noise, it's pickup into the probe, not from the circuit
- Long ground clips act as antennas. Use the spring-tip ground for clean noise measurements

## Oscilloscope FFT: Identifying Noise Frequencies

**Tool:** Oscilloscope FFT
**When:** Identifying specific frequency components in the noise

### Procedure

1. Capture the noisy signal
2. Run FFT with a Hann window
3. Look for peaks — each peak is a clue:
   - 50/60 Hz and harmonics → mains-related
   - Frequency matching a switching converter → conducted or radiated switching noise
   - Frequency matching a clock oscillator → digital crosstalk
   - Harmonics of a known signal → distortion from nonlinearity in the signal path
4. Compare the FFT of the noisy node to the FFT of the suspected source — if the same frequencies appear, you've found the coupling path

### What You Learn

- Precise frequency identification of noise components
- Whether multiple sources are contributing (multiple unrelated peaks)

### Gotchas

- The FFT shows frequency content, not the coupling mechanism. A peak at the switching frequency tells you the switcher is the source, but not whether the noise is conducted through the power rail, radiated through the air, or coupled through a shared ground

## Near-Field Probes: Localizing EMI Sources

**Tool:** Near-field probe (H-field loop or E-field stub) + oscilloscope or spectrum analyzer
**When:** Finding which component or trace on the board is radiating the noise

### How They Work

- **H-field probe (magnetic loop):** Sensitive to current loops — switching transistors, inductors, traces carrying pulsed current
- **E-field probe (short stub):** Sensitive to voltage nodes — high-dV/dt switching nodes, clock traces, unshielded high-voltage nodes

### Procedure

1. Connect the near-field probe to a scope input (50 Ohm termination if the probe is designed for it)
2. Move the probe slowly across the board while watching the scope
3. The signal amplitude increases as you approach the source
4. Rotate the H-field probe — it's directional (maximum pickup when the loop is perpendicular to the current flow)
5. Mark the hotspots — these are the components or traces generating the most emission

### What You Learn

- Which specific component or area of the board is the primary noise source
- The spatial distribution of emissions — useful for deciding where to add shielding or filtering

### Gotchas

- Near-field measurements are qualitative — relative "more here, less there," not calibrated dBm. Use them for localization, not for compliance measurement
- At very close range, both H and E probes respond to their non-primary field type. The spatial resolution is roughly equal to the probe's physical size
- Commercial near-field probe sets exist (Beehive, Langer), or you can make a simple H-field probe from a loop of coax with the shield gap at the tip

## Process of Elimination

**Tool:** Your hands (literally — turning things off, disconnecting, shielding)
**When:** You've characterized the noise but aren't sure of the coupling path

### Procedure

1. **Turn off suspect sources one at a time.** If the noise disappears when you power down the switching converter, the converter is the source. If the noise persists with the converter off, look elsewhere
2. **Disconnect cables one at a time.** If the noise disappears when you unplug a USB cable, the cable is carrying or radiating the noise
3. **Move the DUT away from other equipment.** If noise reduces with distance, it's radiated. If distance doesn't help, it's conducted (through shared power or ground)
4. **Shield with your hand or a piece of copper foil.** Place a grounded copper sheet between the suspected source and victim. If noise drops, radiated coupling is confirmed
5. **Add a ferrite clamp to a cable.** If noise drops, common-mode conducted noise on that cable is the path

### What You Learn

- The specific source and coupling path — which is essential for choosing the right fix
- Whether the problem is conducted, radiated, or both

### Gotchas

- Turning things off changes the circuit's operating conditions. A switching converter that's off isn't producing noise, but it's also not providing power — the circuit may behave differently. If possible, substitute a linear supply to maintain power while eliminating switching noise
- Multiple coupling paths can exist simultaneously. Fixing one path may reveal another that was previously masked
