# Pocket Companion

Notes for building a small pixel-art virtual pet that lives next to an AI companion: in your own app, and inside the AI's tools.

This is not a ready-made app. It is the thinking behind one that is in daily use: the principles, the backend design, the wardrobe system, the text systems, the pixel rendering, and the mistakes made along the way. The character, its art, its tastes and its words belonged to one relationship and stay private. **The system is shared; the companion should be yours.**

> 中文版在 [main 分支](https://github.com/Elisekaiu/pocket-companion). This is the English version.

## What makes it different from a normal virtual pet

- **It only reacts to real presence.** It knows you came by because you really opened the app, sent a message, read a book or watched something together. Nothing is simulated to look busy.
- **It lives in real time.** Hunger, sleep and missing you are folded from an event log over the real clock, so the pet is in the right state whenever anyone looks, with no background timers.
- **Random, but never rerolled.** Every "random" choice (a reply, a postcard, an event) is a fixed hash of its id. Reloading never changes what happened, and nobody can farm a better outcome.
- **One pet, two caretakers.** Your app and your AI companion read and write the *same* state through one small backend contract (and an MCP server). The AI can feed it, give it things and read its notebook. It acts only as itself and can never pretend to be you.
- **Things you do together count.** Watching a film together or reading the same book (with tools like plus-one and coread, see Credits) puts the pet on the sofa or under the reading lamp with you.
- **Travel is a huge combination.** The pet goes on real-time trips and brings back postcards assembled from many small pools (place, weather, spot, who it met, what it ate, a mishap, a P.S.), so no two trips read alike. The places and tastes are personal: fill them with your own.
- **Gentle by design.** Stats have floors, bond is capped per day, repeated actions give less, and the pet never dies or punishes. Being away makes it miss you, not suffer.

## What the little one does (and says)

It has lots and lots of reactions. These are just a few 🥹

**📓 The notebook answers what you write**

| you write | it |
|---|---|
| 我爱你 (I love you) | blushes ♥ "Say it again? I want to hear it once more." |
| 我爱你 (again) | "Snuggle! (pounces)": same words, a different answer every time |
| 我讨厌你！ (I hate you!) | a little hurt ; ; "I'll sit in the corner for a bit… call me and I'll come right back." |
| 对不起嘛，晚安 (sorry, good night) | forgives you ^ ^ "It's okay, it's okay! I stopped being sad ages ago." + "Good night." |
| 今天好累 (so tired today) | wants you to rest … "Close your eyes for five minutes; I'll count sheep for you." |
| 感冒了 (I caught a cold) | worried … "Drink something warm and sleep early. I'll watch the clock for you." |
| 考试过了！ (I passed!) | so proud !! "Shall we celebrate? Little cake, my treat." |
| 没考过 (I failed) | wants to hug you ♥ "I'm not good with words, but I can stay right here." |

Twenty-some feelings, good and bad. It understands "not" (不爱你 ≠ 爱你) and mixed feelings.

**✈️ Travel: different every time**
- Postcards assembled from many small pieces: "The fog is thick; the street lamps look like little moons. In the night square, I thought of you."
- Little mishaps: "The wind blew his hat into the river, and a duck brought it back." "He took the wrong bus and saw half the city extra."
- Local encounters: a gondolier sings for him; a reindeer nudges his hand.
- Sometimes a bonus souvenir; long trips send a second postcard.
- How it went depends on its tastes and what it packed (an umbrella on a rainy day: "it came in handy").

**🌙 While you're away**
- It goes from "misses you a little" to "waiting", "misses you" and "has waited a long time".
- When you come back there is **one** real reunion, sometimes with a drawing it made while waiting ("drew you", "drew our little home").
- It finds a little flower and presses it in a book for you: "He found it, and thought of you first."
- It has strange dreams and writes them down.
- If nobody comes for too long, it has a snack or a wash by itself, and history honestly says "he looked after himself".

**🎁 Gifts, wishes and little events**
- Gift reactions come from its stable tastes and the moment (sleepy, cold, missing you). Repeats give less, but it never dislikes a gift.
- Sometimes it makes a little wish. Give the right thing and it is extra happy.

**✨ Special events: little moments that pop up in its day**

Every now and then something happens, and you choose how to respond:

| event | you can |
|---|---|
| 🌠 **A shooting star!** "Quick, make a wish!" (late at night, rare) | wish about us / about him / about tomorrow |
| 🌙 **He had a strange dream** (only after a full night's sleep) | tell me / hug him first / let him rest |
| 🌼 **He found a little flower** | put it in a vase / press it in a book / give it to you |
| 🧣 **Something new, and he is shy** | wear it now / save it for a special day |
| 👗 **He can't decide what to wear** | pick one from his own wardrobe |
| ❓ **He has a little question**: "Moon or stars?" "Rain or snow?" "The sea or the mountains?" | choose your answer |
| 🎨 **He made a little mess**: "Paint spilled all over the little table…" | tidy up together / laugh it off |
| ☕ **He wants to unwind** | rest / read a little / have tea |
| 🎁 **He found something in a drawer** | have a look |

His dreams are kept too: "Dreamed we sat on the moon fishing, and everything we caught was glowing words." "Dreamed I walked in the snow for a long time, and when I looked back, you'd been behind me all along."

Events follow real life: dreams only after a full night's sleep, a mess only right after playing, flowers on daytime walks, shooting stars late at night. A few a day at most, hours apart, and they quietly pass if left for a day. Every event and your choice are stored and never reroll.

**🙅 Even "no" is cute**
- Feed it right after a meal: refused, with a reason.
- Want to play while it's asleep? "He's asleep."
- Already celebrated today? "We already celebrated today."

**🛋️ Time together**
- Watching together: wrapped in a blanket, eyes on the screen, looking around when you pause, stretching when it ends.
- Reading together: only real reading counts; when you and your AI read the same book, it reads with you.
- Press "celebrate" on a special day and it's happy for six hours.

**👕 Dress-up and growing up**
- Outfits, hats, scarves, wings and tail charms, previewed in a dressing room before you confirm. At bedtime the clothes are folded beside it.
- Different device shells and room backgrounds; the background can follow the hour and what it's doing.
- When it grows up, you can see it: the mark on its forehead changes, and little stars twinkle around it.
- When idle, it wanders around the room, turns and does little hops.

**🤝 Your AI companion is there too**
- The AI can feed it, read its notebook, send it travelling, and give it a new outfit with a note.
- Everything the AI does is recorded under its own name. It never pretends to be you.

## Contents

| | |
|---|---|
| [docs/01-principles.md](docs/01-principles.md) | The rules everything else follows |
| [docs/02-backend.md](docs/02-backend.md) | Event log, folding over time, the operation contract, the sandbox |
| [docs/03-mcp-for-companions.md](docs/03-mcp-for-companions.md) | Packaging it as an MCP server so an AI companion can look after the pet (and itself) |
| [docs/04-wardrobe.md](docs/04-wardrobe.md) | Dress-up on a pixel sprite: one base body, clothing-only layers, calibration |
| [docs/05-text-systems.md](docs/05-text-systems.md) | A notebook that answers what you write; travel postcards built from many small pieces |
| [docs/06-pixel-rendering.md](docs/06-pixel-rendering.md) | Crisp pixels in a web page: glyph atlas fonts, integer scaling, one button viewport, light and dark |
| [docs/07-doing-things-together.md](docs/07-doing-things-together.md) | Watching and reading together: turning real shared activity into presence and moods |
| [docs/08-pitfalls.md](docs/08-pitfalls.md) | What went wrong and what we learned |
| [art/ART-PROMPT-KIT.md](art/ART-PROMPT-KIT.md) | An image-generation prompt kit for designing your own companion's art |

## How to use these notes

1. Design your companion first (`art/ART-PROMPT-KIT.md`, section 0). Make it non-human, recognisable at small size, and yours.
2. Read `01-principles.md` and decide which rules you keep. They shape everything else.
3. Build the backend as an event log plus pure functions (`02-backend.md`). Put the content (tastes, places, lines) in data, not in code.
4. Give your AI companion access through the same operations (`03-mcp-for-companions.md`).
5. Add the wardrobe, text systems and rendering when the core loop feels right.

## Credits

- The *watching together* idea comes from [corcordata/plus-one](https://github.com/corcordata/plus-one): watch anything on the web together with your AI companion.
- The *reading together* idea comes from [meowmana/coread](https://github.com/meowmana/coread): a co-reading room with shared annotations between AI and humans.
- The pixel type uses [Fusion Pixel Font](https://github.com/TakWolf/fusion-pixel-font) by TakWolf (SIL OFL 1.1).
- `art/ART-PROMPT-KIT.md` was written by the AI companion this pet belongs to.

## License

Text in this repository: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it, change it, credit it.
