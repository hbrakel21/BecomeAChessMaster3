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

## The greed line — the identity mechanic

In a runner the gate choice is free: you pass it anyway. Here, choosing has a
price — and the game leans into it. **Gates tier up as they approach you**:

- 🥉 **Bronze** at spawn (e.g. `+1 SHOT`)
- 🥈 **Silver** at 45% of the march (`+2 SHOTS`)
- 🥇 **Gold** at 75% — right above your danger line (`+3 SHOTS`)

Claim early = safe but weak. Let it come = strong, but the squad is seconds
from breaking through, and the rest of the horde marches on while you wait.
Every gate is a push-your-luck bet that changes value by the second. A runner
cannot do this — the run speed decides when you pass a gate; here *you*
decide the moment, and the moment costs something.

## Balance design (the `TUNE` object in index.html)

All balance dials live in one `TUNE` object at the top of the file, each with
its reasoning attached. The load-bearing rules:

- **Squad HP scales with your current DPS.** Wiping a gate squad always takes
  ~2.3 seconds of focused fire, no matter how strong you are. The choice must
  stay "which gate", never "can I even get one".
- **The corridor widens with your firepower.** With 1 shot you can only defend
  a narrow strip, so the horde starts marching in the middle 55% of the field
  and spreads out as you grow.
- **Pulsed pacing.** A quiet trickle punctuated by labeled surges (first one
  after 14s) creates a tension rhythm instead of constant noise.
- **The first gate arrives at 2s** so the core mechanic teaches itself
  immediately.

## Choice readability

- Gate squads wear a **colored ring** matching their gate's banner.
- Claiming a gate triggers a short **slow-mo beat**; the banner flies to you.
- The rejected gate visibly **shatters**: crossed out, tumbling off screen.
  Its orcs lose their rings and keep marching as regular enemies.

Single self-contained HTML file, no dependencies, no build step.
