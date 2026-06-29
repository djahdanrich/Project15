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

The run is a branching chain of races (OutRun-style road map). Each branch node offers a different difficulty/reward ratio. A completed run ends at a final boss race. Losing any race ends the run immediately — no continues, no saved state. The only carry-forward between runs is player knowledge (track layouts, rival patterns, part synergies).

---

## 6. Player Experience Goals

> *What should the player feel at key moments? Write in first-person player voice.*

- **At the start of a session:** "I know this car is slow — but I know what I'm doing this time."
- **After a win/success:** "That part is going to change everything. Do I take the harder branch?"
- **After a loss/failure:** "I was one race away. I know exactly what I did wrong."
- **After extended play:** "I finally strung together a clean run. The car felt like mine by the end."

---

## 7. Game Mechanics

> *Detail each mechanic: what it is, how it works, and why it serves the core pillars.*

### 7.1 — [Mechanic Name]

**What:** TBD  
**How:** TBD  
**Why:** TBD

### 7.2 — [Mechanic Name]

**What:** TBD  
**How:** TBD  
**Why:** TBD

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

### Key NPCs

| Name | Role | Relationship to Player |
|------|------|------------------------|
| TBD | | |

---

## 11. Progression Systems

> *How does the player grow — in skill, power, story, and/or unlocks?*

### Player Skill Progression

No persistent power. The only thing that carries between runs is the player's knowledge of track layouts, rival patterns, and part synergies. Skill is the progression.

### Run Structure

A run is **5 races** against rival drivers. The branching structure (linear vs OutRun-style fork) is TBD — see Open Questions.

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
| 5 | Is there a rival/opponent identity system (named drivers, recurring characters) or purely mechanical AI? | Daniel | — |
| 6 | Is the 5-race run structure linear (race 1 → 2 → 3 → 4 → 5) or OutRun-style branching forks? | Daniel | — |
