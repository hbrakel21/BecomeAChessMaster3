# Prototype 11 — Build Identity Iteration

## What was built

**Branches replace flat upgrades.** Five branches (FIREPOWER / VELOCITY /
SPREAD / PIERCE / COMMAND), levels 1–6, stats recomputed from levels on every
change so the +1/+2/+3 tier jumps can never drift. Gate tier now maps to
levels gained: bronze +1, silver +2, gold +3 — the greed line gambles on
reaching keystones earlier.

**Keystones at level 5**, all running through one on-hit framework
(plain-object bullets carrying `rail` / `carry` / `incend` / `ksMul`;
enemies carrying burn and shield status):

- Overkill (FIRE): excess kill damage carries into the next enemy in the path
- Molten Barrel (VEL): every 4th volley incendiary; burn ticks 4×/s, spreads on death
- Wall of Lead (SPREAD): knockback doubled; shove-splash hits touching neighbors
- Railgun (PIERCE): +20% damage per enemy already pierced within the shot
- War Banner (CMD): allies inherit the player's keystones at 50% strength

**Mastery lock.** The second branch to reach Lv5 locks the other three with a
slow-mo beat; on-screen gates of locked branches shatter, and only the two
mastered branches spawn afterwards. HUD shows per-branch pips with keystone
stars and struck-through locked branches.

**Gate spawning serves the build.** Pairs always offer two different
branches; 60% of picks favor invested branches, 40% fresh. Banners read
"SPREAD Lv3→5 ★" and pulse with a "★ KEYSTONE ★" tag when a gate would
unlock a keystone (the signature beat).

**Three acts to 1000m (~10–12 min).** Champions end acts 1 and 2 (brute ×6,
drop a guaranteed free silver of an invested branch; a leaked champion still
opens the act so no soft-lock). Act 2 adds Shieldbearers (shield eats 3
hits/sec, pierced rounds ignore it). Act 3 adds Shamans (hold the backline,
heal nearby grunts). The Warlord scales to ~22s of the player's live DPS and
runs 3 phases (adds from phase 2, speed from phase 3). `difficulty()` is an
act-aware sawtooth over a rising baseline. Meters freeze during boss fights.

**Telemetry.** `window.TELEMETRY` logs every claim (branch/tier/meters/time/
rolling DPS), every break (reason/tier), keystone and lock timestamps, and
per-run summaries with hearts lost per act. Debug overlay ('D' or triple-tap
top-left) shows build, claim history, gold %, entity counts, fps and a
rolling 60s DPS graph.

## Verified in automated tests

Recompute correctness, gate pair distinctness and labels, all five keystones
(including a fixed bug: Wall of Lead splash originated from the *post-shove*
position and never hit anyone — impact position is now captured before the
knockback), shield block/pierce-bypass, mastery lock restricting spawns to
the mastered pair, champion → bounty → act transition, act-gated enemy
mixes, warlord phase transitions and win, telemetry records, debug toggle.
60fps held with 160-enemy cap. No console errors.

## Known balance concerns

1. **Gold % is unmeasured.** Target band is 25–45% of claims (comment at
   `TUNE.gates.tierAt`). Nothing validates it yet — needs human playtests;
   the debug overlay surfaces the number.
2. **Gate cadence (18–24s) is a guess.** ~25 pairs/run, ~half claimed at avg
   silver ≈ 24 levels. If players feel starved early, lower `gates.min`
   toward 14 rather than raising tier values.
3. **COMMAND is the weakest identity** (flagged in the archetype pass):
   FIRE+CMD and VEL+CMD both read as "shooty squad" — COMMAND amplifies your
   build rather than bending it. Candidate fix: allies could hold POSITION
   (deployable turrets) instead of following, making COMMAND about map
   control.
4. **Molten Barrel vs Shieldbearers:** burn bypasses the shield entirely
   (DoT is not a "hit"), which softens the intended VELOCITY punish in act
   2. Might be fine — it is a keystone after all — but watch it.
5. **playerDps() keystone estimates are rough** (rail ≈ pierce/2 stacks,
   overkill ≈ ×1.15, burn uptime capped at +50%). If squads in act 3 die
   far faster than `focusSeconds`, tighten these first.

## Archetype predictions

- **Strongest: FIRE+PIERCE ("Railgun sniper").** The multipliers are the
  only two that stack *within a single bullet* (rail % applies to a bigger
  base, overkill carries the inflated result), it deletes shieldbearers
  (pierce bypasses) and shamans (reaches the backline through the crowd),
  and boss adds line up in columns. Every act-2/act-3 mechanic plays into it.
- **Weakest: SPREAD+COMMAND ("Phalanx").** Both keystones are crowd-control
  against chaff that already dies in one hit; neither adds single-target
  damage, so Champions and the Warlord become slow HP walls, and allies
  inheriting *Wall of Lead* barely matters since ally knockback events are
  rare. Great feel in act 1, likely stalls in act 3.

## Next iteration candidates (out of scope here)

Meta-progression validation gate first: confirm two different keystone pairs
feel mechanically different in human playtests before building unlocks on
top.
