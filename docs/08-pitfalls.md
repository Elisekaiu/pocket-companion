# 08 · Pitfalls

What went wrong while building this, in roughly the order it happened.

## Character and art

- **Generating the character in every outfit.** Each image redraws the anatomy slightly differently, and no anchor system can fix moving anatomy. → One base per pose family, with clothing-only overlays (see 04).
- **Headphones covering the eyes at every scale.** One image can't fit two ears and a band. → Split items into parts.
- **Outfits "too small".** Coverage was measured including wings and tail. → Measure on the torso.
- **Outfits that looked empty.** They were too tall, so only the collar landed on the body. → Re-crop the source rectangle; don't scale up.
- **Visual off-centre.** Mathematically centred garments with asymmetric details look shifted. → Small per-outfit x corrections, judged by eye.
- **Growth with no visible change.** Stages that only change a number feel like nothing happened. → Show growth on the sprite: a mark that changes, small twinkles, an aura.

## Assets and rendering

- **Buttons from a composed screenshot.** They had softer edges, different density and missing bottoms. → Use a dedicated high-res sheet, crop with margin, and normalize (see 06).
- **Background removal that eats pale edges.** Repeated passes destroy anti-aliased outlines. → Cut once, from alpha, carefully.
- **Black or white rectangles in one theme.** Assets with a baked-in background. → Transparent everywhere; test both themes.
- **Blurry, overlapping text on phones.** A pixel web font at small size. → Glyph atlas, integer scale.
- **Text "too shallow" in light mode.** Greys tuned on a monitor. → Darken until it reads on a phone outdoors.
- **One status chip smaller than the rest.** A longer label changed the layout. → Equal-height chips that stretch.

## Backend

- **Rerolls.** Picking with `Math.random()` at read time means a reload changes what the pet said. → Fixed hashes, and store decisions on the event.
- **Counting the AI as the human.** If the companion's actions mark the human "present", the pet never misses anyone. → Tag every event with who and where; presence comes only from the human.
- **Fake busyness.** Showing "reading" because it's evening is a lie. → Only show what evidence supports, and say what the evidence is.
- **A sandbox that writes.** Testing on the real pet leaves fake history. → `simulate` runs the real operations in memory with a virtual clock; a test asserts the real record is unchanged.
- **Opening the real room in an automated browser.** The page sends "I'm here" pings, and suddenly the pet thinks the human visited at 3 a.m. → Automated checks against production block every non-GET request.

## Text

- **Mixed point of view.** Some lines said 他, others 我, in the same list. → Write once in one voice, convert per context.
- **Leaked tags.** An English tag inside a Chinese sentence. → Every tag needs a label; test by sampling.
- **Substring traps.** `考试` inside `考试过了`; `过了` inside `难过了`. → The longer phrase wins; choose trigger words that aren't parts of opposite phrases.
- **Generic content drowning local content.** Two local lines in a pool of fourteen almost never appear. → Weight by source.

## Deploying

- **Single-file bind mounts follow the inode.** Replacing a file with `mv` or `install` creates a new inode, so a container that mounts the single file keeps serving the old one. → Copy in place (`cp`) and check the inode didn't change.
- **Someone else changed production.** → Before every deploy, compare production with the last version you deployed; merge, never overwrite.
- **Always**: back up what you replace, write a rollback note next to the backup, verify the served file's hash, and smoke-test read-only.
