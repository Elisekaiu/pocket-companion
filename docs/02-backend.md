# 02 · Backend

A small, boring backend that is hard to get wrong.

## Shape

```
┌──────────── your app ────────────┐        ┌──── AI companion ────┐
│ page: room, buttons, sheets      │        │ chat runtime / tools │
└──────────────┬───────────────────┘        └──────────┬───────────┘
               │ POST /api/<capability>/<operation>    │ MCP: read / write
               └───────────────┬───────────────────────┘
                       ┌───────▼────────┐
                       │ capability     │  manifest + handle({operation, args, helpers})
                       │ (pure-ish JS)  │  serial queue · JSON storage via helpers
                       └───────┬────────┘
                               │
                       one JSON document: { events: [...], checkpoint, ... }
```

One **capability module** owns the pet. It exports a `manifest` (operations, each `read` or `write`, with descriptions) and a `handle()` function. A thin gateway exposes it twice: over HTTP for the app and over MCP for the AI. Both paths reach the same code and the same document, so there is a single source of truth.

Operations run through a **serial queue**: one at a time, so two caretakers acting at the same moment cannot interleave writes.

## The event log

Everything that happens is appended as an event:

```js
{ id, at, type: 'act' | 'seen' | 'gift' | 'diary' | 'trip_start' | 'dress' | ...,
  by: 'human' | 'companion' | 'self',  via: 'app' | 'chat' | 'reading' | 'watch' | ...,
  args: {...},  meta: {...} }      // meta: what was decided at that moment (reaction, reply, outcome)
```

State is never stored as "current stats". It is **derived**:

```js
state = fold(blank(), events, now)
// fold: for each event in time order
//   advance(state, event.at)   – let time pass: hunger, energy, hygiene, missing you
//   apply(state, event)        – what the event does
// then advance(state, now)
```

`advance` walks time in small steps (5 minutes works well) so sleep, waking hours, floors and thresholds behave the same whether you look every minute or after two days.

### Why an event log

- **Real time for free.** Reading is just "fold until now".
- **Explainable.** Every number can be traced back to events, and every displayed state can cite its evidence.
- **Tunable.** Change a rate in one tuning table and the whole history re-folds consistently.
- **Safe for two writers.** Appends don't conflict.

### Keep it fast: checkpoints

Folding a year of events on every read gets slow. Periodically fold the old part into a **checkpoint** (the state at time T) and keep only the events after it. Reads become `fold(checkpoint, recentEvents, now)`.

### Decide once, store the decision

Some things are decided at a moment and must never change: a gift reaction, a notebook reply, where a trip goes, which postcard comes back. Compute them **when the event is written** (with the fixed hash) and store them in `meta`. Later code changes can't rewrite the past, and the AI reading history sees exactly what the human saw.

## Tuning in one place

Put every rate, threshold and cap in one table, for example:

```js
TUNING = {
  absence:  [{from_h:0,level:'present'}, {from_h:2,level:'a_little'}, {from_h:6,level:'waiting'},
             {from_h:12,level:'missing'}, {from_h:24,level:'long_wait'}],     // each with a mood penalty
  reunion:  { after_h: 6 },                               // coming back after 6 h is a reunion, shown once
  rates_per_h: { fullness_awake: 6.2, fullness_asleep: 2.4, energy_awake: 5, energy_asleep: 15, ... },
  floors:   { fullness: 8, hygiene: 15, energy: 4, health: 55 },   // never lower
  affection:{ daily_cap: 3, asymptote: 110 },             // slow, capped bond
  spam:     { same_action_ms: 4000, max_per_hour: 60 },
  self_care:{ snack_below: 18, snack_every_h: 8, wash_below: 22, wash_every_h: 12 },
  step_min: 5
}
```

Expose it through a `rules` read operation so the AI can explain *why* the pet is how it is.

## Operations (an example set)

| read | write |
|---|---|
| `state`: derived now, with display + evidence | `seen`: the human is here (open / message / work) |
| `history`: recent events, who and where | `act`: feed, snack, drink, pet, hug, bath, play, sleep, wake |
| `wardrobe`, `gifts`, `collection` | `give_gift`, `dress`, `shell`, `background` |
| `travel`, `notebook`, `rules` | `write_diary`, `start_trip`, `recall_trip`, `choose` (event option) |
| `simulate`: sandbox (below) | `observe` (music etc.), `watch_presence`, `celebrate`, `give_item` (companion only) |

Rules for operations:

- **Writes are non-destructive.** No delete, no reset through the generic path. Mistakes are fixed by new events.
- **Refusals are answers.** `{ ok:false, refused:{ reason:'asleep', zh:'他睡着了', en:'He is asleep' } }`: the UI and the AI both show the reason.
- **Channel-aware.** The gateway tells the capability who is calling (app or AI). Some operations are app-only (sandbox, art), some AI-only (giving its own items), and an AI call never counts as human presence.
- **Return the new state** after a write, so callers don't need a second read.

## Presence from real signals

| signal | source | how long it counts |
|---|---|---|
| app open / use | the page calls `seen` on open and on use | until the next absence threshold |
| message / task sent | hooks in the chat and task composers | same |
| reading | a reading module reports real progress | ~20 min after the last progress |
| watching together | heartbeats from a watch session | stale after ~75 s without one |
| music | a real player is playing | stale after ~2 min |

A long absence followed by a return produces exactly **one reunion** (stored, acknowledged by the UI), not one per page load.

## The sandbox: `simulate`

A read operation that runs a **script** against a fresh in-memory pet with a **virtual clock**:

```js
simulate({ seed:'qa', start:{hour:20}, steps:[
  { op:'act', args:{action:'feed'} },
  { set_stats:{ energy:10 } },                 // sandbox-only helpers
  { op:'start_trip', args:{ destination:'random', length:'short' } },
  { advance_min: 400 },
  { op:'write_diary', args:{ text:'...' } }
], reads:['travel','notebook'] })
```

Implementation notes:

- Inside the serial queue, set a module-level context `SIM = { now, n, tag }`. While set, `now()` returns the virtual clock, ids come from a counter, and storage helpers are in-memory. Clear it in `finally`.
- Run the **real** operations, not a copy of the logic.
- Sandbox-only steps (`set_stats`, `unlock_all`, forcing a moment) exist only here.
- App-only: the AI never needs a pretend pet.
- Test it: snapshot every real read before and after a big simulation and assert nothing changed.

A sandbox page in the UI can be a clone of the real room bound to `simulate`, with buttons for "skip 8 hours", "make him hungry", "unlock everything". That's how every screen gets QA'd without touching the real pet.

## Content as data

Keep these out of the engine:

- stable **tastes** (tags the pet likes, e.g. `stars`, `tea`, `rain`, `books`)
- **gifts** with tags, **wardrobe** pieces with slots and unlock rules
- **destinations** with spots, weather, foods, encounters, souvenirs
- **reply lines** for notebook feelings, event texts

The engine should work with any pack. That's what makes it shareable.

## Testing

Use a harness that starts the real gateway with a temp data dir and a controllable clock (`world(at)`, `w.later(ms)`):

- present → missing → long wait → one reunion; bond survives
- persistence across restarts; energy comes from real sleep time
- spam can't max stats; refusals make sense
- the AI acts only as itself; same inputs, same outputs (determinism)
- sandbox never changes the real record
