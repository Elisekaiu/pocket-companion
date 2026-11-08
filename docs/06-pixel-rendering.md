# 06 · Crisp pixels in a web page

Pixel art in a browser goes blurry in many small ways. These are the fixes that held up on real phones.

## Draw on canvas at integer scale

- Keep the scene at its native size (e.g. 120×96) in a canvas, and scale the canvas element by a **whole number of device pixels**: `scale = floor(available / native)` with `devicePixelRatio` taken into account.
- `image-rendering: pixelated` on every canvas and image, and `ctx.imageSmoothingEnabled = false`.
- A non-integer CSS size (e.g. 2.5×) always blurs or produces uneven pixels. Leave a margin instead.

## Pixel text: a glyph atlas instead of a web font

A pixel web font at 12 px looks fine on desktop and blurry or overlapping on a phone (sub-pixel positioning, hinting, fallback fonts). What worked:

1. Take an open-licensed pixel font (for CJK, e.g. Fusion Pixel 12px, SIL OFL).
2. In headless Chromium, draw **only the characters your UI uses** (collected from your source files) at the font's native size, threshold to 1-bit, and pack them into a PNG atlas + a JSON of `{char: [x, y, width]}`.
3. Detect missing glyphs (the browser fell back to an anti-aliased font: many semi-transparent pixels) and leave them out.
4. Lay text out yourself from the atlas, at integer scale, with explicit line breaking.

Result: ~1,300 glyphs in an ~84 KB PNG instead of a 700 KB font, and identical pixels on every device. Rebuild the atlas whenever the text changes.

**Theme-aware text.** If you paint text into a canvas, the colour is baked in. Store the CSS variable name (e.g. `--px-ink`) with each painted text and repaint when the theme changes. Otherwise a light-mode switch leaves washed-out dark-mode text behind.

## Buttons: one viewport for all

When some buttons come from a different source sheet, they look bigger or blurrier even at the same CSS size, because the *visible artwork* differs.

1. Crop each button with a small transparent margin. Never auto-trim tight.
2. Area-average the new ones so their **frame height in art pixels** matches the originals (e.g. 64 px), correcting non-square pixels in the source.
3. Recolour new buttons to the originals' palette if they don't match.
4. Place every button centred in **one shared viewport** (e.g. 123×69) on one baseline.
5. Draw all of them at one integer scale. Never resize one icon on its own.

Build a QA view with all buttons side by side, guides for viewport, baseline and visible bounds, on light and dark backgrounds.

## Light and dark

- Light should be warm ivory with real contrast. Pale grey text on ivory looks elegant on a monitor and unreadable on a phone in daylight.
- Dark should be deep navy, not black, with borders bright enough to read.
- Never let an asset bring its own background: a white sheet in dark mode, or a black box behind a sprite in light mode, is almost always a transparency bug.

## Motion

- Run the room at ~30 fps with `requestAnimationFrame` gated by a timer. That's smooth enough for pixel art and easy on battery.
- Give the pet a **wander** behaviour in idle states: walk left and right, flip, little hops, pause. Five seconds of a stationary sprite reads as "broken".
- Use short dissolves between states, not hard cuts.
- Stop the loop when the page is hidden.
