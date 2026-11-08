# Pocket Companion

Notes for building a small pixel-art virtual pet that lives next to an AI companion: in your own app, and inside the AI's tools.

This is not a ready-made app. It is the thinking behind one that is in daily use: the principles, the backend design, the wardrobe system, the text systems, the pixel rendering, and the mistakes made along the way. The character, its art, its tastes and its words belonged to one relationship and stay private. **The system is shared; the companion should be yours.**

> 一個住在你和 AI 伴侶之間的小像素寵物的設計筆記。這裡分享的是做法和踩過的坑；角色、形象、喜好、台詞都是私人的，沒有放進來。系統可以共用，小傢伙應該是你自己的。

## What makes it different from a normal virtual pet

- **It only reacts to real presence.** It knows you came by because you really opened the app, sent a message, read a book or watched something together. Nothing is simulated to look busy.
- **It lives in real time.** Hunger, sleep and missing you are folded from an event log over the real clock, so the pet is in the right state whenever anyone looks, with no background timers.
- **Random, but never rerolled.** Every "random" choice (a reply, a postcard, an event) is a fixed hash of its id. Reloading never changes what happened, and nobody can farm a better outcome.
- **One pet, two caretakers.** Your app and your AI companion read and write the *same* state through one small backend contract (and an MCP server). The AI can feed it, give it things and read its notebook. It acts only as itself and can never pretend to be you.
- **Gentle by design.** Stats have floors, bond is capped per day, repeated actions give less, and the pet never dies or punishes. Being away makes it miss you, not suffer.

## Contents

| | |
|---|---|
| [docs/01-principles.md](docs/01-principles.md) | The rules everything else follows |
| [docs/02-backend.md](docs/02-backend.md) | Event log, folding over time, the operation contract, the sandbox |
| [docs/03-mcp-for-companions.md](docs/03-mcp-for-companions.md) | Packaging it as an MCP server so an AI companion can look after the pet (and itself) |
| [docs/04-wardrobe.md](docs/04-wardrobe.md) | Dress-up on a pixel sprite: one base body, clothing-only layers, calibration |
| [docs/05-text-systems.md](docs/05-text-systems.md) | A notebook that answers what you write; travel postcards built from many small pieces |
| [docs/06-pixel-rendering.md](docs/06-pixel-rendering.md) | Crisp pixels in a web page: glyph atlas fonts, integer scaling, one button viewport, light and dark |
| [docs/07-pitfalls.md](docs/07-pitfalls.md) | What went wrong and what we learned |
| [art/ART-PROMPT-KIT.md](art/ART-PROMPT-KIT.md) | An image-generation prompt kit for designing your own companion's art |

## How to use these notes

1. Design your companion first (`art/ART-PROMPT-KIT.md`, section 0). Make it non-human, recognisable at small size, and yours.
2. Read `01-principles.md` and decide which rules you keep. They shape everything else.
3. Build the backend as an event log plus pure functions (`02-backend.md`). Put the content (tastes, places, lines) in data, not in code.
4. Give your AI companion access through the same operations (`03-mcp-for-companions.md`).
5. Add the wardrobe, text systems and rendering when the core loop feels right.

## License

Text in this repository: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it, change it, credit it.
