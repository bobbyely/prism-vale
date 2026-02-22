# Prism Vale — Project Design Document
**Version:** 0.2  
**Last Updated:** February 2026  
**Status:** Pre-production  

---

## 1. Vision Statement

Prism Vale is a cozy, semi-open-world adventure game where players explore a living world that communicates through colour. By forming bonds with emotional creatures called Valens, players restore vibrancy to a world slowly losing its colour — learning empathy, emotional awareness, and self-understanding along the way.

The game is deeply replayable, gentle in tone, and requires minimal reading. It targets Nintendo Switch and PC, and is designed to be accessible to children while offering enough depth for adults.

---

## 2. Core Pillars

**Emotional Colour Language**  
The world communicates through colour rather than text. Creatures, environments, and objects shift colour to express emotional states. Players learn to "read" the world visually.

**Creature Bonding**  
Valens are not enemies to defeat — they are beings with internal emotional states. Players earn trust by responding to a creature's needs, not by overpowering them.

**Restoration Over Combat**  
The primary goal is healing and restoring, not fighting. Conflict exists but is resolved through understanding rather than force.

**Low Barrier, High Depth**  
Easy to pick up (minimal tutorial text, intuitive controls), but with enough systems to reward long-term play.

---

## 3. Narrative Overview

### World
Prism Vale is a magical valley world divided into biomes, each representing a different emotional register — lush greens for calm, deep blues for sadness, fiery oranges for anger, pale greys for numbness. The world was once vibrant and balanced until **The Fracturing** — a massive traumatic event (left intentionally vague: natural disaster, dark magic, ancient conflict) that caused all living beings to fragment into their protective emotional parts. Everyone got stuck in fear and survival mode, draining the world's colour and magic. A creeping "Fade" — a desaturation of colour — has been spreading ever since.

### The Heart of the Vale
At the centre of the world sits the **Heart of the Vale** — a massive crystalline structure that once maintained balance across all biomes. It has been dormant and grey since The Fracturing. Restoring it is the campaign's ultimate goal and the Act 3 climax.

### Player Characters
Players choose from three selectable Keeper characters — semi-abstract, mannequin-style designs with ambiguous gender. All three play identically; the choice is personal/cosmetic.

- **River** *(masculine lean)* — water theme, flowing, adaptable
- **Ember** *(feminine lean)* — fire theme, warm, passionate
- **Sage** *(neutral)* — earth/plant theme, grounded, wise

Pronouns are selected separately from character choice. Creatures in the world address the player by their chosen name.

### Story Arc (high level)
- **Act 1:** Arrive in Prism Vale. Meet first Valen. Learn the colour language. Restore a small area.
- **Act 2:** The Fade grows. Discover its source is tied to a creature in deep emotional pain. Explore multiple biomes, bond with multiple Valens.
- **Act 3:** Confront the source of the Fade — not through combat, but through understanding. Restore the Heart of the Vale in a climactic restoration sequence.
- **Post-game:** Once the story is complete the world remains fully open. Focus shifts to building, deeper creature bonds, levelling companions, and completing the world's full restoration.

### IFS Integration (Internal Family Systems)
Each Valen represents an "inner part" archetype from IFS therapy — Protectors, Exiles, Firefighters, the Self. The player's journey of bonding with Valens mirrors the IFS process of getting to know and accepting your own inner parts. This is embedded in the mechanics, not explained through dialogue.

---

## 4. Gameplay Systems

### 4.1 Movement & Exploration
- Third-person, camera-relative movement
- Walking, running, jumping, gliding (unlocked mid-game via Valen bond)
- Semi-open world: large explorable regions connected by gates that unlock as story progresses
- No fall damage (child-friendly), but hazard zones that reset player position

### 4.2 Colour Language System
- Creatures and environment objects have an `EmotionalState` (enum: Calm, Fearful, Angry, Sad, Numb, Joyful)
- State is expressed via material colour, particle effects, ambient sound pitch
- Player learns to identify states through play, not tutorials

### 4.3 Valen Bonding
- Approach a Valen → it displays its emotional state via colour
- Player uses a contextual "respond" action (offer item, make sound, stand still, etc.) matched to the state
- Correct responses build a Trust meter
- At full trust, the Valen bonds — grants player a new ability and can be called as a companion

### 4.4 Restoration
- Bonded Valens can be used to restore Faded areas
- Restoration is a short animation — colour blooms outward from the restored point
- Unlocks new paths, items, and creatures

### 4.5 Items & Crafting (later milestones)
- Colour Essences — collectibles gathered from restored areas
- Used to craft "resonance tools" (attract certain creatures, soothe certain states)
- No complex crafting trees — keep it accessible

### 4.6 Progression
- No XP or levelling system
- Progression is story-gated: new areas unlock as Valens are bonded
- Player abilities expand through bonds, not stats
- Each Valen bond unlocks a traversal ability (e.g. eagle bond → glide/fly, aquatic Valen → swim deeper)

### 4.7 Mirror Mechanic
Before the player can successfully help a creature in emotional distress, they must first centre themselves. A simple internal balance minigame — matching colours or stabilising a visual state — represents the player regulating their own emotions before projecting outward. If the player's internal state is chaotic, bonding attempts fail. This is the core IFS concept of "leading from Self" embedded directly into the loop.

### 4.8 Forced Wellness Break
A built-in daily ritual — a short 30–60 second evening check-in when the in-game day ends. The player selects a colour representing how they feel. This is logged over time and shown back as a mood history. The game gently encourages stopping rather than endlessly playing. Skipping is allowed but the world softly responds (creatures are sleepy, colours are dimmer at night).

---

## 5. Creatures — Valens

### Design Principles
- Each Valen has a dominant emotional colour and a resting state
- Designs should feel soft, slightly abstract — not realistic animals, not generic fantasy creatures
- Inspiration: Capybara Games' creatures, Studio Ghibli creatures, Pokémon early gen

### Milestone 1 Creature (Patrol Enemy placeholder)
For M1, this is just a placeholder patrol entity — no bonding, no colour system. A simple creature-shaped mesh.

### Core Valen Roster (full game, design later)
To be designed — one per biome, one per IFS archetype. Rough archetypes:
- **The Wanderer** (Protector) — skittish, moves fast, green/teal
- **The Tender** (Self energy) — calm, approaches player first, warm gold
- **The Warden** (Firefighter) — aggressive when threatened, deep red/orange
- **The Hollow** (Exile) — desaturated, hidden, needs the most patience to bond

---

## 6. World & Biomes

### Prism Vale (Hub)
The central valley — starting area. Mixed greens and golds, gentle terrain. Acts as home base.

### The Ember Fields *(Anger biome)*
Warm reds and oranges. Cracked earth, geothermal vents. Wardens roam here.

### The Still Lake *(Sadness biome)*
Deep blues and purples. Foggy, slow ambient sound. The Hollow is found here.

### The Canopy *(Joy/Calm biome)*
Bright greens and yellows. Tree canopy exploration. The Wanderer lives here.

### The Fade Boundary *(Act 2–3)*
Desaturated grey. The creeping edge of the Fade. No creatures, eerie silence.

---

## 7. Visual Style

- **Rendering:** Stylised, not realistic. Aim for a soft, painterly look using Godot's environment settings + custom shaders
- **Colour palette:** Saturated but not garish. Reference: Ori and the Blind Forest, Spiritfarer, Palia
- **Character style:** Semi-chibi proportions. Expressive faces, minimal facial animation needed
- **UI style:** Minimal, diegetic where possible. Health/awareness shown through world elements rather than HUD bars
- **Lighting:** Baked GI where possible for performance; dynamic lights for emotional colour effects

---

## 8. Audio Direction

- **Music:** Original compositions (Cubase). Adaptive — tempo and instrumentation shift with emotional state of area
- **SFX:** Soft, organic sounds. Avoid hard digital sounds
- **Voice:** None. Creatures communicate with non-verbal vocalisations only
- **Ambience:** Every biome has a unique ambient layer

---

## 9. Platforms & Technical Targets

| Target | Spec |
|--------|------|
| PC (primary dev) | Windows 10+, mid-range GPU |
| Nintendo Switch | 720p handheld, 1080p docked — requires performance budget discipline |
| Engine | Godot 4 |
| Language | GDScript |
| Input | Keyboard/mouse + gamepad (Switch Joy-Con compatible) |

---

## 10. Milestone Roadmap (high level)

| Milestone | Focus | Status |
|-----------|-------|--------|
| M1 | Playable first level, player controller, one patrol enemy | 🔵 In Progress |
| M2 | Colour language system, first Valen encounter, basic bonding | ⬜ Planned |
| M3 | Full Level 1 biome, restoration mechanic, collectibles | ⬜ Planned |
| M4 | Second biome, second Valen, ability unlock | ⬜ Planned |
| M5 | Story scaffolding, main menu, save/load | ⬜ Planned |
| Demo | Polished Level 1 + Level 2, 30–45 min experience | ⬜ Planned |
| v1.0 | Full game — all biomes, all Valens, complete story | ⬜ Future |

---

*This document is the canonical reference for what Prism Vale is. All milestone docs, asset briefs, and code specs should align with the pillars and systems described here. Update this doc when major design decisions change.*