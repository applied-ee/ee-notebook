---
title: "Content Style Guide"
weight: 1
---

# Content Style Guide

This document defines the structure, terminology, and intent of pages in the Engineer’s Notebook. Its purpose is consistency — not just in formatting, but in how readers develop **applied reasoning** at the bench.

The goal is that after reading a handful of pages, a reader subconsciously knows:
- where to find practical guidance
- where to look for traps
- how theory connects to real measurements

---

## Page Levels

The notebook is organized into three conceptual levels:

- **L0** — Major domains (e.g. *Fundamentals*)
- **L1** — Topic groupings within a domain (e.g. *Laws & First Principles*)
- **L2** — Individual concepts or techniques (e.g. *Ohm’s Law*)

Each level has a distinct purpose and writing style.

---

## L0 Pages (Domain Overviews)

**Purpose:**  
Orient the reader. Explain *why this domain exists* and *when they should come here*.

**Tone:**  
High-level, conceptual, motivating.

**Characteristics:**
- No equations
- No procedures
- Minimal examples
- Focus on scope and mental framing

**Typical Structure:**
- Short conceptual introduction
- Why this domain matters
- List of child L1 sections with brief descriptions

**Example:**  
⚡ *Fundamentals* — Nature’s rules and universal constraints.

---

## L1 Pages (Concept Groupings)

**Purpose:**  
Define a family of ideas and establish the *non-negotiable constraints* that bind them.

**Tone:**  
Authoritative but explanatory.

**Characteristics:**
- May include equations (sparingly)
- Explains what never fails versus what is commonly misapplied
- No step-by-step procedures
- Very limited numeric examples

**Typical Structure:**
- Conceptual framing
- What this section covers
- Curated list of L2 pages

**Example:**  
*Laws & First Principles* — The constraints every circuit obeys.

---

## L2 Pages (Core Learning Units)

**Purpose:**  
Teach one concept deeply enough to apply it correctly at the bench.

This is where theory meets reality.

**Tone:**  
Practical, grounded, experience-informed.

---

## Standard L2 Section Order

Not every L2 page must contain every section, but **when a section exists, its name and intent are fixed**.

### 1. Core Explanation  
(Title varies: *The Relationship*, *How It Works*, *What’s Happening Physically*, etc.)

- Explains the concept itself
- May include equations
- Minimal numeric examples
- Focus on correctness, not application

---

### 2. When It Applies / When It Doesn’t  
(Optional but common)

- Defines scope and limits
- Clarifies assumptions
- Separates ideal behavior from real-world behavior
- Prefer **plain-language limits** (“narrow range of conditions”, “specific situations”) over analysis jargon

---

### 3. Tips

**Purpose:**  
Enable correct use.

**What goes here:**
- Rules of thumb
- Sanity checks
- Bench techniques
- Safe defaults

**Guidelines:**
- May include **specific numbers** if they teach scale
- Should help the reader succeed
- Forward-looking
- Technique-oriented, not interpretive

**Example:**
- “500 mA through 100 mΩ drops 50 mV — that matters in low-voltage rails”

---

### 4. Caveats

**Purpose:**  
Prevent mistakes and false confidence.

**What goes here:**
- Common failure modes
- Measurement traps
- Situations where intuition breaks
- “This is where people get burned”

**Guidelines:**
- Warning-oriented
- Experience-based
- Numbers only if they explain the trap
- No happy-path examples

---

### 5. Bench Relevance

**Purpose:**  
Tie everything together.

This answers:
> “How does this show up when I’m holding probes?”

**Guidelines:**
- Interpretive, not procedural
- No new techniques (those belong in Tips)
- Avoid universal claims about what *every* engineer does
- Phrase authority in terms of **usefulness**, not habit or order

**Preferred framing:**
- “often useful”
- “a fast way to sanity-check”
- “commonly shows up as”
- “one of the quickest ways to reason about”

Bench Relevance is not a summary — it is a bridge.

---
## Narrative Voice

Use a neutral, tool-centric voice.

Avoid first-person and second-person phrasing (*I*, *we*, *you*, *your*).  
The goal is to describe how laws, models, and measurements behave — not to tell the reader what they personally do.

**Preferred framing:**
- “In practice…”
- “At the bench…”
- “This shows up as…”
- “A common sanity check is…”
- “When a measurement doesn’t line up…”

**Avoid:**
- “When you see…”
- “The first thing you reach for…”
- “If you do this…”
- “We can tell that…”

This keeps the text readable without projecting habits, authority, or assumptions onto the reader.

---

## Consistent Terminology (Canonical)

Use these terms verbatim across all pages:

- **Tips**
- **Caveats**
- **Bench Relevance**

Avoid:
- “Practical Bench Relevance”
- “Why This Matters at the Bench”
- “Bench Notes” (unless a special callout)

Consistency beats cleverness.

---

## Examples and Numbers

> **If an example uses realistic values and teaches scale, keep it.  
> If it only restates the equation, generalize it.**

- Concrete numbers are encouraged at L2
- Especially valuable for:
  - Power
  - Voltage drops
  - Currents
  - Thermal effects
- Avoid contrived algebra-only examples

This is an engineer’s notebook, not a textbook.

---

## Tone Contract

Across all levels:

- Prefer clarity over elegance
- Prefer physical intuition over math gymnastics
- Avoid analysis-native jargon unless it is explicitly being taught
- Treat mismatches as clues, not failures
- Assume the reader will test this on a real circuit

If something doesn’t add up, the law isn’t wrong — the application is.

---

## Final Principle

This notebook is not about memorizing laws.

It’s about building **applied reasoning**:
- knowing when a law applies
- knowing when it doesn’t
- understanding what real measurements are telling you when the numbers don’t match

That skill is what turns theory into working hardware.
