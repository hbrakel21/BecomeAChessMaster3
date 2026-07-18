# Prototype 10 — Reverse Gate Runner

A playable micro-loop prototype of the "reversed" gate-runner concept:
you stand your ground while the orc horde storms toward you.

**The twist: shooting IS choosing.** Orc squads carry upgrade gates.
Wipe out a squad to claim its gate; the other gate in the pair shatters.
No aiming button, no choice button — one thumb does everything.

## How to play

Open `index.html` in any browser (desktop or mobile).

- **Drag** left/right to move — you fire automatically.
- Kill a gate-carrying squad to claim its upgrade (damage, fire rate, multishot, piercing, allies).
- Orcs that cross the red line cost hearts. 10 hearts and you're overrun.
- Push the horde back 300m to summon the Warlord. Kill him to win.

## Design notes

This prototype tests whether the runner-genre appeal survives the inversion:

1. **Progress feeling** → tug-of-war meter ("pushed Xm") instead of a finish line.
2. **Choice = control** → gates are carried by enemies; the squad you shoot is the gate you pick.
3. **Snowball fantasy** → visible weapon evolution + allies joining you.
4. **Tension** → the approaching wall of orcs is the timer.

Single self-contained HTML file, no dependencies, no build step.
