# 4D Biomechanical Vector Analyzer — SNG Faceoff

Interactive 3D physics visualization of a men's field lacrosse faceoff, built on the legal **Standing Neutral Grip (SNG)** stance. Single-file HTML/Three.js — open `faceoff-vector-analyzer.html` in any browser, no build step.

## What it models

A rigid multi-body simulation of the clamp-and-exit, driven by one trajectory across:

- **§1 Kinematic state** — head roll θ(t) about the shaft's long axis, horizontal sweep ψ(t)
- **§2 Neuromechanical latency** — reaction time + electromechanical delay, first-order force rise to F_max
- **§3 Rotational inertia, I∥** — forearm/hand inertia about the shaft roll axis (grip-width independent)
- **§4 Torque budget** — Class-1 lever, bottom hand as fulcrum, Δτ vs. opponent
- **§5 Tribology** — Coulomb friction at the head-turf interface, dry/wet states
- **§6 Hertzian contact** — ball-head contact patch, pressure profile
- **§7 Master equation** — t_cover = (RT + EMD) + √(2θ_c·I∥/τ_net)

Includes a play/scrub timeline, opponent ghost, live pressure plot, and a winning/losing/live summary matrix.

## Headline result

The mechanical roll term is small (~10–20 ms) once I∥ is correctly modeled as forearm-dominated (~6.5×10⁻³ kg·m², independent of hand spacing). **t_cover is neural-latency-dominated** — RT + EMD account for ~85–90% of total time to pin. Grip width affects the §4 drive lever (mechanical advantage), not the roll inertia.

| | Winning frame | Losing frame |
|---|---|---|
| t_cover | 0.245 s | 0.346 s |
| I∥ | 6.50×10⁻³ kg·m² (const.) | 6.50×10⁻³ kg·m² (const.) |
| Δτ | +8 N·m | −6 N·m |

## Stance / geometry conventions

- **SNG only** — knee-down is outlawed under current NCAA rules; both takers stand
- Ball sits on the player's **right**; right hand is the throat/near-head hand (palm-up start), left hand mid-shaft
- Both hands start grounded, shaft flat; left hand rises through the clamp, returns to the ground by the end
- 45° exit punch directed **away from the body**, in the drive phase
- Right foot (closest to the ball) steps down the line toward the ball during the exit, pulling CoM in
- Schematic body (torso/arms/legs) is low-poly and recomputed each frame from fixed shoulder/hip anchors to live hand/foot positions

## Known limitations / open questions

- **§3 "supination roll" framing is a simplification.** Forearm supination ROM (~85° from neutral) can't alone account for a 90° roll starting from a palm-up grip; the real motion likely distributes across wrist + shoulder + top-hand pronation. The model uses a single rotational DOF as an abstraction.
- **ψ = θ/2 sweep coupling is an animation assumption**, not a kinematic law — the roll and sweep are driven by different muscle groups and aren't rigidly linked in a real clamp.
- Body rig proportions (`RIG` joint coordinates, `SHAFT_TILT`/`TILT_MAX`, `SWEEP_SIGN`, `PUNCH_X`, `STEP_X`) are tuned visually and may need adjustment — all exposed as named constants near the top of the player-rig block.
- The model treats the clamp as a single deterministic trajectory; it does not represent counter-moves (jam, rake, plunger) or a mixed-strategy framing.

## Tech

Three.js r128 (CDN), vanilla JS, no dependencies, no build. Constraints: no `OrbitControls`, no `CapsuleGeometry` (r142+), no `localStorage`.
