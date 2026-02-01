---
title: "Safety & High Energy"
weight: 10
bookCollapseSection: true
---

# Safety & High Energy

Mains, high voltage, CAT ratings, isolation, and the habits that keep you alive. This comes first because no measurement matters if you get hurt taking it.

## What This Section Covers

Everything here is about assessing hazard *before* you connect anything — not after you see sparks.

Measurement intent — the questions you're trying to answer at the bench:

- **[Can I touch/probe this safely?]({{< relref "can-i-probe-safely" >}})** — Assessing hazard before connecting anything. Is it de-energized? Are stored charges drained? What's the worst case?
- **[What voltage/energy am I dealing with?]({{< relref "voltage-energy-domain" >}})** — Identifying the energy domain: low-voltage DC, mains AC, high-voltage DC (tube amps, CRTs, switchmode rails), stored energy in capacitors or inductors.
- **[Do I need isolation or differential measurement?]({{< relref "isolation-differential" >}})** — When a floating measurement will fool you or kill you. Isolation transformers, differential probes, and when ground-referenced instruments are dangerous.
- **[Is my instrument rated for this?]({{< relref "instrument-ratings" >}})** — CAT ratings, voltage limits, probe ratings, and the difference between a meter that handles a fault and one that becomes shrapnel.
- **[ESD & Transient Protection Basics]({{< relref "esd-transient-protection" >}})** — What ESD and electrical transients actually do to ICs, how to handle boards without killing them, and how to design protection into circuits.
- **[Automotive Electrical Reality (12V Is Not 12V)]({{< relref "automotive-power-domain" >}})** — What "12V" actually means in a vehicle: voltage swings, cranking brownouts, load dump, reverse polarity, chassis ground behavior, and how to protect circuits that live in cars.
