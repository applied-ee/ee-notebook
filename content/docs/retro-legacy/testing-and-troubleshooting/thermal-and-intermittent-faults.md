---
title: "Thermal & Intermittent Faults"
weight: 40
review:
  status: unreviewed
  method: []
  notes: ""
  date: ""
---

# Thermal & Intermittent Faults

Faults that appear or disappear with temperature are common in legacy equipment and notoriously difficult to isolate because the board works fine on the bench until it warms up (or cools down).

## Temperature-Dependent Failures

- **Cold spray (freeze spray, component cooler):** Apply to individual components or small board areas to cool them rapidly. If the fault appears or disappears when a specific component is cooled, that component is temperature-sensitive. Work from the most suspect components outward — electrolytic caps, semiconductors in thermal contact with heatsinks, and solder joints near heat sources
- **Heat gun (or hot air rework tool on low setting):** Gently warm a board area to reproduce a fault that only appears after the equipment warms up. Do not overheat — the goal is to bring the component to its failing temperature, not to desolder it

## Intermittent Connections

- **Tap test:** While the circuit is powered and monitored (on a scope, with a meter, or by observing the output), gently tap the board, components, and connectors with an insulated tool. A fault that appears on tap indicates a cracked solder joint, fractured trace, or intermittent connector contact in the area being tapped
- **Flex test:** Gently flex the board while monitoring. A crack in a trace or solder joint that is invisible under magnification may open when the board flexes

## Monitoring for Intermittent Dropout

- Set the oscilloscope to trigger on an abnormal event (voltage dropout, spike, or edge) and use single-shot or persistence mode to capture the moment the fault occurs
- A chart recorder or data logger on a critical voltage rail can capture slow intermittents that occur over hours — this is impractical with a scope but straightforward with a multimeter in logging mode

## Caveats

- **Cold spray can cause condensation** — rapid cooling below the dew point creates water droplets on the board, which can cause shorts between closely spaced traces. Allow the board to return to room temperature and dry before drawing conclusions from a cold-spray test on a powered board

## In Practice

- **Intermittent audio crackling that follows tapping on the board** traces to a cracked solder joint — the most likely locations are high-thermal-stress components (power transistors, regulators, large resistors) and connector pins
- **A board that works when cold but fails after 15-20 minutes of operation** has a thermally marginal component — systematic application of freeze spray to individual components while monitoring the output will isolate which device is temperature-sensitive
