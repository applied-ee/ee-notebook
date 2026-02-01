---
title: "🛠️ Design & Development"
weight: 8
bookCollapseSection: true
---

# Design & Development

From concept to working hardware.

Design and development covers the process of turning ideas into real, working systems — from early concept and requirements, through schematic and layout, into part sourcing, prototyping, validation, and iteration. This section captures the decisions, tradeoffs, and workflows that sit between theory and reality.

Design does not happen in isolation. It is shaped by component availability, physical constraints, manufacturability, testability, and long-term maintenance. Choices made early echo forward into debugging, repair, and production. The focus here is practical design thinking — not just how to build circuits, but how to build systems that survive contact with the real world.

## Sections

- **[Ideation & Requirements]({{< relref "ideation-and-requirements" >}})** — Deciding what you're actually building: problem definition, constraints, and knowing when not to build.
  *What this section covers:* Problem Definition vs Solution Bias · Functional vs Non-Functional Requirements · Constraints · Component Dependencies & Datasheet-Driven Requirements · Good-Enough Criteria · Knowing When Not to Build · When Software Beats Hardware

- **[Proof of Concept]({{< relref "proof-of-concept" >}})** — Testing the hard parts before committing: breadboarding, simulation, dev boards, and structured experiments that answer the riskiest questions.
  *What this section covers:* Breadboarding Strategies · Simulation as Proof of Concept · Dead Bug & Manhattan Construction · Dev Boards & Modules · Structuring Experiments · Knowing When the PoC Is Done

- **[System Architecture]({{< relref "system-architecture" >}})** — Decomposing complexity: block diagrams, partitioning, interfaces, and avoiding accidental complexity.
  *What this section covers:* Block Diagrams as Thinking Tools · Partitioning · Interfaces & Boundaries · Avoiding Accidental Complexity · Architecture Documentation · Upgrade Paths & Extensibility

- **[Schematic Design]({{< relref "schematic-design" >}})** — Encoding intent into circuits: reference designs, tolerance analysis, simulation, testability, and design reviews.
  *What this section covers:* Reference Designs · Translating Blocks into Schematics · Designing for Tolerance & Variation · Simulation Before Commitment · Signal Naming & Documentation · Design for Test · Early Design Reviews · Grounding Strategy in Schematics

- **[Part Selection & Sourcing]({{< relref "part-selection-and-sourcing" >}})** — Where design meets supply chain: availability, lifecycle, second-sourcing, and designing for substitution.
  *What this section covers:* Availability vs Ideal Specs · Parametric Search Pitfalls · Lifecycle Status · Distributor Realities · Second-Source Strategies · Designing for Substitution

- **[PCB Layout & Physical Integration]({{< relref "pcb-layout-and-physical-integration" >}})** — Where ideas become geometry: stackups, mechanical constraints, thermal paths, and assembly awareness.
  *What this section covers:* Layout as Design · Stackups & Materials · Mechanical Constraints · Connector Placement · Design for Assembly · Thermal Considerations

- **[Prototyping & Bring-Up]({{< relref "prototyping-and-bring-up" >}})** — First contact with reality: prototype strategies, bring-up checklists, power validation, and capturing lessons.
  *What this section covers:* Prototype Strategies · Assembly Options · Bring-Up Checklists · Power-First Validation · Safe Smoke Testing · Capturing Early Lessons

- **[Validation & Verification]({{< relref "validation-and-verification" >}})** — Does it actually meet intent? Functional testing, environmental testing, margin analysis, and regression.
  *What this section covers:* Functional Validation · Environmental Testing · Margin Testing · Regression Testing · Documentation of Results · Feeding Data Back

- **[Iteration & Revision Control]({{< relref "iteration-and-revision-control" >}})** — Design is revision: change tracking, BOM management, spin-vs-patch decisions, and versioning sanity.
  *What this section covers:* Revision Discipline · Change Tracking & Rationale · BOM Drift · Hardware Versioning Sanity · When to Spin vs Patch · ECOs Without Bureaucracy

- **[Design for Manufacturing & Longevity]({{< relref "design-for-manufacturing" >}})** — Thinking beyond the bench: manufacturability, yield, serviceability, and end-of-life planning.
  *What this section covers:* Manufacturability Basics · Assembly Yield · Cost Reduction Passes · Documentation Handoff · Serviceability · End-of-Life Planning

- **[Project Retrospectives]({{< relref "project-retrospectives" >}})** — Where experience accumulates: surprises, quiet failures, broken assumptions, and turning mistakes into patterns.
  *What this section covers:* What Surprised You · What Failed Quietly · What Assumptions Broke · What You'd Do Differently · Turning Mistakes into Patterns
