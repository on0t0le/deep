# Endless Descent — Architecture Specification

## Vision

A relaxing single-player browser game inspired by Journey and Endless Ocean.

The player pilots a tiny submarine and descends deeper into an endless-looking procedural ocean.

There is no combat, no inventory, no crafting, no money, no XP, and no base building.

The core emotion is curiosity.

The game should make the player constantly think:

> "I wonder what is deeper."

Although the ocean initially appears infinite, it actually has a final depth and a meaningful ending. There is no New Game+.

---

# Technical Constraints

* Single self-contained HTML file.
* All CSS and JavaScript embedded.
* No external libraries.
* No external assets.
* Everything generated procedurally.
* Internal render resolution: 480×270.
* Upscaled to screen using nearest-neighbor pixel scaling.
* Tiny memory footprint.
* Fast startup with no loading screens.

```css
canvas {
    image-rendering: pixelated;
}
```

---

# Visual Style

Simple 2D pixel art.

Dark atmospheric colors.

Inspired by:

* Journey
* Endless Ocean
* Ecco the Dolphin
* Proteus
* Rain World

Everything is generated.

No spritesheets.

No downloaded textures.

---

# Camera

Side-view.

The submarine is always near the center.

The world scrolls around the player.

---

# World

Infinite-looking vertical world.

Chunks are 16×16 tiles.

Only nearby chunks are loaded.

Far chunks are removed using LRU cache.

Chunks are deterministic.

World generation depends on:

```javascript
worldSeed
chunkX
chunkY
```

The same seed always produces the same world.

---

# Generation

Use:

* Value noise
* fBm

Generate:

* Water
* Rock formations
* Caves
* Corals
* Ruins
* Thermal vents
* Creature spawning regions

Traversal is always possible.

No dead ends.

Every chunk guarantees open passages upward and downward.

---

# Biomes

## Surface

0-100m

* Bright
* Coral reefs
* Schools of fish

---

## Twilight

100-1000m

* Blue colors
* Less light
* Larger creatures

---

## Midnight

1000-4000m

* Darkness
* Bioluminescence

---

## Abyss

4000-10000m

* Strange geometry
* Ancient ruins

---

## Beyond

10000m+

* Surreal
* Impossible lifeforms
* Physics becomes dreamlike

---

# Rendering

Simple tile rendering.

No particles required initially.

Pixel art.

Everything generated from typed arrays.

Procedural textures:

* Rock
* Sand
* Coral
* Cracks
* Algae
* Glow effects

---

# Controls

WASD

* W = ascend
* S = descend
* A = left
* D = right

Space

Activate sonar.

Shift

Boost.

Esc

Pause menu.

---

# Sonar

Core mechanic.

Visibility is intentionally limited.

Press Space:

Expanding circle reveals:

* Terrain
* Creatures
* Ruins
* Caverns

Objects fade back into darkness.

The player should rely on sonar instead of full visibility.

---

# UI Philosophy

The world is the UI.

Almost nothing is permanently visible.

No:

* Health bar
* XP bar
* Inventory
* Minimap
* Compass
* Quest markers

---

# Permanent UI

Only depth:

```text
842m
```

Bottom-right corner.

---

# Temporary UI

Discovery messages:

```text
Glass Coral

Discovered
```

Fade after a few seconds.

---

# Journal

Accessible only while surfaced.

Contains:

```text
✓ Small Jellyfish
✓ Coral Arch

???
???
???
```

Unknown entries remain hidden.

The total number of discoveries is never shown.

---

# Oxygen

Represented by bubbles.

No numbers.

Many bubbles:

○ ○ ○ ○ ○

Few bubbles:

○ ○

Critical:

○

---

# Save System

Autosave.

No save slots.

Save:

* worldSeed
* discoveries
* maxDepth
* submarine upgrades

---

# Creatures

Procedurally generated.

Parameters:

```javascript
{
 bodyLength,
 tailLength,
 finCount,
 glowColor
}
```

Peaceful.

No combat.

No aggression.

Creatures are meant to inspire curiosity.

---

# Discoveries

Examples:

* Glass Coral
* Giant Ray
* White Whale
* Singing Trench
* Crystal Forest
* Living Island
* Endless Chasm
* Black Sun

Rare discoveries are not announced beforehand.

---

# Progression Philosophy

No money.

No XP.

No crafting.

No grinding.

Progress comes from restoration of an old submarine.

---

# Surface Boat

Between dives the player returns to a tiny boat.

Inside:

* Maps
* Old journals
* Workshop
* Photos
* Specimen shelf

No menus.

Everything should feel physical.

---

# Submarine Systems

Only five systems exist.

## Hull

Pressure resistance.

Mk I

500m

Mk II

1500m

Mk III

4000m

Mk IV

9000m

Mk V

20000m

---

## Oxygen

Allows longer dives.

---

## Lights

Different wavelengths reveal hidden life.

Blue.

Red.

UV.

---

## Sonar

Increased range.

Better cave detection.

Reveals hidden structures.

---

## Propeller

Allows travel through stronger currents.

Not speed upgrades.

---

# Upgrade Philosophy

Upgrades are never purchased.

Upgrades are unlocked by discoveries.

Example:

At 1200m:

Player discovers abandoned research pod.

Finds:

Pressure Valve Blueprint.

Returns safely.

Hull Mk II becomes available.

---

At 4000m:

Finds sonar crystal.

Long-range sonar unlocked.

---

At 8000m:

Finds bioluminescent bacteria.

Efficient lighting unlocked.

---

At 15000m:

Finds unknown alloy.

Hull Mk V unlocked.

---

# Risk

Progress requires returning alive.

Discoveries are only secured after reaching the surface.

Greed is dangerous.

The player constantly chooses:

Continue deeper?

Or return safely?

---

# Player Motivation

Short-term:

Return alive.

Medium-term:

Complete the journal.

Long-term:

Discover what lies beneath.

---

# Story Structure

Stage 1

Beauty.

Surface life.

---

Stage 2

Mystery.

Ruins.

Strange creatures.

---

Stage 3

Impossible.

Dreamlike phenomena.

---

Stage 4

Evidence that others descended before.

Abandoned submarines.

Old logs.

Broken stations.

---

Stage 5

Final descent.

The ocean has a bottom.

No boss.

No combat.

No explanation.

Just arrival.

---

# Endgame

The player reaches the bottom.

The game ends.

No New Game+.

No infinite progression.

No replay loop.

The journey itself is the reward.

---

# Engine Architecture

```text
Game
│
├── Renderer
├── Input
├── ChunkManager
├── SaveSystem
│
├── TerrainGenerator
├── TextureGenerator
├── CreatureGenerator
├── BiomeGenerator
│
├── SonarSystem
├── DiscoverySystem
├── UpgradeSystem
│
├── SurfaceBoat
├── Journal
│
└── UI
     ├── DepthText
     ├── OxygenBubbles
     └── DiscoveryPopup
```

---

# Development Order

Phase 1

* Rendering
* Movement
* Chunk loading
* Infinite ocean

Phase 2

* Biomes
* Sonar
* Darkness

Phase 3

* Creature generator
* Procedural textures

Phase 4

* Discoveries
* Journal

Phase 5

* Surface boat
* Upgrade system

Phase 6

* Story layers
* Rare phenomena

Phase 7

* Final depth
* Ending

