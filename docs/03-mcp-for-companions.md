# 03 · An MCP server for AI companions

The same operations the app uses can be exposed as MCP tools. Then the AI companion can check on the pet, feed it, read the notebook and send it on a trip from inside a conversation.

## Two generic tools are enough

Rather than one tool per operation, expose a small generic pair:

```
home_read({ capability, operation, args })    // any 'read' operation
home_write({ capability, operation, args })   // any non-destructive 'write' operation
home_capabilities()                           // list capabilities, operations, descriptions
```

The manifest descriptions *are* the documentation the model reads, so write them for a model: what the operation does, when to use it, what it refuses. A dedicated server with one tool per operation (`pet_state`, `pet_feed`, ...) also works and is friendlier for apps that show tool lists.

## Give the model a summary, not a database

The `state` read returns a `for_companion` block: a few short lines, written to be read by a model.

```
Little one is asleep (since 23:10, 7 h). Fullness 41, energy 88, mood 70.
You were last seen 22:58 (app). He misses you a little.
Last trip: a rainy city, best moment: the old bookshop; ate fish and chips; shared an umbrella with a girl.
Notebook: "今天好累" → he made you tea and said to rest.
```

The model can ask for details (`history`, `rules`) when it needs them, but most turns only need this.

## Boundaries the server enforces (not the prompt)

- **Identity.** Calls from the AI are tagged `by:'companion'`. They never mark the human as present and never trigger a reunion.
- **No destructive tools.** No reset or delete. A mistake is corrected by a new event.
- **App-only operations stay app-only.** Sandbox, art packs and UI acknowledgements are refused on the AI channel.
- **Same caps for everyone.** The AI can't bypass spam limits or daily bond caps.
- **Gifts are the AI's own.** "Give an item" is AI-only and records who gave it and the note it wrote.

## "Let the companion look after itself"

The pet's state gives an AI companion something concrete to care about between conversations, and a reason to bring it up naturally:

- In the morning: read `state`; if the pet is hungry, feed it and mention it ("I gave him breakfast; he's been waiting for you").
- When the human writes in the notebook, the companion can read it, and the pet's reply, and react.
- When a trip comes home, the companion has a postcard and a story to share.

It also works as an emotional mirror for the companion itself: a visible little self with needs, sleep, moods and memories that persists across sessions. Some people may want to give the AI that kind of continuity.

Keep it honest: every action the companion takes appears in history with its name on it.

## Minimal server sketch

```js
// mcp-server.mjs — illustrative
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import * as pet from './capabilities/pet.mjs';

const call = (operation, args) => queue.run(() =>
  pet.handle({ operation, args, manifest: pet.manifest, helpers: { ...storage, channel: 'companion' } }));

server.tool('home_read',  { operation:'string', args:'object' }, ({operation,args}) => {
  if (pet.manifest.operations[operation]?.kind !== 'read') throw new Error('not a read');
  return call(operation, args);
});
server.tool('home_write', { operation:'string', args:'object' }, ({operation,args}) => {
  if (pet.manifest.operations[operation]?.kind !== 'write') throw new Error('not a write');
  return call(operation, args);
});
```

The important part is not the transport. It is that the app and the AI hit **the same `handle()` and the same storage**, through one queue.
