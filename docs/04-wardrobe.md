# 04 · Wardrobe on a pixel sprite

Dress-up is the feature that looks easy and isn't. This is the system that finally worked, after several that didn't.

## The rule

> **One canonical base body per pose family. Clothing is exported as transparent overlays on the exact same canvas, containing clothing pixels only. Never redraw the character.**

For poses where clothing can't sensibly fit (asleep, bath, lying down), use explicit precomposed variants or a fallback (e.g. the outfit folded beside the bed), not a forced body outfit.

## Why not one sprite per outfit?

That was the first attempt: generate the character wearing each outfit, then cut it out. It failed because every generated image redraws the character slightly differently: a different head size, ear angle, belly line or paw position. Anchors can't fix that, because the *anatomy itself* moved. Swapping outfits made the pet visibly change shape.

## Pose families

Group poses into families that share a skeleton:

- **standing / main**: idle, happy, eating, ... → full wardrobe
- **sitting**: reading, writing, gaming → full wardrobe with sitting offsets
- **sleep**: asleep, dreaming, just woke → sleep pieces only (cap, mask), outfit folded nearby
- **special**: bath, travel, sick → fallback: accessories only or none

Each family has one base sprite. Dress-up happens on the base.

## Anchors: name the place, not the item

In a calibration file, give each family named anchor points measured on its base sprite:

```json
"anchors": { "standing": {
  "head_top": [46, 11], "head_center": [46, 20], "ears": [46.5, 16], "eyes_face": [46.5, 28],
  "above_head": [46, 2], "neck": [45, 42], "chest": [45, 48], "back": [45, 44], "tail": [69, 50],
  "ear_l": [22, 23], "ear_r": [69, 22], "eye_l": [37.5, 27.5], "eye_r": [55.5, 27], "head_w": 48
}}
```

Each accessory says which anchor it uses and how to sit there:

```json
"crown":      { "anchor": "head_top", "pivot": [0.5, 1], "transform": { "x": 0, "y": 5, "scale": 1.08, "z": 3 } },
"halo":       { "anchor": "above_head", "w": 22, "transform": { "y": -3 } },
"headphones": { "parts": {
  "band":  { "anchor": "head_top", "z": 2 },
  "cup_l": { "anchor": "ear_l",   "z": 2 },
  "cup_r": { "anchor": "ear_r",   "z": 2 } } }
```

- `transform` = `{x, y, scale, rotation, z}`, plus optional `transformByPoseFamily` overrides.
- **Parts** split items that touch several places. Headphones as one image always covered the eyes at some scale; as band + two cups, each piece lands where it belongs. Glasses work the same way (lens · bridge · lens on the eye points).
- `pivot` says which point of the item sits on the anchor (bottom-centre for hats, centre for glasses).

## Body outfits: fit onto a template, don't stretch

Give the base a **fitting template**: neckline, shoulders, chest, hem, paws, body bounds and torso bounds.

```json
"templates": { "standing": {
  "neckline": [45, 42], "shoulders": [[29, 44], [60, 44]], "chest": [45, 48], "hem": 55,
  "paws": [[29, 55], [60, 55]], "body_bounds": [25, 61], "torso_bounds": [29, 60] } }
```

Each outfit says which part of its source art is the garment (`src` rectangle) and how wide it should be (`w`). At build or load time:

1. Crop the garment at **source resolution** (the high-res generated image).
2. Resample that rectangle onto the template window (neckline → hem, shoulder to shoulder).
3. Only then pixelize it to the sprite's pixel size (area-average, then quantize to the outfit's palette).
4. Split into **layers**: back (cape, behind the body), body, sleeves, paws, trim, drawn in z order around the base.
5. Add an **occlusion mask** (hull of the torso, expanded a pixel or two) so the base doesn't poke through.

Keep per-outfit scale close to 1 (≈ 0.95–1.08). If an outfit needs more than that, the source crop or width is wrong: **re-export it, don't stretch it.** Free warping breaks pixel art.

Small per-outfit `x` corrections are normal: a cape with a long right side *looks* off-centre even when its box is centred. Fix it visually, by eye, per outfit.

## Tools that made this possible

- **A calibration sandbox page**: pick any item × pose family, drag sliders for x / y / scale / rotation / z, toggle guides (anchors, body bounds, shoulders, hem, occlusion mask), and **export JSON** to paste into the calibration file. Never edits the real pet.
- **Bounds warnings**: flag pieces that cover the eyes (test against the actual pixels, not the bounding box), leave the body bounds, or hide the face.
- **A QA sheet**: every outfit and accessory on every family in one image, regenerated after each change. Mistakes are obvious when you see 40 at once.

## Pitfalls here

- Measure coverage on the **torso**, not the whole body. Wings and tails make an outfit look "too small" when it isn't.
- An outfit that seems empty may just be **too tall**: only the collar lands in the window. Re-crop the source.
- Keep nearest-neighbour everywhere after pixelizing. One smooth scale ruins it.
