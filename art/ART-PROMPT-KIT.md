# Companion Tamagotchi Art Prompt Kit

A reusable image-generation prompt kit for creating a **unique pixel-art Tamagotchi companion**.

This kit is intentionally generic.  
Replace the placeholders with your own companion's identity, colors, motifs, personality, and visual style.

The goal is **not** to make every user generate the same mascot.  
The goal is to give each person a consistent visual system for their own companion.

---

# 0. Before generating anything: define the companion identity

Fill this out first.

```text
Companion name:
Creature type / silhouette:
Main body shape:
Head shape:
Ear / horn / antenna details:
Tail:
Eye style:
Primary colors:
Secondary colors:
Accent colors:
Signature motif:
Signature accessory:
Personality:
Overall mood:
Visual keywords:
Things to avoid:
```

Example structure:

```text
Companion name: [NAME]

Creature:
A small non-human fantasy companion with a soft rounded body,
large expressive eyes, distinctive ears/horns, and a recognizable tail.

Palette:
[PRIMARY COLOR], [SECONDARY COLOR], [ACCENT COLOR]

Signature motifs:
[MOON / STARS / FLOWERS / CLOUDS / FIRE / SEA / ETC.]

Personality:
[CALM / CURIOUS / MISCHIEVOUS / SHY / ELEGANT / COZY / ETC.]

Visual keywords:
cute pixel art, collectible virtual pet, soft game sprite,
clean silhouette, readable at small size, expressive but not over-detailed
```

---

# 1. MASTER CHARACTER REFERENCE

Use this first.

## Prompt

```text
Create the definitive pixel-art character reference for my Tamagotchi companion.

Character identity:
[PASTE YOUR CHARACTER IDENTITY HERE]

The character must be clearly NON-HUMAN and visually unique.

Design requirements:
- cute collectible virtual-pet proportions
- compact readable silhouette
- large expressive face
- simple enough to remain recognizable at small pixel size
- strong visual identity
- no realistic human anatomy
- no robot design unless explicitly requested
- no random costume yet
- neutral/default pose
- front-facing or slight 3/4 view
- full body visible
- consistent pixel density
- crisp pixel edges
- no blur
- no painterly rendering
- no text
- no UI frame
- plain transparent or very simple neutral background

Style:
high-quality handcrafted pixel art,
soft but crisp shading,
clear dark outline,
limited coherent palette,
Tamagotchi / virtual pet game sprite feeling,
modern polished pixel-art game asset

Important:
This image will become the MASTER CHARACTER REFERENCE.
Do not add temporary props that change the base silhouette.
Keep all signature anatomy and markings stable.
```

---

# 2. BASE POSE / STATE SPRITE SHEET

Generate after the master reference is approved.

## Prompt

```text
Using the approved companion reference, create a consistent pixel-art state sprite sheet.

The SAME character must appear in every state.
Do not redesign the face, anatomy, ears, horns, tail, proportions, or palette.

Required states:

idle
happy
very happy
excited / energized
just woke up
sleepy
tired
asleep
dreaming
hungry
eating
snacking
drinking
petted
hugging
together / cuddling
sad
lonely / missing someone
waiting
waiting a long time
sick
unwell
bath
reading
focused / studying
writing diary
listening to music
gaming / playing
watching a movie
receiving a gift
shy
thinking
curious
discovering something
celebrating
traveling
returning home
relaxed
cared for
stargazing

Sprite-sheet requirements:
- same canvas size for every cell
- same pixel density
- same body proportions
- same facial structure
- no random redesign between cells
- character fully inside every cell
- no cropping
- clean separation between cells
- plain neutral background
- no labels inside the art
- no overlapping cells

Important:
Some poses may naturally hide the neck, tail, or body.
That is okay.
Do not force every accessory slot to be visible in every pose.

Prioritize consistency over variety.
```

---

# 3. CANONICAL DRESS-UP BASE

This is important for wardrobe systems.

## Prompt

```text
Create a canonical dress-up base sprite for this companion.

Use the approved master character reference.

Requirements:
- neutral standing or seated pose
- clean front-facing silhouette
- body centered
- both shoulders clearly readable
- paws/hands visible and separated from torso
- chest center visible
- neck area visible
- tail position clearly readable
- no outfit except the minimal default base
- no large props
- no headwear
- no cape
- no handheld item
- no effect particles

This sprite will be used as the wardrobe fitting template.

Keep:
- exact character proportions
- exact face
- exact anatomy
- exact palette
- crisp pixel edges

Do not redesign the character.
```

---

# 4. WARDROBE — BODY OUTFITS

Generate outfits separately from the base.

## Prompt

```text
Create a pixel-art wardrobe outfit sheet for my companion.

Use the approved canonical dress-up base as the fitting reference.

Important:
Do NOT redraw the companion.
Generate clothing designs that fit the companion's existing body proportions.

Create these outfit concepts:

- cozy homewear
- sleepwear
- academy / school outfit
- reading cardigan
- rain poncho
- winter cloak
- ceremonial outfit
- elegant dark / gothic outfit
- casual everyday outfit
- travel outfit

Style direction:
[INSERT YOUR COMPANION'S PERSONAL STYLE]

Color palette:
[INSERT PALETTE]

Requirements:
- clear silhouette
- readable at small pixel size
- avoid excessive tiny details
- 1–2 strong visual motifs per outfit
- clean neckline
- readable sleeves
- clear hem
- clothing should look wearable, not pasted on
- do not change the companion's anatomy
- do not add a new face/head/body
- no background scene
- no text

The outfits should visually belong to the same world as the companion.
```

---

# 5. WARDROBE — ACCESSORIES

## Prompt

```text
Create a pixel-art accessory sheet for my Tamagotchi companion.

Use the approved companion reference.

Create accessories in these categories:

HEAD
- beret
- sleep cap
- crown
- flower crown
- headphones
- glasses
- halo
- seasonal headwear

NECK / CHEST
- scarf
- ribbon
- pendant
- collar
- bow

BACK
- wings
- cape
- small backpack

TAIL
- tail ribbon
- charm
- bell
- star/moon charm

HANDHELD
- tiny book
- cup
- flower
- gift
- lantern
- travel bag

EFFECTS
- sparkles
- hearts
- music notes
- snow
- petals
- stars
- bubbles

Style:
same pixel density and outline style as the companion.

Important:
Accessories must remain modular.
Do not redraw the whole character inside each accessory asset.
Do not make every item oversized.
Keep silhouettes readable at small scale.
```

---

# 6. TAMAGOTCHI SHELL / DEVICE SKINS

## Prompt

```text
Create a set of collectible Tamagotchi-style device shells for my companion.

Do NOT include the companion sprite inside the screen.

The shell should feel like a magical collectible virtual-pet device.

Create 6–8 shell designs based on these themes:

- default / signature theme
- night / moon
- study / library
- flower / garden
- winter / snow
- celestial / angelic
- elegant dark / gothic
- seasonal special edition

Visual identity:
[INSERT MOTIFS]
[INSERT PALETTE]

Requirements:
- each shell uses the same approximate overall device proportions
- screen opening remains consistent
- buttons remain in consistent locations
- decorative frame can vary
- each shell should feel clearly distinct
- readable pixel-art details
- no blur
- no text
- no companion character
- no UI labels
- centered individual shell
- generous spacing between designs

Make each shell large enough to crop cleanly later.
Do NOT pack tiny shells too tightly.
```

---

# 7. IN-DEVICE BACKGROUNDS / SCENES

## Prompt

```text
Create pixel-art background scenes for the inside of a Tamagotchi screen.

These backgrounds are for my companion:
[SHORT CHARACTER / WORLD DESCRIPTION]

Generate separate scenes:

- cozy bedroom
- reading nook
- study desk
- moonlit bedroom
- rainy window
- snowy night
- flower garden
- bath room
- sick/rest room
- movie watching room
- music room
- meal / kitchen corner
- travel departure scene
- travel destination scene
- celebration scene
- stargazing scene
- quiet waiting scene
- daytime home scene
- nighttime home scene

Requirements:
- no main character in the scene
- leave central foreground space for the companion sprite
- avoid excessive visual clutter
- consistent perspective
- consistent pixel density
- strong atmosphere
- readable at small Tamagotchi-screen size
- no text
- no UI
- no borders

Use the companion's palette and motifs subtly throughout.
```

---

# 8. MAIN ACTION BUTTON ICONS

## Prompt

```text
Create a unified pixel-art Tamagotchi action button set.

The buttons must all look like they were created as ONE asset family.

Actions:

Feed
Interact / Together
Sleep
Gift
Travel
Wardrobe
Appearance / Shell
Notebook / Diary

Button-frame style:
[DESCRIBE FRAME STYLE: CLOUD / STAR / FLOWER / METAL / ETC.]

Companion visual motifs:
[INSERT MOTIFS]

Requirements:
- identical button canvas size
- identical frame silhouette
- identical border thickness
- identical pixel density
- identical baseline
- equal perceived visual weight
- same lighting/shading style
- icon centered inside each button
- no text inside the button
- no labels
- generous spacing between buttons
- no overlapping assets

Use simple readable icon symbols:
Feed = food bowl
Together = hug / heart
Sleep = bed / moon
Gift = present
Travel = plane / suitcase
Wardrobe = hanger / clothes
Appearance = shell / mirror
Notebook = diary / notebook

Generate at a sufficiently large native pixel size so each icon can be cropped cleanly.
```

---

# 9. GIFT ITEM SHEET

## Prompt

```text
Create a pixel-art gift item collection for a virtual companion.

Visual world:
[INSERT COMPANION STYLE / PALETTE]

Create items in these categories:

FOOD
- cake
- pancakes
- pudding
- tea
- croissant
- macaron
- chocolate
- rice ball / comfort food

BOOKS / STATIONERY
- ornate book
- stack of books
- open book
- moon notebook
- quill
- ink bottle
- letter
- photo / postcard

TOYS
- plush rabbit
- plush cat
- plush sheep
- toy plane
- snow globe
- handheld game
- headphones
- puzzle

FLOWERS
- signature flower bouquet
- lilies
- hydrangea
- cherry blossom
- lavender
- sunflower
- cotton flower
- preserved flower

ACCESSORIES
- necklace
- bracelet
- pendant
- key
- pocket watch
- snowflake charm
- perfume
- ribbon

SOUVENIRS
- postcard
- landmark snow globe
- city bus / train miniature
- small travel building
- winter cabin
- aurora postcard
- star bottle

DAILY / COZY
- scarf
- hot water bottle
- candle
- sleep mask
- blanket
- pillow
- mug
- moon cushion

Requirements:
- each item clearly separated
- consistent pixel density
- consistent lighting
- no labels inside the artwork
- no character
- no UI
- no overlapping items
- large enough for clean cropping
```

---

# 10. TRAVEL ASSETS

## Prompt

```text
Create pixel-art travel assets for a Tamagotchi companion.

The visual style should match:
[INSERT COMPANION WORLD]

Create:

- tiny suitcase
- passport
- train ticket
- plane ticket
- map
- camera
- postcard
- travel bag
- compass
- souvenir pouch
- departure sign
- return-home gift box

Also create several travel postcards / destination scenes:
- snowy town
- old European street
- quiet Japanese town
- seaside city
- mountain lake
- flower field
- aurora night
- rainy city
- cozy café abroad

Requirements:
- collectible-game feeling
- consistent pixel density
- no text unless decorative pseudo-text
- no real logos
- no copyrighted branding
- no main character unless specifically requested
```

---

# 11. SPECIAL EVENT / CHOICE UI

## Prompt

```text
Create a pixel-art choice-event UI kit for a virtual pet game.

Visual identity:
[INSERT COMPANION STYLE]

Create:

- speech bubble
- thought bubble
- event title frame
- 2-choice panel
- 3-choice panel
- 4-choice panel
- selected-choice state
- rare-event frame
- gift-event frame
- travel-event frame
- small decorative arrows / stars / hearts

Requirements:
- clean pixel typography area
- leave room for text
- no actual long text rendered
- no baked-in language
- readable on mobile
- soft game-like frame
- visually compatible with Tamagotchi UI
```

---

# 12. PIXEL STATUS BUBBLES

## Prompt

```text
Create a set of tiny pixel-art status/thought bubbles for my virtual companion.

Include visual bubbles for:

hungry
sleepy
happy
sad
lonely
thinking
curious
reading
music
movie
travel
gift
bath
sick
waiting
celebrating
love
surprised

Use icons and tiny expressions such as:
heart
moon
book
music note
plane
gift
bowl
bubble
question mark
ellipsis
sparkle
zZ

Keep them very simple and readable.

No full sentences.
No large text.
```

---

# 13. STAT METER VISUALS

## Prompt

```text
Create a unified pixel-art stat meter UI kit.

Stats:

Affection
Fullness
Energy
Mood
Hygiene
Health
Growth

Each meter should have a different visual identity:

Affection = hearts
Fullness = food/bowl segments
Energy = stars/moon/battery
Mood = expressive blocks/stars
Hygiene = bubbles/water
Health = crosses/health cells
Growth = diamonds/stars/evolution track

Requirements:
- same overall UI family
- different colors per stat
- readable at small mobile size
- empty / partial / full states
- no text labels baked into the artwork
```

---

# 14. LOADING / PROGRESS ELEMENTS

## Prompt

```text
Create a pixel-art loading and progress UI kit for a Tamagotchi companion app.

Include:
- segmented loading bar
- rounded progress bar
- star progress bar
- heart progress bar
- circular pixel loader
- small waiting spinner
- travel progress indicator
- sleep progress indicator

Keep the style consistent with:
[INSERT COMPANION WORLD]

No text.
```

---

# 15. DARK / LIGHT THEME VARIANTS

## Prompt

```text
Create coordinated LIGHT and DARK theme UI assets for the same virtual-pet world.

Light theme:
- soft ivory / pale pastel background
- strong enough contrast to remain readable
- gentle but not washed out

Dark theme:
- deep navy / midnight palette
- glowing accent colors
- keep pixel borders readable

Both themes must feel like the SAME product.

Do not redesign the asset family between themes.
Only adapt:
- background
- contrast
- highlight intensity
- border brightness
```

---

# 16. IMPORTANT CONSISTENCY PROMPT

Paste this at the end of EVERY generation request when making follow-up assets.

```text
CONSISTENCY REQUIREMENTS:

Use the approved companion reference as the source of truth.

Do not redesign:
- face
- eyes
- ears
- horns
- tail
- body proportions
- signature markings
- palette

Maintain:
- identical pixel density
- identical outline thickness
- identical shading language
- identical world aesthetic

Do not introduce:
- random new anatomy
- random robot parts
- random human features
- realistic rendering
- painterly blur
- inconsistent pixel size

If creating UI assets:
keep all related assets in the same visual family.

If creating sprites:
keep all poses on consistent canvases.

If creating wardrobe:
do not redraw the companion when only the clothing asset is requested.

Prioritize consistency over novelty.
```

---

# 17. OPTIONAL — MAKE IT MORE UNIQUE

To avoid everyone producing the same blue/moon pet, replace generic motifs with personal ones.

Try choosing:

```text
Creature inspiration:
[fox / moth / sea slug / cloud creature / dragon / rabbit-like fantasy creature / etc.]

Motif:
[constellations / tea / rain / roses / library / fireflies / ocean / stained glass / mushrooms / etc.]

Palette:
[3–5 colors]

Texture:
[soft velvet / glass / snow / ink / ceramic / clouds / etc.]

Mood:
[cozy / dreamy / mischievous / gothic / warm / quiet / whimsical / etc.]

Signature object:
[key / ribbon / bell / moon / book / flower / lantern / etc.]
```

The system should be shared.

The companion should be personal.
