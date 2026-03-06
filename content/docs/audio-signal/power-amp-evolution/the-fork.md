---
title: "The Fork"
weight: 50
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# The Fork

By the late 20th century, the Class AB linear amplifier was mature. The multi-stage architecture worked. Distortion was vanishingly low. Feedback tamed every nonlinearity. The remaining problem — efficiency — was fundamental to the approach.

Two paths diverged:

## Path 1: Refine the Linear Amplifier

Accept the efficiency limitation and push everything else to the extreme. Better bias circuits. Better matched devices. Cascoded stages for higher gain. Current mirrors for better linearity. MOSFET output devices for easier thermal management. Error-correction circuits that supplement feedback.

This path leads to audiophile-grade amplifiers with THD measured in parts per million, ruler-flat frequency response, and specifications that exceed human hearing by large margins. The penalty is heat, weight, and cost — but for applications where absolute signal fidelity matters, this path has been extraordinarily successful.

## Path 2: Abandon Linear Output

Ask the radical question: what if the output transistors are never in their linear region? What if they're always either fully on (saturated) or fully off (cutoff)?

A transistor that's fully on has very low voltage across it (V_CE(sat) ≈ 0.1-0.3V). A transistor that's fully off has no current through it. In both states, the power dissipated by the transistor is nearly zero. The only time a switching transistor dissipates significant power is during the brief transition between states.

If you can encode the audio signal as a pattern of on/off switching — and then recover the audio from that pattern — you get an amplifier with theoretical efficiency approaching 100%.

This is the idea behind Class D: encode audio as pulse-width modulation, switch the output, and filter to recover the signal.

## The Tradeoff

| | Linear (Class AB) | Switching (Class D) |
|---|---|---|
| Efficiency | 50-65% | 85-95% |
| Distortion | Extremely low | Low (improving rapidly) |
| Idle power | Moderate | Very low |
| Switching noise | None | Requires filtering |
| EMI | Low | Can be significant |
| Complexity | Analog design | Mixed analog/digital |

Neither path is universally better. The choice depends on what you're optimizing for.

The next two pages explore each path: [Refining Linear Amplifiers]({{< relref "refining-linear-amplifiers" >}}) and [Class D Switching]({{< relref "class-d-switching" >}}).
