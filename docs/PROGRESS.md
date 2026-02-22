# Prism Vale — Session Progress & Handoff
**Last Updated:** February 2026

---

## Current State

### Infrastructure ✅
- Git repo established at `prism-vale-test`
- Godot 4.6 project created at repo root (Forward+, Jolt Physics, D3D12)

### Assets ✅
- Mixamo character downloaded — custom character "River" (not Y Bot)
- 6 FBX files imported into Godot at `assets/models/test_models/River/`:
  - Idle, Walking, Running, Jump, Sweep Fall, Sitting Idle
- Each FBX downloaded separately (correct pipeline — one animation per file)
- **Confirm:** Were these downloaded "With Skin"? Critical for correct keyframes.

### Scripts — Not Yet Started
- `scenes/player/player.gd` — needs to be written (previous version from earlier repo may need rewrite for this project)
- No other scripts exist yet

### Docs ✅
- `docs/MILESTONE_1.md` — full M1 scope and spec
- `docs/HUMAN_ASSETS.md` v0.2 — Rob's asset pipeline and checklist
- `docs/PROJECT_DESIGN.md` — game design overview
- `docs/PROGRESS.md` — this file

---

## What Rob Has / Hasn't Done Yet

### Done
- Godot project created and open
- Folder structure created inside Godot FileSystem panel
- Mixamo FBX downloads (Idle, Walking, Running, Jump, Sweep Fall, Sitting Idle)
- FBX files imported into Godot

### Not Done Yet
- Player scene built in Godot
- Level geometry (CSG blocks or Blender mesh)
- Enemy placeholder mesh

---

## Next Session — Immediate Tasks

### Priority 1: Get a character moving in an empty scene
This is the Phase 1 checkpoint from the milestone doc.

**Steps in order:**

1. ~~**Download from Mixamo**~~ ✅ Done — River character, 6 animations

2. ~~**Import FBXs into Godot**~~ ✅ Done — at `assets/models/test_models/River/`

3. **Check what Godot named the animation clips**
   - Click one imported FBX in FileSystem
   - Look at the Scene tab — expand the AnimationPlayer
   - Note the exact clip names (might be "mixamo.com", "Armature|Walk", etc.)
   - Update the `const ANIM_*` lines at the top of `scenes/player/player.gd` to match

4. **Build the player scene** (`scenes/player/player.tscn`)
   - Create new scene, root node = CharacterBody3D, rename to "Player"
   - Add child: CollisionShape3D (CapsuleShape3D, height ~1.8, radius ~0.3)
   - Add child: Node3D, rename to "MeshRoot" — this rotates when player turns
     - Instance one of the imported FBX scenes as child of MeshRoot
   - Add child: AnimationPlayer (separate from the one in the FBX)
   - Add child: SpringArm3D
     - Add child: Camera3D
   - Attach `scenes/player/player.gd` to the CharacterBody3D root

5. **Update node paths in player.gd**
   - `@onready var anim_player` → path to your AnimationPlayer
   - `@onready var mesh_root` → path to MeshRoot (or whatever wraps the character)
   - `@onready var spring_arm` → path to SpringArm3D

6. **Set up AnimationLibrary**
   - Select AnimationPlayer → Inspector → Libraries → Add AnimationLibrary
   - Drag animation clips from the imported FBX files into the library
   - Clips should be accessible by the names set in `ANIM_*` constants

7. **Set up Input Map** (Project → Project Settings → Input Map)
   - `move_forward` → W
   - `move_back` → S
   - `move_left` → A
   - `move_right` → D
   - `jump` → Space
   - `sprint` → Left Shift

8. **Test scene**
   - Add a flat CSGBox below the player as a floor (scale ~20,20,1 rotated flat)
   - Add a WorldEnvironment node (so it's not pitch black)
   - Hit Play — player should spawn, camera follow, WASD move

---

## Key Technical Decisions Made

| Decision | Choice | Reason |
|---|---|---|
| Animation pipeline | Import each FBX separately into Godot, use AnimationLibrary | Simpler than combining in Blender NLA; less error-prone |
| FBX download setting | Always "With Skin" | "Without Skin" causes rest pose mismatch and broken keyframes |
| Character rotation | Rotate a MeshRoot Node3D, not CharacterBody3D | CharacterBody3D must stay unrotated; camera orbits independently |
| Camera | SpringArm3D child of CharacterBody3D | Handles collision automatically; rotates with mouse independently of mesh |
| Renderer | Forward+ | Best for PC/Switch target |

---

## Player Script — What It Does
`scenes/player/player.gd` is complete. Key behaviours:
- WASD moves relative to camera facing (not character facing)
- Character mesh smoothly rotates to face movement direction
- Shift sprints, Space jumps
- Coyote time: 0.1s grace period after leaving edge
- Animation states: Idle → Walk → Run → Jump → Fall → Land (one-shot) → Idle
- Mouse orbits SpringArm3D camera; Escape releases cursor, click recaptures
- All tunable values exposed as `@export` vars — tweak in Inspector without touching code

---

## What Claude Code Should NOT Do Next Session
- Do not create `project.godot` — Godot owns that file
- Do not create `.tscn` files by hand — build scenes in the Godot editor
- Do not reorganise the folder structure — it matches the Godot FileSystem
