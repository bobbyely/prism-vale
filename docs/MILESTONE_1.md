# Prism Vale — Milestone 1 Design Document
**Version:** 0.1  
**Last Updated:** February 2026  
**Engine:** Godot 4 (GDScript)  
**Repository:** GitHub  

---

## 1. Project Overview

**Prism Vale** is a cozy, semi-open-world adventure game for PC and Nintendo Switch. The core concept blends creature bonding mechanics with Internal Family Systems (IFS) therapy principles — players interact with creatures whose emotional states are communicated through colour, teaching empathy and emotional literacy through play.

Target audience: Children and adults (roughly 7+). Minimal reading required; visual/audio cues drive communication.

---

## 2. Milestone 1 — Scope

**Goal:** A playable first level with a controllable player, a navigable environment, and one patrolling enemy. This is a technical foundation milestone, not a content milestone. Art and mechanics can be rough/placeholder.

### ✅ In Scope
- Third-person player controller (move, jump, camera)
- A first level: terrain, basic geometry, walkable paths
- One enemy with simple patrol AI (A → B → A loop)
- Player–enemy collision/detection (player can "see" enemy, enemy has awareness zone)
- Basic HUD placeholder (health bar or awareness indicator)
- Scene management (start game, restart on death/reset trigger)
- Git repo structure established and documented

### ❌ Explicitly Out of Scope (save for later milestones)
- Creature bonding / IFS mechanics
- Inventory, crafting, building
- Dialogue or story elements
- Multiplayer
- Switch-specific input handling (controller support can wait)
- Polished art, animations, or audio
- Save/load system

---

## 3. Tech Stack

| Tool | Purpose | Who Uses It |
|------|---------|-------------|
| Godot 4 | Game engine | Both |
| GDScript | Programming language | Claude Code primarily |
| Blender | 3D modelling, rigging | Human (Rob) |
| GitHub | Version control | Both |
| GitHub Issues | Task tracking | Both |
| Mixamo | Free placeholder character + animations | Human (Rob) |
| Polyhaven | Free textures/HDRIs | Human (Rob) |
| Krita / Photoshop | 2D art, UI mockups | Human (Rob) |
| Cubase | Music/audio (later milestones) | Human (Rob) |

---

## 4. Player Controller Spec

### Character
- Placeholder: Use a free Mixamo character (e.g. "Y Bot" or similar) exported as `.glb` with animations
- Animations needed for M1: Idle, Walk, Run, Jump, Fall

### Movement
- WASD / arrow keys to move
- Camera-relative movement (player moves in the direction the camera faces)
- Shift to run (optional toggle, can be added late)
- Spacebar to jump (single jump only for M1)
- Coyote time: ~0.1s grace period after walking off an edge

### Camera
- Third-person follow camera, orbits player with mouse
- Camera collision: basic raycast to prevent clipping through walls
- Default distance: ~5m behind and ~2m above player
- Mouse sensitivity: configurable constant in a `GameConfig.gd` autoload

### Physics
- Use Godot's `CharacterBody3D`
- Gravity applied manually in `_physics_process`
- Move and slide for collision handling

---

## 5. Level 1 Spec

### Layout Concept
A simple valley corridor — wide enough to explore but guided enough to encounter the patrol enemy. Think: enter from one end, explore a rough path, reach a "goal zone" at the other end. No puzzle-solving needed, just navigation.

**Rough structure:**
```
[START ZONE] → [OPEN AREA with patrol enemy] → [GOAL MARKER]
```

### Environment
- Terrain: Godot's built-in terrain or simple mesh from Blender (flat-ish valley, some rocks/props)
- Blocking volumes at level edges (invisible walls or natural barriers like cliffs)
- One elevated section requiring a jump (tests jump mechanic)
- A clearly marked "goal" object (glowing orb placeholder, simple OmniLight3D)

### Assets for M1 (placeholder-quality is fine)
- Ground/terrain mesh
- 2–3 rock/obstacle props (can reuse same mesh, varied scale)
- One tree or structure for visual reference
- Enemy mesh (use same Mixamo base, different material colour)

---

## 6. Enemy Spec — Patrol AI

### Behaviour
Simple two-point patrol loop. No pathfinding required for M1 — direct movement between two `Marker3D` nodes placed in the scene.

**State machine (3 states):**
```
PATROL → (player enters detection radius) → ALERT → (player exits radius) → PATROL
```

- **PATROL:** Move toward next waypoint at `patrol_speed`. On arrival, wait `patrol_wait_time` seconds, then move to other waypoint.
- **ALERT:** Stop moving. Face player. Pulse enemy colour to red. Play alert sound (placeholder beep). No chasing in M1.
- **RESET:** If player exits radius, return to PATROL after `alert_cooldown` seconds.

### Parameters (defined as `@export` vars for easy tuning in Godot Inspector)
```gdscript
@export var patrol_speed: float = 2.0
@export var patrol_wait_time: float = 1.5
@export var detection_radius: float = 5.0
@export var alert_cooldown: float = 3.0
```

### Detection
- `Area3D` sphere with radius = `detection_radius`
- Connect `body_entered` and `body_exited` signals to handle state transitions

### Visual Feedback
- Enemy uses an `OmniLight3D` child node; colour shifts from neutral (white) → alert (red)
- No animation required for M1 (static mesh is fine)

---

## 7. HUD

Minimal placeholder only:

- **Enemy awareness indicator:** Simple `ColorRect` in the corner — grey when safe, red when enemy is alerted. Implemented as a singleton signal listener.
- No health system required for M1 unless you want a "fall into void = restart" mechanic (recommended — easy win).

---

## 8. Scene Structure (Godot)

```
res://
├── scenes/
│   ├── main_menu.tscn          # Simple "Play" button, M1 placeholder
│   ├── level_01.tscn           # The main playable scene
│   ├── player/
│   │   ├── player.tscn
│   │   └── player.gd
│   ├── enemy/
│   │   ├── patrol_enemy.tscn
│   │   └── patrol_enemy.gd
│   └── ui/
│       └── hud.tscn
├── scripts/
│   └── game_config.gd          # Autoload — global config values
├── assets/
│   ├── models/                 # .glb files from Blender/Mixamo
│   ├── textures/               # .png/.jpg from Polyhaven etc
│   └── audio/                  # placeholder sounds
├── addons/                     # Any Godot plugins
└── project.godot
```

---

## 9. Git Setup

### Repo Structure
- Repo name suggestion: `prism-vale`
- Default branch: `main`
- Add Godot's official `.gitignore` (covers `.godot/`, import cache, etc.)
- Add a `README.md` from day one

### Branching Strategy (keep it simple for solo dev)
```
main          ← stable, milestone-tagged releases
dev           ← active development branch
feature/xxx   ← short-lived feature branches, merge into dev
```

Tag releases: `v0.1-m1` when Milestone 1 is complete.

### What to Track vs Ignore
**Track:** All `.gd` scripts, `.tscn` scenes, `.tres` resources, small assets (.png, .wav under ~2MB), project.godot  
**Ignore:** `.godot/` folder, `*.import` files (auto-generated), large binary assets over ~10MB (use Git LFS for those)

### Git LFS (optional but recommended for game dev)
Large `.glb` models and audio files should use Git LFS:
```
git lfs install
git lfs track "*.glb"
git lfs track "*.wav"
git lfs track "*.mp3"
```

### GitHub Issues as Task Tracker
Use GitHub Issues with these labels:
- `claude-code` — tasks to hand off to Claude Code
- `human` — Rob only (art, modelling, music)
- `bug`, `enhancement`, `milestone-1`

---

## 10. Responsibilities Matrix

| Task | Owner |
|------|-------|
| Player controller GDScript | Claude Code |
| Enemy patrol AI GDScript | Claude Code |
| HUD GDScript + scene | Claude Code |
| Scene/project structure setup | Claude Code |
| State machine architecture | Claude Code |
| .gitignore, README, repo init | Claude Code |
| Player mesh + animations (Mixamo) | Rob |
| Level terrain + environment mesh | Rob |
| Prop models (rocks, trees) | Rob |
| Texturing assets | Rob |
| Blender → .glb export pipeline | Rob |
| Placing waypoints in level scene | Rob |
| Playtesting + feedback | Rob |
| Final art direction calls | Rob |

---

## 11. Timeline

Estimated at ~5–8 hours/week of combined effort. Adjust to your schedule.

### Phase 1 — Foundation (Week 1–2)
- [ ] Create GitHub repo, push initial Godot project
- [ ] Establish folder structure and `.gitignore`
- [ ] Claude Code: implement player controller (movement, jump, camera)
- [ ] Rob: source/export placeholder character from Mixamo
- [ ] Get player moving in an empty scene

**Checkpoint:** Player walks, runs, jumps in empty grey-box scene.

### Phase 2 — Level Build (Week 3–4)
- [ ] Rob: build Level 1 terrain and props in Blender, export to `.glb`
- [ ] Rob: place assets in `level_01.tscn` in Godot
- [ ] Claude Code: add fall-into-void restart logic
- [ ] Claude Code: scene management (main menu → level)
- [ ] Claude Code: basic HUD scene

**Checkpoint:** Player navigates a real level, dies on falling out of bounds.

### Phase 3 — Enemy (Week 5–6)
- [ ] Claude Code: patrol enemy state machine
- [ ] Claude Code: detection Area3D and signal wiring
- [ ] Claude Code: visual alert feedback (light colour change)
- [ ] Rob: place enemy and waypoints in level scene
- [ ] Rob: playtest and tweak `@export` parameters

**Checkpoint:** Enemy patrols, detects player, HUD reacts.

### Phase 4 — Polish & Tag (Week 7–8)
- [ ] Bug fixes from playtesting
- [ ] README updated with how to run the project
- [ ] Tag `v0.1-m1` on GitHub
- [ ] Write brief notes on what's next (Milestone 2 scope ideas)

**Milestone 1 Complete ✅**

---

## 12. Next Steps (Milestone 2 Preview)

Once M1 is done, likely candidates for M2:
- Basic creature encounter (approach, colour response, no bonding yet)
- Simple collectible system (colour essences)
- Ambient audio and music integration
- Expanded level with biome variation
- Controller/gamepad support

---

*This document should live at `docs/MILESTONE_1.md` in your repo and be updated as decisions change.*