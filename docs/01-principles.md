# 01 · Principles

These rules came first. Most later design decisions follow from them.

## 1. Nothing invented about the people

The pet may *feel* anything, but it may only *know* what really happened.

- "You were here" comes from real signals: the app was opened, a message was sent, a reading session made progress, a watch-together session sent heartbeats, music was really playing.
- Each displayed state carries its **evidence**: what the pet is doing, since when, and why ("asleep since 23:10 · you last appeared 22:58 in the app").
- If a signal is missing, the pet says less. It never fills the gap with a plausible guess.

## 2. Real time, not a game clock

The pet's state is a function of *(everything that happened, now)*. Open it after eight hours and it has slept, got hungry and missed you during those eight hours, computed in one step when you look. No cron, no background process, no catch-up animation.

## 3. Random but fixed

Variety matters (a pet that always says the same thing is dead), but rerolling is cheating and breaks trust.

- Every choice is `hash(id + purpose) % options`, using a stable hash (e.g. the first 32 bits of SHA-256).
- The same diary entry always gets the same reply; the same trip always brings back the same postcard.
- To avoid repetition, look at *recent history* (don't use the same line twice in a row), never at a fresh random number.

## 4. Gentle, bounded, unfarmable

- Stats drift towards floors, never to zero. Being neglected makes the pet hungry, grubby or lonely, never dead.
- Bond (affection) grows slowly, has a **daily cap** and an asymptote. It cannot be maxed by tapping.
- Repeating the same action quickly is refused with a reason ("he just ate"), and repeated feelings within a day give less mood.
- Refusals always explain themselves in the pet's voice, so a "no" still feels like character.

## 5. Two caretakers, one truth

The human's app and the AI companion share one authoritative state behind one contract.

- The AI never scrapes the UI, reads browser storage or edits the database. It calls the same operations as the app.
- Every event records **who** did it and **where** (app, chat, reading, watch, travel, self-care).
- The AI acts only as itself. It can give a gift *from itself* or write *its own* notebook line, but its activity never counts as the human being present.

## 6. Self-care is allowed, and visible

If nobody comes for a long time, the pet can have a snack from its stash or a quick wash. This is bounded (only below a threshold, at most every few hours), never raises bond, never counts as presence, and always appears in history as "he looked after himself".

## 7. Test without touching the real pet

Any feature must be testable in a **sandbox**: the same operations, run on an in-memory copy with a virtual clock. The real pet's record never changes because someone was testing.

## 8. Content is personal, the system is shared

Tastes, destinations, reply lines, outfits and art make a companion *someone's*. Keep them as data packs, separate from the engine, so the engine can be shared without giving away the character.
