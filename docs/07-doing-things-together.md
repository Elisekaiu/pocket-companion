# 07 · Doing things together: watching and reading

The pet feels most alive when it notices the things you and your AI companion really do together. Two activities matter most: **watching something together** and **reading together**.

Both ideas come from other people's projects, which do the *together* part itself. The pet only listens to them:

- Watching together with an AI companion: [corcordata/plus-one](https://github.com/corcordata/plus-one)
- Co-reading with shared annotations between AI and human: [meowmana/coread](https://github.com/meowmana/coread)

The pet does not reimplement them. It turns their **real signals** into presence and moods.

## Watching together

The watch tool sends the pet's backend a small projection of the session, never the content itself:

```js
watch_presence({ session, event: 'watch_start' | 'heartbeat' | 'pause' | 'end', title })
```

- While heartbeats arrive, the pet is in a **watching** state (its own sprite: blanket, snacks, eyes on the screen) and the human counts as present.
- **Stale after ~75 s** without a heartbeat. If the tab was closed without an `end`, the pet doesn't keep "watching" forever.
- Pause and end are real states: on pause he looks around, on end he stretches.
- **One reward per session.** Watching three hours gives one warm moment, not a stat farm.
- Priority: being away on a trip beats watching, and watching beats sleeping (he wakes up for a film night).

## Reading together

The reading module reports real progress: a page turned, time spent, a note written.

- **Only real progress makes him read.** An open book with no progress is not reading.
- **Fresh for ~20 min** after the last progress, then he puts the book down.
- If the human *and* the AI companion are both reading the same book, he is reading **together**: a different sprite, a different line, a little more bond.
- The pet's tastes matter: if it likes `books`, reading lifts its mood a bit more.

## Why this works

The pet doesn't need cameras, guesses or timers. Each "we did something together" comes from a tool that really saw it happen. The pet just turns that into something cute, so the evening reads like *we watched a film, and the little one watched with us*.

If you build your own, connect whatever your shared life looks like: music, games, walks, cooking. The pattern is the same: **a real signal in → a state with evidence → a gentle, bounded effect.**
