---
title: "Abstraction Leakage & Failure Propagation"
weight: 40
bookCollapseSection: true
---

# Abstraction Leakage & Failure Propagation

## What This Section Covers

- **[How Internal Behavior Leaks Out of ICs]({{< relref "how-internal-behavior-leaks" >}})** — The mechanisms by which behavior that should stay inside a package becomes visible and consequential outside it.

- **[Upward Failure Propagation]({{< relref "upward-failure-propagation" >}})** — How failures at lower abstraction levels surface as symptoms at higher levels, and why the symptom rarely points directly to the cause.

- **[Downward Failure Propagation]({{< relref "downward-failure-propagation" >}})** — How problems at higher levels — wrong configurations, violated operating conditions, bad system-level decisions — damage or stress lower-level components.

- **[Misdiagnosis from Reasoning at the Wrong Layer]({{< relref "misdiagnosis-wrong-layer" >}})** — How choosing the wrong abstraction level leads to plausible but incorrect explanations, and how to recognize when it's happening.
