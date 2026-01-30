---
title: "Rework & Repair Techniques"
weight: 50
---

# Rework & Repair Techniques

Once you've found the faulty component, you need to remove it and install a replacement without damaging the board. Rework is a manual skill — reading about it helps, but practice on scrap boards is essential.

## Through-Hole Desoldering

### Solder Sucker (Desoldering Pump)

Best for: through-hole pins, large pads, clearing plated-through holes.

1. Heat the joint with the iron until the solder is fully molten
2. While holding the iron on the joint, bring the solder sucker tip right next to the joint
3. Release the pump — it sucks the molten solder away
4. Repeat if needed. Some joints take 2–3 passes
5. Gently wiggle the component to check if it's free

**Tips:**
- Keep the sucker tip clean and the seal intact — a worn tip loses suction
- Heat from one side, suck from the same side. Don't try to heat from the front and suck from the back
- If the hole is still plugged, add fresh solder (to improve thermal contact and add flux), then suck again

### Solder Wick (Desoldering Braid)

Best for: cleaning up pads, removing solder bridges, through-hole joints where the sucker can't reach.

1. Place the wick on the joint
2. Press the hot iron on top of the wick
3. The wick draws solder up by capillary action
4. Lift wick and iron together — don't drag the wick while it's soldered to the pad

**Tips:**
- Use the right width wick for the job — too narrow is slow, too wide steals heat
- Add a touch of flux to the wick if it's not picking up solder well
- Fresh wick works much better than the end that's already saturated — clip off the used portion

### Pad Care

Through-hole pads and traces are bonded to the board with adhesive and copper plating. They can lift if you apply too much force or heat.

- **Don't pry** components out. If it's not free, you need to remove more solder
- **Limit heat time** — a few seconds per pad. If it's not working, let the board cool and try again
- **If a pad lifts**, you can sometimes glue it back with cyanoacrylate (super glue) and re-solder carefully. If the trace is broken, you'll need a jumper wire

## SMD Replacement

### Two-Terminal Components (Resistors, Caps, Diodes)

The simplest SMD rework. Two approaches:

**Iron method:**
1. Add fresh solder to both pads (more solder = more thermal mass = more working time)
2. Touch one pad with the iron to melt it, then slide the tip to the other pad
3. With a steady hand, alternate between pads quickly until both are molten simultaneously
4. Lift the component away with tweezers
5. Clean the pads with wick
6. Tack the new component on one pad first, then solder the other

**Hot air method:**
1. Apply flux to both pads
2. Heat evenly with hot air until solder melts
3. Lift component with tweezers
4. Clean pads, place new component, tack and solder

### Multi-Pin ICs (SOIC, TSSOP, QFP)

**Removal:**
1. Apply flux generously to all pins
2. For SOIC (8–16 pins): add solder to bridge all pins on one side, then heat with the iron to melt the whole bridge and lift that side. Repeat for the other side
3. For QFP (many fine-pitch pins): hot air is usually easier. Shield nearby components with kapton tape or aluminum foil, apply flux, heat evenly, lift when all pins are molten

**Installation (drag soldering):**
1. Clean and tin the pads lightly
2. Align the new IC carefully — get pin 1 orientation right!
3. Tack one corner pin
4. Check alignment. Adjust if needed (reheat the tack pin)
5. Tack the opposite corner
6. Apply flux to the remaining pins
7. With a fine tip and a small amount of solder on the iron, drag across the pins. The flux and surface tension pull solder to the pads and away from the gaps between pins
8. Inspect for bridges under magnification

### QFN and BGA

These packages have pads underneath, not on the sides. You cannot rework them with an iron alone.

**QFN:** Hot air is the standard approach. Apply flux, heat from above evenly. The part will visibly settle when the solder melts. For removal, lift with tweezers once molten.

**BGA:** Requires controlled hot air (ideally a rework station with a nozzle matched to the package), solder paste or preforms for re-balling, and good alignment. This is advanced rework — practice on scrap before attempting it on something you care about.

**Temperature guidance:**
- Lead-free solder: 240–260°C peak (hot air station set higher to account for airflow losses)
- Leaded solder: 200–220°C peak
- Always preheat the board to ~100–150°C to reduce thermal shock and warp

## Lifted Pads and Trace Repair

When a pad lifts or a trace breaks, you need to restore the electrical connection.

### Trace Repair

1. Scrape the solder mask to expose bare copper on both sides of the break (use a hobby knife carefully)
2. Tin the exposed copper
3. Bridge with a short piece of fine wire (30 AWG wire-wrap wire works well), soldered to both exposed copper areas
4. Optionally cover with UV-cure solder mask or conformal coating

### Pad Repair

If a through-hole pad has lifted but the trace is intact:
1. Glue the pad back to the board with cyanoacrylate
2. Re-solder carefully with minimal heat
3. If the pad is gone entirely, solder the component lead directly to the trace (scrape to expose copper first)

## Jumper Wiring

When you need to add a connection that doesn't exist on the board — whether for a repair, a modification, or a design fix.

**Wire gauge selection:**
- Signal lines: 30 AWG wire-wrap wire (thin, easy to route, handles mA-level signals)
- Power lines: 26–24 AWG or heavier, depending on current. Match the wire gauge to the current it will carry
- Coax or shielded wire for sensitive signals (RF, high-speed clocks) if needed

**Routing:**
- Keep jumpers short and direct
- Secure long jumpers to the board with kapton tape or a dab of adhesive to prevent snagging
- Route away from heat sources
- Label the jumper (a tag of tape with a note) so future you knows why it's there

## Cleaning After Rework

Flux residue left on the board can cause problems over time: corrosion, leakage currents, and unreliable measurements.

**Why it matters:**
- Activated flux residue is corrosive — it was designed to dissolve oxides, and it'll keep corroding copper if left in place
- Flux residue between pads can create leakage paths, especially at higher voltages or with moisture
- Sticky flux attracts dust and makes inspection harder

**Cleaning procedure:**
1. Apply isopropyl alcohol (IPA, 90% or higher) to the rework area
2. Scrub with a stiff brush (a toothbrush works, ESD-safe brushes are better)
3. Wipe with a lint-free cloth or let air dry
4. Inspect under magnification — make sure all residue is gone, especially between fine-pitch pins

**No-clean flux** is designed to leave benign residue, but "benign" depends on the application. For anything precision or high-impedance, clean it anyway.
