# Camel Rush — Asset Pack (Optional Drop-In Files)

The game runs out-of-the-box using procedural primitives + canvas-painted textures.
This folder lets you drop in **optional high-quality assets** to drastically
improve the visual quality without touching code.

If a file below is found at load, the procedural fallback is automatically
replaced. If absent (404), the game silently uses the procedural version.

## Folder layout

```
assets/
├── models/
│   ├── camel.glb               (optional — replaces 5 primitive camels)
│   └── palm.glb                (optional — replaces 12 primitive palm trees)
└── textures/
    ├── sky.hdr                 (optional — HDR environment for IBL reflections)
    ├── board.png               (optional — AI/illustrator board image; overrides procedural canvas)
    └── sand/
        ├── albedo.jpg          (optional — PBR sand color map)
        ├── normal.jpg          (optional — PBR sand normal map)
        ├── rough.jpg           (optional — PBR sand roughness map)
        └── ao.jpg              (optional — PBR sand ambient occlusion)
```

## Recommended free CC0 sources

All models below are free for commercial use under CC0 (no attribution required).

### Camel (`camel.glb`)
Pick **one** model and rename to `camel.glb`:

| Source | Notes |
|---|---|
| [Quaternius — Animals Pack Vol 4](https://quaternius.com/packs/animalsultimate.html) | Includes a stylized camel with walk/idle animations. Recommended. |
| [Poly Pizza — Camel](https://poly.pizza/search/camel) | Multiple CC0 camel models, search and pick. |

**Tip**: The loader auto-tints the camel's diffuse color to each player color
(blue/green/red/yellow/white). Use a model with neutral / light beige base for
best tinting results. If the model has an embedded walk animation, it will be
played automatically while the camel is moving.

### Palm tree (`palm.glb`)
| Source | Notes |
|---|---|
| [Quaternius — Nature Pack](https://quaternius.com/packs/naturepack.html) | "Palm_Tree" model. |
| [Kenney — Nature Kit](https://kenney.nl/assets/nature-kit) | Multiple palm options. Convert OBJ→GLB if needed (see below). |
| [Poly Pizza — Palm](https://poly.pizza/search/palm) | |

### HDR environment (`sky.hdr`)
| Source | Notes |
|---|---|
| [Polyhaven — HDRIs](https://polyhaven.com/hdris/skies) | Free CC0 equirectangular HDRs. Recommended: "satara_night", "kloppenheim_06", "moonless_golf". |
| [HDRI Haven](https://hdri-haven.com) | Larger archive (mostly CC0). |

Download a `.hdr` (1k or 2k resolution is plenty), rename to `sky.hdr`, drop in.
Used as image-based lighting (IBL) — golden domes/dice will reflect the sky.

### PBR sand textures (`sand/`)
| Source | Notes |
|---|---|
| [Polyhaven — Textures](https://polyhaven.com/textures/terrain) | Search "sand", e.g. **aerial_beach_01** or **sandy_gravel_02**. Download "1k JPG", extract: `*_diff.jpg`→`albedo.jpg`, `*_nor_gl.jpg`→`normal.jpg`, `*_rough.jpg`→`rough.jpg`, `*_ao.jpg`→`ao.jpg`. |
| [AmbientCG — Sand](https://ambientcg.com/list?type=Material&q=sand) | Similar setup. |

Folder structure example:
```
assets/textures/sand/
├── albedo.jpg
├── normal.jpg
├── rough.jpg
└── ao.jpg
```

### AI-generated board image (`board.png`)
This **overrides the procedural canvas board entirely** with a single illustrated image.

**Recommended dimensions**: 2560×1920 px, PNG, sRGB.

**Suggested AI prompt** (Midjourney / DALL-E / Stable Diffusion):
```
top-down view of an arabian camel race board game, oval racing track with
16 numbered sand-colored stone tiles arranged around an ellipse, ornate
arabic geometric border with 8-point gold stars, paved sandstone interior
plaza, faint sun emblem in the center, parchment background with grain
texture, hand-painted boardgame illustration style, clean readable numbers
1 to 16 on each tile in sequence, no other objects, plain top-down map view
--ar 4:3 --style raw
```

After generation:
1. Crop / resize to ~2560×1920 with the oval centered.
2. (Optional) Use Photoshop / GIMP to overlay tile numbers 1–16 if AI output
   isn't readable.
3. Save as `assets/textures/board.png`.

The 3D pyramid, pavilions, dice tray etc. continue to sit on top of the
illustration.

## Converting other formats to GLB

If your downloaded model is `.obj`, `.fbx`, or `.gltf+bin`, convert to
self-contained `.glb`:

- **Blender**: File → Import → (your format) → File → Export → glTF 2.0
  (.glb), enable "Selected Objects" and "+Y Up".
- **Online**: <https://anyconv.com/obj-to-glb-converter/> or similar.

## Auto-fit & scaling

The loader auto-scales the model to a target world height:
- Camel: ~0.85 units tall
- Palm: ~2.6 units tall

You don't need to pre-scale your model — just make sure it points up (+Y) and
the origin is at the base.

## Verifying it works

1. Drop your file(s) into the right folder (`assets/models/` or `assets/textures/...`).
2. Reload the page.
3. Open browser DevTools → Console. You should see one log line per loaded asset:
   ```
   [camel-rush] Loaded GLB camel asset.
   [camel-rush] Loaded GLB palm asset.
   [camel-rush] Loaded HDR environment.
   [camel-rush] Loaded PBR sand textures.
   [camel-rush] Loaded AI board illustration override.
   ```
4. If you see no log line for an asset, the file was not found. Check the
   filename and path.

## Adding more asset slots

Open `index.html` and search for `tryLoadGLB(`. You'll see a small block per
asset. Copy/paste the pattern to add more slots (e.g., a `pavilion.glb` that
replaces the blue tents, or a `pyramid.glb` that replaces the procedural one).
