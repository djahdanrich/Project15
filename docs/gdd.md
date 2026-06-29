# Game Design Document — Project 15 (working title)

> **Status:** Scoping  
> **Last Updated:** 2026-06-29  
> **Author(s):** Daniel

---

## Table of Contents

1. [Game Overview](#1-game-overview)
2. [Core Pillars](#2-core-pillars)
3. [Genre & Platform](#3-genre--platform)
4. [Target Audience](#4-target-audience)
5. [Core Gameplay Loop](#5-core-gameplay-loop)
6. [Player Experience Goals](#6-player-experience-goals)
7. [Game Mechanics](#7-game-mechanics)
8. [World & Setting](#8-world--setting)
9. [Story & Narrative](#9-story--narrative)
10. [Characters](#10-characters)
11. [Progression Systems](#11-progression-systems)
12. [UI / UX](#12-ui--ux)
13. [Art Direction](#13-art-direction)
14. [Audio Direction](#14-audio-direction)
15. [Monetisation](#15-monetisation)
16. [Scope & Milestones](#16-scope--milestones)
17. [Open Questions](#17-open-questions)

---

## 1. Game Overview

> *One-paragraph elevator pitch. What is the game, who is it for, and why does it feel good to play?*

Project 15 is a pseudo-3D retro street racer with roguelite progression, built entirely as a code-driven renderer inside a single Godot RichTextLabel — no traditional sprites or 3D meshes, just pure programmatic output. The title is a direct homage to Need for Speed (2015) — that game's dark, grounded street racing tone and no-nonsense attitude are the emotional benchmark. Layered over that is the mountain pass intensity of Initial D. Players start every run with a completely stock car and race through a branching road network in the vein of OutRun. Win a race and you're rewarded with parts — engine upgrades, suspension tuning, visual mods — which you install before the next leg. Lose a race and the run ends: back to the garage, back to stock, start again. The appeal is the compounding tension of a run in progress: every win raises the stakes of what you stand to lose, and the retro terminal aesthetic frames the whole thing like a game running on hardware that probably shouldn't be able to pull it off.

---

## 2. Core Pillars

> *3–5 non-negotiable design values every decision is measured against.*

| # | Pillar | Description |
|---|--------|-------------|
| 1 | **Every run is earned** | You start stock every time. There is no persistent power between runs — only skill and the parts you win mid-run. |
| 2 | **Speed is readable** | The pseudo-3D renderer must make velocity feel visceral and legible at a glance, even as pure code output. |
| 3 | **Parts matter, choices matter** | Each part reward is a meaningful decision. No filler drops — every upgrade should visibly change how the car handles or how fast the run can go. |
| 4 | **Retro authenticity** | Aesthetically and mechanically, the game should feel like it belongs to the era it references — late 90s/early 2000s street racing culture, no irony. |

---

## 3. Genre & Platform

| Field | Value |
|-------|-------|
| **Primary Genre** | Racing |
| **Sub-genre(s)** | Roguelite, Pseudo-3D, Arcade Racer |
| **Platform(s)** | Web (HTML5 → itch.io), Windows/Linux; mobile (Android/iOS) stretch goal |
| **Engine** | Godot 4 |
| **Rendering Architecture** | Single RichTextLabel — fully code-driven pseudo-3D renderer |
| **Target Resolution / Frame Rate** | TBD |
| **Single / Multiplayer** | Single-player |

---

## 4. Target Audience

| Field | Value |
|-------|-------|
| **Age Range** | 18–35 (nostalgia-driven); secondary 16–24 (retro aesthetic enthusiasts) |
| **Player Archetypes** | Car culture fans, roguelite players, demoscene / terminal aesthetic appreciators |
| **Comparable Titles** | OutRun (1986), Initial D Arcade Stage, Need for Speed (2015), Road Rash, Burnout Legends |

### Positioning

> *How does this game sit relative to its comparables? What does it do differently?*

Where OutRun is an endless joyride and NFS (2015) is a story-driven street racing drama, Project 15 fuses both with roguelite stakes — every run is a fresh attempt at a perfect chain of races and part rewards. The title is a direct nod to NFS 2015. The terminal/code-driven renderer is the key differentiator: it deliberately looks like something running inside a dev console, which is both a technical constraint and a core aesthetic identity no direct competitor shares.

---

## 5. Core Gameplay Loop

```
[RACE] → [WIN / LOSE] → [PART REWARD] → [INSTALL / SKIP] → [NEXT RACE]
                 ↓ lose
           [GAME OVER — back to stock]
```

### Micro Loop

Race → finish position determines part quality → choose which part to install before the next race begins. Each race segment is a single road stretch against one or more rival cars using the pseudo-3D renderer.

### Macro Loop

The run is a linear chain of 6 races. Races 1–5 each draw a rival randomly from that tier's pool — increasing difficulty and part reward quality as the tier rises. Race 6 is always the boss: a fixed, named opponent who represents the run's definitive skill check. Losing any race ends the run immediately — no continues, no saved state. The only carry-forward between runs is player knowledge (rival patterns, part synergies, boss behaviour).

---

## 6. Player Experience Goals

> *What should the player feel at key moments? Write in first-person player voice.*

- **At the start of a session:** "I know this car is slow — but I know what I'm doing this time."
- **After a win/success:** "That part is going to change everything. Do I take the harder branch?"
- **After a loss/failure:** "I was one race away. I know exactly what I did wrong."
- **After extended play:** "I finally strung together a clean run. The car felt like mine by the end."

---

## 7. Game Mechanics

### 7.1 — Touge Race Structure

**What:** Each race is a solo mountain pass run against a ghost. No opponent car on the road — the rival is represented by their ghost time and drift score to beat.

**How:** The player must finish the pass within the ghost's time AND meet or exceed the ghost's drift score. Both conditions must be met to win. Missing either ends the run.

**Why:** The dual win condition creates genuine tension — going flat-out ignores scoring, grinding for drifts bleeds time. Every corner is a micro-decision.

---

### 7.2 — Drift Initiation (QTE)

**What:** Entering a corner triggers a quick time event to initiate the drift.

**How:** As the corner approaches, the QTE fires. The player executes the initiation sequence in order:

1. **Gear down** (D-pad Down) + **Clutch tap** — weight transfer prep, shift into the corner
2. **Brake tap** — trail brake to pitch weight to the rear
3. **Handbrake** — break rear traction, initiate the slide
4. **Counter-steer** (L/R d-pad) — catch the car

Initiation score scales with:
- **Timing accuracy** — how close to the ideal initiation point
- **Entry speed** — faster entry = higher multiplier (governed by Engine part)
- **Brake input quality** — correct trail brake widens the timing window (governed by Brakes part)
- **Clutch timing** — hitting the clutch kick cleanly adds a bonus score (governed by Differential part)

A clean sequence transitions into drift maintenance. A mistimed sequence either understeers (wasted corner, time loss) or spins out (run over).

**Why:** Each step maps to a real drift technique — clutch kick, trail brake, handbrake flick, counter-steer. The QTE is the turn-in moment compressed into taps. Skill lives in the sequence and timing, not analog precision.

---

### 7.3 — Drift Maintenance (Balance Minigame)

**What:** Once a drift is initiated, the player holds it through the corner by keeping an angle indicator inside a moving sweet spot — inspired by the Stardew Valley fishing minigame.

**How:** The maintenance phase uses **two simultaneous axes** displayed as a 2D zone in the RichTextLabel — a reticle the player keeps inside a box.

| Axis | Input | Controls |
|------|-------|----------|
| **X — Steering Angle** | D-pad Left / Right | How much counter-steer is applied — keeps the car pointed along the road |
| **Y — Throttle Depth** | Accelerate (taps) | How hard the throttle is pushed — controls rear wheel spin and drift angle |

**The axes push against each other.** More throttle (Y axis deeper) forces the angle (X axis) to swing out further and faster — harder to hold, but higher points per frame. The player chooses how deep to push the throttle and takes on the corresponding steering challenge.

```
Points per frame = base_rate × angle_depth_multiplier × throttle_depth_multiplier
```

Max points = deep throttle + sharp angle + staying in zone. The zone shrinks as throttle depth increases — the risk/reward is live and continuous, not a one-time decision.

**Zone states:**
- **Reticle in zone** → score accumulates, drift holds
- **Reticle at edge** → score pauses, warning
- **Reticle exits zone** → spin out, run over
- **Corner exit** → drift ends cleanly, score banked

Parts affect each axis independently:

| Part | Effect |
|------|--------|
| Tyres | Wider zone on the X axis (steering angle more forgiving) |
| Differential | Y axis movement is slower (throttle depth easier to hold) |
| ECU | Score multiplier on all points banked from this drift |

**Why:** Two axes interacting creates genuine skill expression — the player is constantly deciding how much risk to carry. A cautious run with shallow throttle is safe but won't beat a high-scoring ghost. A deep-throttle run is max points but punishing. That tension is the game.

---

### 7.4 — Scoring

**What:** Drift score is accumulated across all corners in a run and must meet the ghost's threshold to win.

**How:** Each drift banks a score calculated as:

```
Initiation Score (timing × entry speed)
+ Maintenance Score (time in zone × angle_depth × throttle_depth × ECU multiplier)
```

Chained drifts (consecutive corners without fully straightening) apply a combo multiplier (TBD scaling). Final run score is the sum of all banked drifts.

The throttle depth multiplier is the primary scoring lever — a player who pushes deep on every corner will outscore a cautious player even with fewer clean drifts. This means beating a high-tier ghost's score requires intentional risk-taking, not just clean execution.

**Why:** Rewards both precision (clean initiation) and endurance (holding the drift long and clean through the corner).

---

### 7.5 — How Parts Affect Mechanics (Summary)

| Part | Initiation Effect | Maintenance Effect |
|------|------------------|-------------------|
| **Tyres** | — | Wider sweet spot zone |
| **Engine** | Higher entry speed multiplier | — |
| **Differential** | Clutch kick bonus score | Slower angle marker drift |
| **Brakes** | Wider QTE timing window | — |
| **ECU** | — | Score multiplier on banked drifts |

Higher-tier parts improve the same parameters — they don't unlock new mechanics, they make the existing mechanics more forgiving and rewarding.

---

## 8. World & Setting

| Field | Value |
|-------|-------|
| **Setting / Era** | Contemporary Japan — night, urban fringes and mountain passes |
| **Tone** | Dark, atmospheric, grounded — NFS 2015 urban edge meets Initial D mountain isolation |
| **World Size** | 6 distinct tracks, one per race position (fixed order) |
| **Static or Dynamic** | Static — tracks are always in the same order; rivals are randomised |

Tracks are fixed per race position. Players learn the roads across runs — the road is the constant, the opponent is the variable. Each track has a distinct visual character expressed through the terminal renderer's environment elements.

---

### Tracks

| Race | Track | Type | Character |
|------|-------|------|-----------|
| 1 | **Kasumi Pass** | Mountain touge | Wide, open, mist-covered — forgiving intro. Pine forest both sides, open night sky. |
| 2 | **Minato Circuit** | Flat docklands circuit | Port/harbour industrial setting. Flat sweeping corners and chicanes — no elevation. Cranes, containers, wet tarmac. NFS urban energy. |
| 3 | **Kage Bridge** | Urban/industrial | Bridges and underpasses, concrete barriers, overpasses. Mixed urban and mountain transition. |
| 4 | **Yurei Gorge** | Technical touge | Dense hairpins, fast rhythm. Tight treeline, cliff drop on the outside edge. |
| 5 | **Mine Expressway** | High-speed summit | Open mountain summit, wide road, long straights. Fewer corners = must push deep on every drift to hit score threshold. |
| 6 | **Kurayami** | Boss — legendary descent | Ryuji Kaido's home road. Combines everything: hairpins, tunnel sections, cliff straight, final hairpin. The hardest and most complete track. |

### Renderer Environment Elements Per Track

| Track | Road Shape | Key Elements |
|-------|-----------|--------------|
| Kasumi Pass | Gentle curves, wide | Treeline, open sky, mist character (soft colour wash) |
| Minato Circuit | Flat chicanes, sweepers | Container stacks, crane silhouettes, wet road sheen, low horizon |
| Kage Bridge | Mixed rhythm | Overpass arches, concrete walls, urban light spill |
| Yurei Gorge | Tight hairpins | Dense treeline, cliff edge guardrail, narrow road width |
| Mine Expressway | Long straights, fast sweepers | Open sky, minimal environment, summit exposure |
| Kurayami | All types combined | Tunnel mouth arches, cliff drop, guardrails, treeline — full renderer palette |

---

## 9. Story & Narrative

### Premise

TBD

### Act Structure

| Act | Summary |
|-----|---------|
| Act 1 | TBD |
| Act 2 | TBD |
| Act 3 | TBD |

### Themes

TBD

---

## 10. Characters

### Player Character

| Field | Value |
|-------|-------|
| **Name** | TBD |
| **Role** | TBD |
| **Motivation** | TBD |
| **Abilities** | TBD |

### Rivals

Each tier has a pool of 3 rivals drawn randomly per run. Car and behavioural profiles TBD.

| Tier | # | Name | Style |
|------|---|------|-------|
| 1 | 1 | Yuta Kondo | Japanese |
| 1 | 2 | Dex Cruz | American |
| 1 | 3 | Jake Stone | American |
| 2 | 1 | Kenji Hara | Japanese |
| 2 | 2 | Rico Vega | American |
| 2 | 3 | Sho Tanaka | Japanese |
| 3 | 1 | Naoto Ishida | Japanese |
| 3 | 2 | Cole Nash | American |
| 3 | 3 | Hiro Watanabe | Japanese |
| 4 | 1 | Kazuma Mori | Japanese |
| 4 | 2 | Ace Dominguez | American |
| 4 | 3 | Ryo Takase | Japanese |
| 5 | 1 | Cain Mercer | American |
| 5 | 2 | Haruki Soma | Japanese |
| 5 | 3 | Taka Nishida | Japanese |

### Boss — Race 6

| Field | Value |
|-------|-------|
| **Name** | Ryuji Kaido |
| **Style** | Japanese |
| **Role** | The fixed final opponent — every run ends here |
| **Car / Profile** | TBD |

---

## 11. Progression Systems

> *How does the player grow — in skill, power, story, and/or unlocks?*

### Meta Progression — Car Unlocks

Car unlocks are the **only** persistent progression between runs. Completing a run unlocks the next car. Each car has different base handling stats — the stock feel is unique per car, which changes which parts you prioritise in a run.

The player starts with a beat-up **Stern Dreier** (stock, rough around the edges). Completing a run on any car unlocks the next. All car names use fictional manufacturer brands — obviously inspired, not licensed.

| # | In-Game Name | Inspired By | Name Logic | Unlock | Base Handling Profile | Parts Priority |
|---|-------------|-------------|------------|--------|----------------------|----------------|
| 1 | Stern Dreier | BMW M3 | Stern = star (DE); Dreier = three (DE) | Start | FR, balanced, slightly heavy — forgiving all-rounder | Tyres, Differential |
| 2 | Izumi Kei | Honda Civic | Izumi = spring (JP); Kei = Japanese car category | Run 1 | Light, nimble, weak stock engine — angle good, throttle shallow | Engine, ECU |
| 3 | Edison Kern | Ford Focus RS | Edison = American inventor; Kern = core/focus (DE) | Run 2 | Hot hatch feel, responsive, good stock brakes | Differential, Engine |
| 4 | Ahura Kaze | Mazda Miata | Ahura = Ahura Mazda (deity Mazda named after); Kaze = wind (JP) | Run 3 | Lightest car, best stock angle axis, low power ceiling | Engine, Brakes |
| 5 | Pleiad Arashi | Subaru WRX | Pleiad = one of the Pleiades (Subaru in JP); Arashi = storm (JP) | Run 4 | AWD — hardest to initiate drift, very stable once in zone | Brakes, ECU |
| 6 | Aichi Raiden | Toyota AE86 | Aichi = Toyota's prefecture; Raiden = thunder (JP) / Trueno = thunder (ES) | Run 5 | Light, nimble, underpowered — great angle, suffers throttle depth | Engine, ECU |
| 7 | Datsu Gin | Nissan Silvia S15 | Datsu = from Datsun (Nissan's original brand); Gin = silver (JP) / Silvia = silver (LA) | Run 6 | Natural FR oversteer — best stock drift platform, average everything else | ECU, Tyres |
| 8 | Ahura Kaen | Mazda RX-7 | Kaen = flame (JP) — rotary engines run notoriously hot | Run 7 | Rotary engine — high throttle depth ceiling, front-heavy, punishing angle | Differential, Brakes |
| 9 | Datsu Kumo | Nissan Skyline GT-R | Kumo = cloud (JP) — Skyline | Run 8 | AWD, massive power — hardest initiation, highest speed multiplier | Brakes, Tyres |
| 10 | Aichi Taiyō | Toyota Supra | Taiyō = sun (JP) — the apex, the pinnacle | Run 9 | FR, top-end power monster — hardest angle control, highest score ceiling | Tyres, Differential |

| State | What carries over |
|-------|------------------|
| Win a race | Parts installed (within run only) |
| Lose a race | Nothing — back to stock |
| Complete a run | Next car unlocked (permanent) |

### Player Skill Progression

Beyond car unlocks, the only thing that carries between runs is the player's knowledge — track layouts, rival patterns, part synergies per car, and when to push the throttle deep. Skill is the progression.

### Run Structure

A run is **6 races**:

| Race | Structure | Rival |
|------|-----------|-------|
| 1–5 | Tiered progression, linear chain | Randomly drawn from that tier's rival pool |
| 6 | Fixed boss race — always the same opponent | "The big guy" — the run's final challenge |

Rivals in races 1–5 are drawn randomly from a pool of **3 rivals per tier** (15 rivals total across 5 tiers). Players won't face the same sequence twice, but rivals have identity — car and behavioural profile. Names TBD. The boss in race 6 is always fixed: **Ryuji Kaido** — the known wall every run builds toward.

### Parts System

There are **5 part categories**. Winning a race rewards one or more parts from the pool. The player chooses which to install before the next race.

| Category | Role |
|----------|------|
| **Tyres** | Grip, cornering ability, surface handling |
| **Engine** | Top speed, acceleration curve |
| **Differential** | Power distribution, oversteer/understeer balance |
| **Brakes** | Braking distance, late-braking window |
| **ECU** | Tuning multiplier — amplifies other installed parts |

Each category has multiple tiers (exact count TBD). Higher-tier parts drop from harder races. Installing a part is permanent for the run — no swapping back.

---

## 12. UI / UX

### Input / Control Scheme

Max 8 inputs: 4-directional d-pad + 4 action buttons. Designed for on-screen touch controls (mobile) with keyboard equivalents.

| Input | Normal Driving | During Drift |
|-------|---------------|-------------|
| D-pad Left / Right | Steer | Counter-steer (direction correction) |
| D-pad Up / Down | Gear up / Gear down | — |
| Action: Accelerate | Throttle | Throttle feathering — **the fishing minigame input** |
| Action: Clutch | Clutch | Clutch kick (initiation QTE step) |
| Action: Brake | Brake | Trail brake (initiation QTE step) |
| Action: Handbrake | — | Break rear traction (initiation QTE trigger) |

### HUD Elements

TBD

### Menus & Flows

TBD

### Accessibility Considerations

TBD

---

## 13. Art Direction

| Field | Value |
|-------|-------|
| **Visual Style** | Dark terminal / code-driven pseudo-3D; car is the visual hero |
| **Colour Palette** | 8-colour soft terminal palette — muted, low-contrast, dark background |
| **Reference Titles / Art** | OutRun (road rendering), Initial D (car identity), NFS 2015 (tone/night aesthetic) |
| **Camera Perspective** | Behind-car, pseudo-3D first-person road view |
| **Post Processing** | CRT shader (pre-built, applied globally) |
| **Time of Day** | Night only |

---

### The Car is the Visual Hero

The car ASCII silhouette is the primary visual element — it sits large and centred in the frame. The road and environment exist to give it context and convey speed, not the other way around. The player must be able to recognise a Silvia from an RX-7 from an AE86 at a glance. Each car model has a distinct multi-line ASCII side-profile built from RichTextLabel BBCode characters.

Car silhouettes are coloured using the soft palette — the car's body colour is the dominant warm tone on screen. Visual upgrades (spoilers, body kits) are post-MVP; the silhouette is fixed per car for v1.0.

---

### Colour Palette

8 colours total. Soft, muted — akin to a dark-mode developer UI rather than arcade neon. No pure whites or saturated primaries.

| Role | Colour | Usage |
|------|--------|-------|
| **Background** | Deep charcoal navy | Sky, negative space, UI background |
| **Road surface** | Muted slate grey | Road fill |
| **Road markings** | Warm off-white | Centre lines, road edges |
| **Road stripes** | Soft amber | Alternating OutRun-style road bands |
| **Environment** | Muted sage | Treeline, mountain silhouettes, guardrail posts |
| **Car body** | Soft coral / periwinkle | Player car (varies by car model) |
| **Rival / ghost** | Muted lavender | Rival car representation |
| **UI / accent** | Cool teal | HUD elements, score, warnings |

Exact hex values TBD during implementation. All 8 colours must remain readable against the deep charcoal background at the CRT shader's intensity.

---

### Road Rendering (Pseudo-3D)

OutRun-style horizontal band rendering. The road is drawn as stacked rows of characters, each row narrower toward the top to suggest a vanishing point. Road bands alternate between two tones (road surface + stripe) to convey forward motion as they scroll.

Japanese mountain touge elements layered on top:
- Armco guardrail characters along road edges
- Treeline silhouette row above the road horizon
- Tunnel mouth (character block arch) for specific corner types
- Mountain ridge outline at the skyline

The environment is sparse — enough to read as a touge pass, not so much it competes with the car.

---

### Drift Zone UI

The 2-axis drift maintenance box is rendered in-world, overlaid on the lower portion of the screen. Soft teal outline box, coral reticle marker. The box visually shrinks on the throttle axis as depth increases — the tightening zone is legible at a glance without needing text labels.

---

## 14. Audio Direction

| Field | Value |
|-------|-------|
| **Music Style** | Glitch-Eurobeat — high BPM Eurobeat processed through glitch/bitcrush effects |
| **SFX Style** | Retro 8-bit for UI/events; downsampled real engine recordings for car audio |
| **Voice Over** | None (out of scope v1.0) |
| **Reference — Music** | Initial D soundtrack (Eurobeat core), Arca / Crystal Castles (glitch texture) |
| **Reference — SFX** | Classic arcade racers (OutRun, Ridge Racer) for 8-bit event sounds |

---

### Music

Eurobeat is the correct genre for touge racing — it's literally the Initial D soundtrack DNA. The glitch layer is what makes it distinctly Project 15: tracks sound like Eurobeat that's been run through a broken terminal. Bitcrushing, stuttering, digital artefacts woven into the production rather than applied as an effect. The BPM stays high (140–160) to match the racing energy.

Each race tier could have a distinct track or intensity level. The boss race (Ryuji Kaido) warrants its own track — harder, more distorted, the glitch more aggressive. TBD whether the music reacts dynamically to the drift state (e.g. filter opens up when deep in a drift).

### SFX

**Engine audio:** Real car engine recordings — but downsampled and bitcrushed to match the terminal aesthetic. The engine doesn't sound clean; it sounds like the game is barely holding the audio together. Each car model has a distinct engine profile (rotary vs inline vs boxer). Pitch-shifts with gear and RPM.

**8-bit event sounds:**

| Event | SFX Character |
|-------|--------------|
| Drift initiation QTE prompt | Sharp 8-bit blip, rising tone |
| Clean initiation | Short ascending 8-bit chord |
| Mistimed initiation / spin out | Descending buzz, flat |
| In-zone drift | Low looping tone, slightly glitched |
| Score banked | 8-bit register chime |
| Gear shift | Crisp click + short pitch pop |
| Race win | Ascending 8-bit fanfare |
| Race loss / run over | Descending flatline tone |
| Part reward | Warm 8-bit notification chord |

**Ambient:** Minimal. Wind noise downsampled to near-noise. Road surface hum. The music carries the atmosphere — ambient SFX stays out of the way.

---

## 15. Monetisation

> **Deferred.** Focus is on building the game. Monetisation to be decided once v1.0 is complete. Likely itch.io release — pay-what-you-want or one-time purchase are the natural options for that platform.

---

## 16. Scope & Milestones

| Milestone | Deliverables | Target Date |
|-----------|--------------|-------------|
| Greenlight / Concept | GDD v1, concept art, vertical slice spec | TBD |
| Prototype | Playable core loop | TBD |
| Alpha | All features in, rough content | TBD |
| Beta | Content complete, bug-fixing | TBD |
| Gold / Ship | Release-ready | TBD |

### Out of Scope (v1.0)

> *Explicitly list features that are NOT in the initial release to prevent scope creep.*

- Visual / cosmetic part upgrades (body kits, spoilers, wheel changes)
- Multiplayer of any kind
- Story mode / cutscenes / voiced narrative

---

## 17. Open Questions

> *Track unresolved design decisions here. Move to the relevant section once resolved.*

| # | Question | Owner | Due |
|---|----------|-------|-----|
| 1 | ~~What is the target platform?~~ **Resolved:** Web/HTML5 (itch.io) + Windows/Linux; mobile stretch goal. | Daniel | — |
| 2 | ~~How many races per run?~~ **Resolved:** 5 races against rivals. Branch structure (linear vs fork) still TBD — see #6. | Daniel | — |
| 3 | ~~How many part categories?~~ **Resolved:** 5 — Tyres, Engine, Differential, Brakes, ECU. Tier count per category TBD. | Daniel | — |
| 4 | ~~Does the game have a working title beyond "Project 15"?~~ **Resolved:** "Project 15" is the title — homage to NFS (2015). | Daniel | — |
| 5 | ~~Rival identity system?~~ **Resolved:** Named rivals with identity, randomly drawn per tier from a pool. Race 6 is always the fixed boss. | Daniel | — |
| 6 | ~~Linear vs branching run structure?~~ **Resolved:** Linear 6-race chain — races 1–5 randomised rivals, race 6 fixed boss. | Daniel | — |
| 7 | ~~Rival pool size?~~ **Resolved:** 3 rivals per tier, 15 total + 1 boss. | Daniel | — |
| 8 | ~~Boss identity?~~ **Resolved:** Boss is **Ryuji Kaido**. All 15 rivals named — mix of Japanese (Initial D), American English, and F&F Latino styles. Car/behavioural profiles TBD. | Daniel | — |
| 9 | ~~Car roster?~~ **Resolved:** 10 cars, fully fictional names. Stern Dreier → Izumi Kei → Edison Kern → Ahura Kaze → Pleiad Arashi → Aichi Raiden → Datsu Gin → Ahura Kaen → Datsu Kumo → Aichi Taiyō. Each name has an obscure real-world connection. | Daniel | — |
| 10 | ~~Visual upgrades?~~ **Resolved:** Deferred post-MVP. Terminal renderer leaves little room for cosmetic complexity. Mechanical parts only for v1.0. | Daniel | — |
| 11 | Does music react dynamically to drift state (e.g. filter opens, BPM locks to drift rhythm)? | Daniel | — |
| 12 | Does each car have a unique engine audio profile, or are cars grouped by drivetrain type (rotary, inline, boxer)? | Daniel | — |
