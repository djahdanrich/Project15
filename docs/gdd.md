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
| **Setting / Era** | TBD |
| **Tone** | TBD |
| **World Size** | TBD |
| **Is the world static or dynamic?** | TBD |

### Key Locations

| Location | Description | Purpose in Gameplay |
|----------|-------------|---------------------|
| TBD | | |

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

### Player Skill Progression

No persistent power. The only thing that carries between runs is the player's knowledge of track layouts, rival patterns, and part synergies. Skill is the progression.

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
| **Visual Style** | TBD |
| **Colour Palette** | TBD |
| **Reference Titles / Art** | TBD |
| **Camera Perspective** | TBD |

---

## 14. Audio Direction

| Field | Value |
|-------|-------|
| **Music Style** | TBD |
| **SFX Approach** | TBD |
| **Voice Over** | TBD |
| **Reference Titles** | TBD |

---

## 15. Monetisation

| Field | Value |
|-------|-------|
| **Business Model** | TBD |
| **Price Point** | TBD |
| **DLC / Expansion Plans** | TBD |
| **Platform Revenue Share** | TBD |

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

- TBD

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
