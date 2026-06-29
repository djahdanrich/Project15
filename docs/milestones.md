# Milestones — Project 15

> No target dates. Each milestone is a self-contained unit of work — completable, testable, and roughly equal in effort. If a milestone feels too big, split it.

---

## M01 — Project Foundation

- Godot 4 project created, folder structure set
- Single scene with one RichTextLabel filling the viewport
- 8 inputs mapped (d-pad + 4 action buttons) and echoing to console
- CRT shader wired up and togglable

---

## M02 — Pseudo-3D Road Renderer

- Horizontal band rendering draws a road to screen via RichTextLabel BBCode
- Vanishing point perspective (bands narrow toward top)
- Road scrolls forward at a fixed speed
- Basic colour palette applied (road, stripes, sky)

---

## M03 — Road Curves & Track Segment System

- Track defined as a sequence of segments (straight, curve left, curve right)
- Road visually bends when a curve segment is active
- One placeholder looping track to drive through

---

## M04 — Environment Layer

- Treeline row rendered above road horizon
- Guardrail characters along road edges
- Tunnel mouth arch renders on tunnel segments
- Skyline/mountain ridge row at top of frame

---

## M05 — Car Rendering on Road

- One placeholder ASCII car silhouette centred in frame
- Car sits correctly in pseudo-3D perspective
- Speed and gear value displayed in HUD area

---

## M06 — Driving Fundamentals

- Accelerate, brake, and gear shift inputs affect speed simulation
- Speed feeds into road scroll rate
- Gear model (up/down shifting with clutch input)
- Car feels like it has weight — not instant response

---

## M07 — Drift Initiation QTE

- QTE fires when a corner segment approaches
- 4-step input sequence: gear down + clutch → brake → handbrake → counter-steer
- Clean initiation transitions to drift state
- Mistimed initiation results in understeer (time loss) or spin out

---

## M08 — Drift Maintenance (2-Axis Zone)

- 2D zone box renders on screen during drift
- X axis (d-pad L/R) controls steering angle
- Y axis (accelerate taps) controls throttle depth
- Reticle moves within box; exits zone = spin out
- Zone shrinks as Y depth increases

---

## M09 — Scoring System

- Initiation score: timing accuracy × entry speed
- Maintenance score: time in zone × angle depth × throttle depth
- Score banked to run total on clean corner exit
- Combo multiplier for chained consecutive drifts
- Live score counter visible in HUD

---

## M10 — Ghost & Race Win/Loss

- Each track has a ghost target: time + score threshold
- Race timer runs during the race
- Win condition: finish within time AND meet score threshold
- Loss condition: time out, spin out, or fail threshold
- Star rating calculated on win (1 / 2 / 3 stars)

---

## M11 — Parts Data & Effects

- 5 part categories defined in data (Tyres, Engine, Differential, Brakes, ECU)
- Multiple tiers per category
- Parts correctly modify drift axes (tyres = X zone width, diff = Y marker speed, engine = entry multiplier, brakes = QTE window, ECU = score multiplier)
- Car state tracks which parts are installed

---

## M12 — Part Roll & Selection Screen

- Star rating drives part roll count (3 / 6 / 9)
- Parts drawn randomly from tier pool
- Player selects 1 / 2 / 3 from the rolled set
- Selected parts applied to car state for the run

---

## M13 — Run State Management

- Run initialises with stock car (no parts)
- Installed parts persist across races within the run
- Loss at any race clears all parts, resets to stock
- Run completion triggers car unlock check

---

## M14 — Rival Card Screen

- Layout: header (race #, track, tier) + run history log + rival block + flavour text
- Rival ASCII car portrait displays centred
- Flavour text renders one character at a time (typewriter effect)
- Run history lists previous rivals beaten this run

---

## M15 — Car Review Screen

- Full ASCII car portrait displays
- Installed parts listed by category
- Base stats shown
- Read-only — no interaction, just the build summary

---

## M16 — Race Intro + In-Race HUD

- Race intro screen: track name, tier, ghost targets (time + score)
- In-race HUD: speed, gear, ghost delta timer, live drift score
- Drift zone UI overlaid during drift maintenance phase

---

## M17 — Star Rating + Upgrade Screens

- Star rating display animates on win
- Part roll cards displayed and selectable
- Upgrade/install confirmation screen
- Installed parts confirmed before returning to rival card

---

## M18 — Game Over Card + Main Menu

- Game over card: rival portrait, victory flavour text, run summary
- Main menu screen
- Car select screen with unlock state visible
- Flow connects: menu → car select → rival card → race loop

---

## M19 — Track Layouts: Kasumi, Minato, Kage

- Kasumi Pass: gentle curves, pine forest, mist wash, open sky
- Minato Circuit: flat chicanes, container stacks, crane silhouettes, wet sheen
- Kage Bridge: mixed rhythm, overpass arches, concrete walls

---

## M20 — Track Layouts: Yurei, Mine, Kurayami

- Yurei Gorge: tight hairpins, dense treeline, cliff edge guardrail
- Mine Expressway: long straights, open summit, minimal environment
- Kurayami: all environment elements combined, tunnel arches, cliff drop, final hairpin

---

## M21 — All 10 Car ASCII Silhouettes

- Stern Dreier, Izumi Kei, Edison Kern, Ahura Kaze, Pleiad Arashi
- Aichi Raiden, Datsu Gin, Ahura Kaen, Datsu Kumo, Aichi Taiyō
- Each silhouette distinct and recognisable at a glance
- Integrated into car review screen and rival card

---

## M22 — All 16 Rival Portraits + Flavour Text

- ASCII car portrait for all 15 rivals + Ryuji Kaido
- Pre-race and post-loss flavour text written for all 16
- Kaido gets 3–4 lines — weightier, no boasting
- Typewriter effect tested with all text

---

## M23 — Audio: SFX

- All 8-bit event sounds implemented (initiation, spin out, score banked, win, loss, gear shift, part reward)
- Engine audio: downsampled recordings per drivetrain type, pitch-shifts with RPM
- Ambient wind/road hum

---

## M24 — Audio: Music

- Glitch-Eurobeat tracks integrated and looping per tier
- Boss track (Kurayami) distinct and more distorted
- Music transitions between screens

---

## M25 — Car Unlock System

- Completing a run unlocks the next car (persistent save)
- Unlock acknowledged on game over / run complete screen
- Car select reflects current unlock state across sessions

---

## M26 — Web Export & itch.io

- Godot HTML5 export configured and building cleanly
- Tested in browser (Chrome, Firefox)
- itch.io page created, build uploaded and playable

---

## M27 — Balance Pass

- Ghost targets tuned across all 6 tracks
- Star thresholds feel right (1 star = hard earned, 3 star = demanding)
- Part tier quality distribution reviewed
- At least one full run playtested per car tier (stock, mid, late)

---

## M28 — Mobile Controls (Stretch)

- On-screen d-pad and action button overlay
- Touch input mapped to existing input system
- Godot Android/iOS export tested
- Controls usable at intended play size

---
