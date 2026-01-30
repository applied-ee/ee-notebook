---
title: "Is There Jitter, and Where Is It From?"
weight: 50
---

# Is There Jitter, and Where Is It From?

Timing variation on clocks and data signals. A perfect clock has edges at exactly equal intervals — real clocks don't. Jitter is the deviation of edge timing from the ideal, and it comes from noise, power supply variation, crosstalk, and the oscillator itself. Small jitter is normal; excessive jitter causes bit errors, sampling problems, and failed timing margins.

## Understanding Jitter Types

### Period Jitter

The variation in the period of consecutive cycles. If the ideal period is 10 ns and measured periods vary from 9.8 ns to 10.2 ns, the period jitter is 0.4 ns peak-to-peak (or ~0.1 ns RMS, depending on distribution).

### Cycle-to-Cycle Jitter

The change in period from one cycle to the next. This captures high-frequency jitter — the kind that changes from edge to edge rather than drifting slowly. Most timing specs for clocks (PLLs, clock buffers) specify cycle-to-cycle jitter.

### Long-Term (Accumulated) Jitter

How much timing error accumulates over many cycles. If each cycle varies independently, accumulated jitter grows as sqrt(N) for random jitter. This matters for serial data links where the receiver's clock recovery must track the data over many bit periods.

### Random vs. Deterministic Jitter

| Type | Source | Behavior | Reducible? |
|------|--------|----------|------------|
| Random (RJ) | Thermal noise, shot noise | Gaussian distribution, unbounded tails | No — fundamental noise floor |
| Deterministic (DJ) | Power supply noise, crosstalk, ISI, duty cycle distortion | Bounded, often periodic or pattern-dependent | Yes — fix the root cause |

Total jitter = deterministic jitter + N × random jitter RMS (where N depends on the BER target).

## Oscilloscope: Jitter Measurement

**Tool:** Oscilloscope with persistence and statistical measurements
**When:** Measuring jitter on clocks, data signals, or any periodic edge

### Persistence (Infinite Persist) Method

1. Trigger on the rising edge of the clock
2. Enable infinite persistence
3. Let the display accumulate — edges spread into a "band" showing the jitter distribution
4. The width of the band at the threshold crossing is the peak-to-peak jitter
5. Where the band is thickest indicates the most probable edge position; the thin tails show rare excursions

### Statistical Measurement Method

1. Set up the scope to measure period (or frequency) on the signal
2. Enable statistics (mean, standard deviation, min, max)
3. Let the scope accumulate thousands of measurements
4. Period jitter (peak-to-peak) = max - min
5. Period jitter (RMS) ≈ standard deviation of the period measurement
6. Cycle-to-cycle jitter requires measuring the difference between consecutive periods — some scopes have this as a built-in measurement; others need a math function (Period_N - Period_N-1)

### What You Learn

- Peak-to-peak jitter (worst case over the measurement window)
- RMS jitter (the standard deviation — more meaningful for random jitter)
- Whether jitter is random (Gaussian distribution on histogram) or deterministic (bimodal or periodic)

### Gotchas

- The scope's own trigger jitter adds to the measurement. Most modern scopes have trigger jitter of ~1–5 ps RMS, which is negligible for ns-scale jitter but matters at the ps level. For sub-100 ps jitter measurements, you need a scope specified for low trigger jitter
- Measuring jitter by triggering on one edge and observing subsequent edges accumulates jitter. The Nth edge after the trigger shows N cycles' worth of accumulated jitter. Trigger on the immediately preceding edge to see cycle-to-cycle jitter
- Insufficient sample rate broadens the apparent edge and inflates the jitter measurement. The scope's time resolution is approximately 1/(sample rate), so a 1 GS/s scope has ~1 ns time resolution — it can't meaningfully measure jitter below a few ns
- Peak-to-peak jitter keeps growing as you accumulate more measurements (because random jitter is unbounded). RMS jitter converges. Always report the number of measurements when quoting peak-to-peak jitter

## Identifying Jitter Sources

### Power Supply Noise

- Jitter is correlated with power supply ripple — the oscillator's frequency varies with supply voltage
- Diagnosis: trigger the scope on the clock edge, display the power supply on a second channel. If supply noise and jitter are correlated in time, supply noise is the source
- Fix: improve decoupling, use a low-noise regulator, add power supply filtering for the oscillator

### Crosstalk

- Jitter appears when a nearby signal switches — the coupling shifts the clock edge timing
- Diagnosis: trigger on the interfering signal and observe the clock. The clock edges near the interfering edge shift; those far away don't
- Fix: increase spacing between traces, improve shielding, add ground between aggressor and victim

### Oscillator Phase Noise

- Intrinsic jitter from the oscillator itself — thermal noise in the crystal or LC resonator
- This is random and irreducible for a given oscillator type
- A crystal oscillator has much lower phase noise (less jitter) than an RC oscillator. An OCXO has less than a standard crystal
- If all other sources are eliminated and jitter remains, it's the oscillator's intrinsic noise

### Substrate/Ground Bounce

- In digital ICs, switching activity on other outputs causes supply and ground bounce, which modulates internal delays
- Jitter increases with bus activity — worst case when many outputs switch simultaneously
- Diagnosis: jitter correlates with bus activity patterns

### Gotchas

- Multiple jitter sources can be present simultaneously. Deterministic jitter (from power supply, crosstalk) adds linearly. Random jitter adds in RSS (root-sum-square). Separating them requires jitter decomposition, which higher-end scopes and dedicated jitter analysis tools can do
- Temperature changes cause frequency drift, which looks like very low-frequency jitter on short measurements. This is wander, not jitter (by convention, jitter is variation above 10 Hz; wander is below 10 Hz)
- A PLL cleans up input jitter within its loop bandwidth but adds its own jitter (VCO phase noise) outside its loop bandwidth. A PLL with high loop bandwidth tracks the input closely (including its jitter); low loop bandwidth filters input jitter but shows VCO jitter at higher offsets
