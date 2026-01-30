---
title: "What's the Actual Uncertainty?"
weight: 40
---

# What's the Actual Uncertainty?

Understanding measurement uncertainty beyond just "accuracy." Every reading has an uncertainty band — a range within which the true value lies. Resolution, accuracy, repeatability, environmental effects, and the measurement method itself all contribute. Knowing how many digits of your reading are real prevents false confidence and unnecessary worry.

## Resolution vs. Accuracy vs. Precision

**Concept:** Three different things that people often confuse

| Term | What it means | Analogy |
|------|--------------|---------|
| Resolution | Smallest change the instrument can display | Tick marks on a ruler |
| Accuracy | How close the reading is to the true value | Whether the ruler's markings are correct |
| Precision (repeatability) | How much the reading varies when measuring the same thing repeatedly | Whether the ruler gives the same reading each time |

### A DMM Example

A 3½-digit DMM on the 4V range:
- **Resolution:** 1 mV (0.001V) — the last digit
- **Accuracy:** ±(0.5% + 3 counts) = ±23 mV on a 4V reading
- **Precision:** Might repeat within ±2 mV reading to reading

The display shows 4.000V but the true voltage could be anywhere from 3.977V to 4.023V. The three decimal places look precise, but only the first two are reliable.

## Calculating Measurement Uncertainty

**Concept:** Combining multiple error sources

### DMM Voltage Measurement Uncertainty

1. **Reading accuracy:** From the spec sheet, ±(% of reading + counts)
2. **Temperature coefficient:** If outside 23°C ±5°C, add the temp coeff (typically ±0.1× the accuracy spec per °C outside the range)
3. **Lead resistance:** For low-resistance measurements, lead resistance adds systematic error
4. **Loading effect:** DMM input impedance creates a voltage divider with the source impedance

**Total uncertainty = sqrt(error1² + error2² + ...)** (for independent, random errors)

Or simply add them for worst-case: **Total = |error1| + |error2| + ...** (conservative)

### Oscilloscope Amplitude Uncertainty

1. **Vertical accuracy:** Typically ±3–5% of the reading
2. **Probe attenuation accuracy:** Typically ±1–3% for a compensated 10x probe
3. **DC offset accuracy:** ±(vertical accuracy × offset setting)
4. **ADC quantization:** LSB/2 = (full scale) / (2^bits × 2)

For a scope reading of 3.3V with 3% vertical accuracy and 2% probe accuracy:
- Vertical: ±0.099V
- Probe: ±0.066V
- Combined (RSS): ±0.119V
- The reading is 3.3V ±0.12V — or about ±3.6%

### Gotchas

- Systematic errors (like a consistently miscalibrated meter) don't reduce with averaging. Only random errors reduce with averaging
- The uncertainty of a derived quantity (like power = V × I) combines the uncertainties of the individual measurements. For multiplication/division: **%uncertainty_result = sqrt(%uncertainty_V² + %uncertainty_I²)**

## How Many Digits Are Real?

**Concept:** Significant figures in the context of instrument specs

### The Rule

Only report digits that are within the instrument's uncertainty. If the reading is 3.456V and the uncertainty is ±0.023V, the true value is between 3.433V and 3.479V. The meaningful report is 3.46V ±0.02V — the third decimal place is noise.

### Quick Guide by Instrument

| Instrument | Typically meaningful digits | Don't trust |
|-----------|---------------------------|-------------|
| 3½-digit DMM (DC Volts) | 3 digits | The last digit fluctuates; last 2 digits are within accuracy tolerance |
| 4½-digit DMM (DC Volts) | 4 digits | Last digit is noise |
| Oscilloscope cursor reading | 2–3 digits | Anything beyond 3% of the reading |
| Scope auto-measurement | 2–3 digits | Extra digits from averaging look precise but aren't accurate |
| Component tester / LCR meter | 2–3 digits (depends on model) | Check spec for the specific component type and range |

### Gotchas

- Digital displays create false precision. A DMM showing "3.3021V" on a 3½-digit meter doesn't mean the voltage is known to 0.1 mV — the display has more resolution than the measurement has accuracy
- Averaging increases precision (reduces random variation) but not accuracy (doesn't fix systematic errors). A meter that's consistently 1% high will still be 1% high after averaging a thousand readings

## Repeatability and Environmental Effects

### Checking Repeatability

1. Measure the same quantity 10 times without changing anything
2. Calculate the standard deviation
3. This is the repeatability — the random component of uncertainty
4. If the standard deviation is larger than the instrument's resolution, external factors are contributing (temperature drift, noise, contact resistance)

### Environmental Factors

| Factor | Effect | How to manage |
|--------|--------|---------------|
| Temperature | Most instrument specs assume 23°C ±5°C. Outside this, accuracy degrades | Work at room temperature, let instruments warm up |
| Humidity | Affects high-resistance measurements (leakage paths) | Be aware in humid conditions, especially for > 1 MOhm |
| Vibration | Affects contact resistance, can create intermittent connections | Use stable probe connections, fixtures |
| EMI | External interference adds to the reading | Shield sensitive measurements, use twisted leads |
| Warm-up time | Instruments drift during warm-up | Allow 30+ minutes for precision measurements |

### Gotchas

- Temperature is the biggest environmental factor for most measurements. A DMM's accuracy spec at 23°C might be ±0.05%; at 40°C it might be ±0.1%. If you're working in a hot garage, your readings are less accurate than the headline spec
- Thermoelectric voltages (Seebeck effect) at dissimilar metal junctions create microvolt-level offsets. For precision DC measurements (calibrating a reference), this matters. For everyday bench work, it doesn't

## Practical Uncertainty for Bench Work

For most hobby and learning bench work, formal uncertainty analysis is overkill. But these rules of thumb help:

1. **DMM DC voltage:** Trust to about ±1% on a name-brand meter. If two readings differ by less than 1%, they're effectively the same
2. **DMM resistance:** Trust to about ±1% above 10 Ohm. Below 10 Ohm, lead resistance matters. Below 1 Ohm, use 4-wire or don't trust it
3. **Scope amplitude:** Trust to about ±5%. Use the scope for waveshape, the DMM for voltage
4. **Scope timing:** Trust to about ±0.01% (50 ppm timebase). Timing is the scope's best measurement
5. **Component values:** A 5% resistor measuring 4.8% off is within spec. Don't declare it faulty
6. **Frequency:** If your scope says 1.000 MHz and a counter says 1.001 MHz, they probably agree — the difference is within typical timebase accuracy
