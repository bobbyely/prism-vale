# Prism Vale — Human Asset Checklist
**Owner:** Rob  
**Version:** 0.2  
**Last Updated:** February 2026  
**Purpose:** Practical briefs for every asset Rob needs to create. Each item includes what to make, to what spec, and which tools to use.

---

## How to Use This Document

Each section covers a category of assets. Items are ordered by milestone priority. Use the checkboxes to track progress. Each item includes:
- **Brief** — what it needs to look/be like
- **Spec** — technical requirements (poly count, texture size, rig, animations)
- **Tools** — what software to use
- **Export** — how to get it into Godot
- **Notes** — gotchas, tips, references

---

## 🔧 Character Asset Pipeline

This is the standard workflow for all humanoid characters (Keeper variants, NPCs). Creature/Valen pipeline differs — see Valen section.

**Step 1 — Concept Art**
Create or source concept art. Works best as a clean front-facing or ¾ view with clear silhouette and no heavy overlapping elements. This is your Meshy input.

**Step 2 — Meshy (AI mesh generation)**
Upload concept art to Meshy (meshy.ai). Download the output mesh. Expect a dense triangle soup — that's fine, it's the starting point not the final asset.

**Step 3 — Blender cleanup**
- Run **Quad Remesher** (or free alternative QRemeshify) to convert triangles to clean quads
- Target poly count: 5,000–10,000 tris for hero characters, 3,000–6,000 for secondary
- Fix any n-gons (5+ sided faces — dissolve them into quads or tris)
- Ensure mesh is symmetrical — apply Mirror modifier if needed
- **Get the mesh into a clean T-pose** — Mixamo's auto-rigger requires this. Arms out at ~45°, feet flat, fingers relaxed. If Meshy outputs a different pose, correct it manually in pose mode.
- Apply all modifiers and transforms (Ctrl+A → All Transforms) before export

**Step 4 — Mixamo auto-rig**
- Export mesh from Blender as FBX
- Upload to mixamo.com → auto-rig
- Place the rig markers carefully (chin, wrists, elbows, knees, groin) — take your time here, bad marker placement = bad deformations
- **Test several animations in the Mixamo preview before downloading anything** — if shoulders or hips look wrong, go back and adjust markers
- Once happy with the rig, download the rigged character

**Step 5 — Download animations from Mixamo**
- Select each animation → download as **FBX Binary, With Skin** every time
- Do NOT use Without Skin — causes rest pose mismatch and broken keyframes in Godot
- For locomotion (walk, run): tick **In Place** to strip root motion
- Download each animation as a separate FBX file

**Step 6 — Into Godot via AnimationLibrary**
Rather than combining animations in Blender (complex, error-prone), import each FBX separately into Godot and use AnimationLibrary to combine them at the engine level:
- Import each FBX → Godot auto-generates a `.glb` equivalent
- In your player scene, add an **AnimationPlayer** node
- Add an **AnimationLibrary** resource
- Drag each animation clip from each imported file into the library
- All animations are now available to AnimationTree from one library

**Step 7 — Custom animations (added later)**
Mixamo only covers standard humanoid animations. Custom animations (Keeper interacting with Valens, emotes, ability animations) are added later in Blender and imported the same way — create the action in Blender on the same armature, export as GLB, import into Godot, add to the existing AnimationLibrary. Non-destructive, can be done at any milestone.

---

## 🧍 Characters

### [ ] M1 — Placeholder Player Character

**Brief:**  
A humanoid character to serve as the player stand-in during development. Does not need to match the final character design. Should look intentionally placeholder so you're not tempted to over-polish it.

**Recommended approach:** Download a free Mixamo character (Y Bot or X Bot) directly — skip concept art and Meshy for the placeholder. Save the full pipeline for the real Keeper character.

**Spec:**
- Humanoid rig (standard biped — hips, spine, chest, head, shoulders, arms, hands, legs, feet)
- Mesh: ~3,000–8,000 triangles (low poly is fine)
- Single material / flat colour — no textures needed for placeholder
- Skeleton must be compatible with Godot's `AnimationTree` (Mixamo exports work fine)

**Animations required for M1:**
- [ ] Idle (standing still, subtle breathing loop, ~60–90 frames)
- [ ] Walk (looping, ~30 frames)
- [ ] Run (looping, faster cycle, ~20 frames)
- [ ] Jump (one-shot: anticipation → rise → peak → fall, ~30 frames)
- [ ] Land (one-shot: impact pose → recover, ~15 frames)

**Tools:** Mixamo (mixamo.com)

**Export:**
1. On Mixamo: select Y Bot → select each animation → tick **In Place** for Walk/Run/Idle → download as **FBX Binary, With Skin** for every animation
2. Import each FBX directly into Godot (drag into FileSystem panel)
3. In player scene: AnimationPlayer → AnimationLibrary → add each clip
4. Place character GLB at `res://assets/models/characters/player_placeholder/`

**Notes:**
- Download With Skin for every animation — this was the source of the keyframe issues encountered during development
- Y Bot is cleaner than X Bot for Godot imports
- GLB imported via Godot avoids the FBX scale issues (0.01 scale problem)

---

### [ ] M1 — Placeholder Enemy Character

**Brief:**  
A simple creature-shaped placeholder for the patrol enemy. Can literally be a re-coloured version of the player placeholder, or a basic primitive mesh. The goal is just having something visible in the scene.

**Spec:**
- Any humanoid or creature shape — even a capsule with eyes is fine for M1
- No animations required for M1 (static mesh is acceptable)
- Distinct colour from player (suggest red/orange material) so it reads clearly

**Tools:** Blender (basic primitive mesh) or reuse Mixamo character with different material

**Export:** `.glb`, place at `res://assets/models/characters/enemy_placeholder.glb`

---

### [ ] M3+ — Keeper (Final Player Characters: River, Ember, Sage)

**Brief:**  
Three selectable Keeper characters — River (masculine lean), Ember (feminine lean), Sage (neutral). Semi-abstract, mannequin-style designs with soft stylised proportions. Should read clearly at Switch handheld size. Expressive silhouette. Clothing has colour-reactive elements (small gems/patches) that tie into the colour language system. All three share the same rig and animations — only mesh and textures differ.

**Pipeline:** Concept art → Meshy → Blender cleanup → Mixamo rig → Mixamo animations → Godot AnimationLibrary (see Character Asset Pipeline section above)

**Spec:**
- Poly count: 5,000–10,000 triangles per character (Switch budget)
- Shared rig — all three characters use identical bone structure so animations are interchangeable
- Textures per character:
  - 1x 2048×2048 **albedo** (greyscale/neutral — light and shadow detail only, no colour)
  - 1x 1024×1024 **zone mask** (flat solid colours per region: pure red = skin, pure green = clothing, pure blue = boots/accessories, pure white = emissive elements)
  - 1x 512×512 **normal map**
  - 1x 512×512 **emissive map** (for gems/reactive elements only)
- Vertex groups for rig deformation only (not used for shader zones — mask texture handles that)
- Blendshapes: Smile, Surprise, Sad (minimum — for future cutscene use)

**Zone mask painting guide:**
- Paint in Krita or Photoshop at full 1024×1024 resolution
- Use hard edges between zones — no gradients or feathering at boundaries
- Regions: skin (face, hands, neck), primary clothing (shirt/tunic), secondary clothing (trousers/skirt), footwear, hair, accessories/gems
- Each region gets a unique solid colour that the fade shader reads as a mask

**Animations required (via Mixamo + custom):**
Mixamo sourced:
- [ ] Idle
- [ ] Walk (In Place)
- [ ] Run (In Place)
- [ ] Jump
- [ ] Land
- [ ] Fall (loop)
- [ ] Sit

Custom (added in Blender, later milestones):
- [ ] Interact / reach forward
- [ ] Glide (arms out, slow fall)
- [ ] Celebrate / emote
- [ ] Valen bond reaction

**Tools:** Concept art (Krita/Photoshop) → Meshy → Blender (cleanup, T-pose) → Mixamo (rig + animations) → Godot AnimationLibrary

---

### [ ] M4+ — First Valen: The Wanderer

**Brief:**  
A small, skittish quadruped creature. Think somewhere between a deer fawn and a crested lizard — delicate limbs, large eyes, a small luminous frill along its neck that shifts from teal (calm) to pale yellow (curious) to grey (fearful). Soft forms, no sharp edges.

**Pipeline:** Concept art → Meshy → Blender cleanup → **custom rig in Blender** (Mixamo cannot rig quadrupeds) → hand-animate in Blender → export GLB → Godot AnimationLibrary

**Spec:**
- Poly count: 3,000–6,000 triangles
- Textures:
  - 1x 1024×1024 albedo (greyscale/neutral base)
  - 1x 512×512 zone mask (body regions for fade shader tinting)
  - 1x 512×512 emissive (frill only — this is what the colour language system controls at runtime)
- Custom creature rig (not biped — quadruped spine, tail bones, separate frill bones)
- Blendshapes for frill puff (relaxed → fully extended)

**Animations required (all hand-animated in Blender):**
- [ ] Idle (subtle breathing + ear flick, loop)
- [ ] Walk (quadruped gait, loop)
- [ ] Run / Flee (faster gait, loop)
- [ ] Alert (head up, ears forward, freeze, one-shot)
- [ ] Approach (slow, curious walk, loop)
- [ ] Bond reaction (joyful wiggle / spin, one-shot)

**Tools:** Blender (all steps — no Mixamo for creatures)

**Notes:**
- Quadruped rigging in Blender is more complex — use a Rigify quadruped template or reference Blender's animal rig tutorials
- The frill emissive map is what the colour language system controls at runtime via GDScript material parameters — plan the UV layout to isolate the frill region clearly
- Custom animations are added to Godot AnimationLibrary the same way as humanoid animations

---

## 🌍 Environment & Level Assets

### [ ] M1 — Level 1 Terrain

**Brief:**  
A simple valley corridor — the geometry for the first playable level. Functional over beautiful for M1. Should have: a start zone, an open middle area, an elevated section for jump testing, and a clearly defined end zone.

**Spec:**
- Can be built directly in Godot using CSG nodes (quickest for M1) or as a simple Blender mesh
- If Blender: single mesh, no higher than 10,000 triangles for the base terrain
- Texture: can use a single tiling grass/dirt texture from Polyhaven (free, CC0)
- Must have clear walkable area — flat enough that the player doesn't slide

**Recommended for M1:** Use Godot's CSGBox and CSGCombiner nodes to rough-block the level geometry first. No Blender needed for M1 terrain. Replace with real mesh in M2/M3.

**Checklist:**
- [ ] Start zone (flat open area, ~10m × 10m)
- [ ] Middle corridor with patrol area (~20m × 10m)
- [ ] Elevated platform requiring jump (~1.5m high, reachable)
- [ ] End zone with goal marker placement
- [ ] Edge blocking (cliffs, invisible walls, or natural barriers)
- [ ] Void zone below level (triggers reset — just an Area3D at bottom of map)

---

### [ ] M1 — Basic Props (3 minimum)

**Brief:**  
Simple environmental props to break up the empty level and give the patrol enemy something to walk around. M1 quality — basic shapes are fine.

**Spec per prop:**
- Under 500 triangles each
- Single material, simple texture or flat colour
- No rigging or animation

**Minimum set for M1:**
- [ ] Rock / boulder (1–3 size variants from same mesh, scaled in Godot)
- [ ] Simple tree or bush (use a flat plane with alpha texture for leaves — efficient)
- [ ] Wooden crate or barrel (something clearly manmade for visual contrast)

**Tools:** Blender

**Export:** Individual `.glb` files, placed at `res://assets/models/props/`

---

### [ ] M2+ — Level 1 Full Environment (Prism Vale Hub)

**Brief:**  
Replace the M1 grey-box level with a proper designed environment matching the Prism Vale hub biome visual style — soft greens and golds, gentle valley terrain, warm lighting.

*(Full brief to be written when M1 is complete and scope is clearer)*

---

## 🎨 Textures & Materials

### [ ] M2+ — Fade Shader (Claude Code builds, Rob provides inputs)

The core visual system for Prism Vale. Claude Code writes the shader — Rob needs to ensure all assets are built to support it.

**What the shader does:**
- Lerps each asset between greyscale and full colour based on a `colour_amount` value (0.0 = fully grey, 1.0 = fully restored)
- Reads a **zone mask texture** to apply separate tints per body/asset region
- Supports a **fill-from-bottom** effect — colour rises upward from world Y position during restoration animations
- Emissive elements can stay partially coloured even in Faded state (makes creatures visible and inviting in grey areas)

**Rob's responsibilities for shader compatibility:**

Every character and creature asset needs:
- [ ] **Albedo texture** — greyscale/neutral only. Light, shadow, surface detail. No colour baked in.
- [ ] **Zone mask texture** — flat solid colours per region (pure red, green, blue, white etc). Hard edges between zones, no gradients.
- [ ] **Emissive map** — only for elements that glow or react (Valen frills, gems, magical elements)

Every environment asset needs:
- [ ] Albedo texture (can have subtle colour — terrain and props are less critical than characters)
- [ ] Zone mask only if the asset has multiple distinct colour regions

**Zone mask colour conventions (use consistently across all assets):**
- Pure Red `(1,0,0)` — skin / organic surface
- Pure Green `(0,1,0)` — primary clothing
- Pure Blue `(0,0,1)` — secondary clothing / footwear
- Pure White `(1,1,1)` — emissive / magical elements
- Pure Yellow `(1,1,0)` — accessories / hair
- Black `(0,0,0)` — unaffected / uses raw albedo

---

### [ ] M1 — Terrain texture (grass/dirt)

**Source:** Polyhaven.com — search "grass" or "meadow". Download at 2K resolution, PBR set (albedo, normal, roughness).

**Setup in Godot:**
- Create a `StandardMaterial3D` on terrain mesh
- Assign albedo + normal textures
- Set UV scale to tile across large terrain (~8–16x repeat)

**Checklist:**
- [ ] Downloaded from Polyhaven (CC0 license — no attribution required but good practice)
- [ ] Imported into Godot
- [ ] Applied to terrain with tiling set

---

### [ ] M2+ — Biome material library

Each biome needs a small material set: ground, rock, foliage, water surface. To be built per biome as needed.

---

## 🖼️ UI & 2D Art

### [ ] M1 — HUD placeholder

Claude Code will build the HUD scene in Godot. You only need to provide:
- [ ] A simple icon or shape for the awareness indicator (can be a coloured circle — purely code-generated for M1)

No 2D art required from Rob for M1 HUD.

---

### [ ] M3+ — UI visual design

**Brief:**  
Minimal, organic UI style. Think hand-drawn borders, soft rounded elements. No hard rectangles or sharp corners. Should feel like it's part of the world, not pasted on top.

**Assets needed (future):**
- [ ] Health/stamina bar (organic shape, ~512×64px)
- [ ] Inventory slot (rounded square, ~128×128px)
- [ ] Dialogue bubble / awareness indicator styled
- [ ] Main menu background illustration
- [ ] Font selection (Google Fonts or similar — suggest something rounded and readable)

**Tools:** Krita or Photoshop (export PNG with transparency)

---

### [ ] M5+ — Main Menu Art

**Brief:**  
An establishing illustration of Prism Vale — the valley, warm light, a creature silhouette in the distance. Used as the main menu background.

*(Full brief when needed)*

---

## 🎵 Audio

### [ ] M2+ — Hub Biome Ambience

**Brief:**  
A looping ambient audio bed for the Prism Vale hub area. Should feel calm, warm, slightly magical. No melody — texture and atmosphere only.

**Spec:**
- Length: 60–120 seconds (loops seamlessly)
- Format: `.ogg` (Godot's preferred audio format for streaming)
- Mix: low mid-range texture, gentle high-frequency detail, no sudden transients

**Tools:** Cubase

---

### [ ] M3+ — Hub Biome Music

**Brief:**  
Adaptive music system for the hub. Needs at minimum two layers that can be crossfaded:
- [ ] Exploration layer (gentle, open, curious)
- [ ] Valen-nearby layer (adds warmth, subtle harmonic lift)

**Spec:**
- Stems exported separately as `.ogg`
- Same BPM, designed to loop and crossfade seamlessly
- Length: 2–4 minutes per loop

---

### [ ] M1 — Placeholder SFX (minimal)

These can be sourced free for M1 — no original composition needed yet.

- [ ] Footstep sounds (grass) — source from freesound.org or Kenney.nl
- [ ] Jump whoosh — source free
- [ ] Land impact — source free
- [ ] Enemy alert sound (beep or tone) — can be generated in Audacity or sourced free

**Import format:** `.wav` for short SFX (low latency), `.ogg` for longer clips

---

## 📸 Reference & Mood Board

*(Not a deliverable — just a recommendation)*

Before modelling anything, build a reference board in PureRef covering:
- [ ] Character style references (3–5 games with similar aesthetic)
- [ ] Creature design references (Studio Ghibli, Pokémon gen 1–3, Capybara Games)
- [ ] Environment references (Ori and the Blind Forest, Palia, Spiritfarer)
- [ ] Colour palette reference — pick 5–6 hex colours for the hub biome before texturing

---

## 📦 Export Cheat Sheet

| Asset Type | Format | Location in Godot |
|-----------|--------|-------------------|
| Characters | `.glb` | `res://assets/models/characters/` |
| Props | `.glb` | `res://assets/models/props/` |
| Terrain | `.glb` or CSG | `res://assets/models/environment/` |
| Textures | `.png` (albedo, normal) | `res://assets/textures/` |
| UI art | `.png` with transparency | `res://assets/ui/` |
| Music | `.ogg` | `res://assets/audio/music/` |
| SFX | `.wav` or `.ogg` | `res://assets/audio/sfx/` |

---

## ✅ M1 Summary Checklist (Rob's minimum to unblock Claude Code)

These are the only things Claude Code is waiting on from you to complete Milestone 1:

- [ ] `player_placeholder.glb` — Mixamo character with Idle, Walk, Run, Jump, Land animations
- [ ] `enemy_placeholder.glb` — any visible mesh, no animations required
- [ ] Level geometry placed in `level_01.tscn` (can use Godot CSG blocks — no Blender needed)
- [ ] Two `Marker3D` nodes placed in scene for enemy patrol waypoints
- [ ] One `Marker3D` placed for player spawn point
- [ ] Rough terrain texture applied (optional but makes testing more pleasant)

Once those are in place, Claude Code can wire everything together.