# Inkworm Lab

A real-time WebGL2 simulator of marbled, cilia-fringed worm creatures: cream bodies with feathered ink stripes, segment arcs, speckles, gill-fan or eye-face heads, white and red cilia, and tail strands. Everything is in one file, `index.html`, with no build step or dependencies.

## Run it

Open `index.html` in a current Chrome, Safari, Firefox or Edge (WebGL2 is required). It works on phones; the control panel becomes a bottom sheet.

Start on a specific creature by adding a hash: `index.html#plumed-grub`, `#fan-striper`, `#bubble-chain`, `#pale-crawler`, `#coiled-ring`.

## How it works

| Part | Where | What it does |
| --- | --- | --- |
| Spine simulation | `class Creature` (JS) | Follow-the-leader chain of up to 64 nodes. The head steers with wander, slither, obstacle sensing and an optional lure; joints relax toward a curl target ("shape memory") and are limited by a max bend. Peristalsis waves change segment length and girth. |
| Collisions | `world.collideWalls`, `world.collideCreatures` | Nodes are pushed out of wall segments and other creatures. A crossing test stops segments tunnelling through thin walls; segments slide along walls instead. |
| Body shader | `BODY_FS` (GLSL) | Each body is a stack of layered ellipses, one per node, drawn on one quad. Per pixel it finds the top layer, then shades in body coordinates (along/across the spine): edge streaks, longitudinal stripes, checkered cross bands, side shadow, comb lines, speckles, Worley cell voids, light rims, segment arcs, gill-fan or eye-face head, and an outer beaded membrane. Simplex noise at several anisotropic octaves gives the combed, feathered ink edges. |
| Cilia | `buildHairGeom` + `HAIR_FS` | Each cilium is a 10-point strip rebuilt every frame from its root on the body rim, with curl, travelling wiggle and drag when the body moves. The fragment shader draws a light core with a dark outline. Red cilia fade from root to tip color. |
| Post | `POST_FS` | Softness, exposure, contrast, saturation, vignette and film grain. |

## Controls

- **Toolbar:** Pause, Regrow, add or remove creatures, show or hide controls.
- **Pointer tools:** Select (tap to pick a creature, drag to steer it), Draw wall (drag; turn on Freehand in Walls for curves), Erase, Lure (creatures crawl toward the pointer), Spawn (tap to place).
- **Panel:** every shape, motion, skin, ink, outline, head, cell, cilia, strand, spore and view parameter. "Controls edit" switches between the selected creature and all creatures.
- **Walls:** thickness, color, presets (Box, Arena, Pillars, Maze, Funnel), clear, undo.
- **Save & shortcuts:** copy a creature's settings as JSON, or paste JSON to apply.

Keyboard: `Space` pause · `R` regrow · `A` add · `Delete` remove · `C` panel · `H` hide all UI · `1`–`5` presets · `G` gallery cycle.
