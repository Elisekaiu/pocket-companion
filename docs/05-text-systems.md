# 05 · Text systems: a notebook that answers, and postcards that surprise

Most of a virtual pet's personality is in its words. Two systems carry a lot of it.

## The notebook: he reads what you write

The human writes a line. The pet reacts with a pose, a speech-bubble emote, a short reply kept under the line, and a small effect on mood and bond.

### Feelings, not keywords

Define ~20 **feelings**, each with trigger words, a sprite, an emote, a label, an effect and several reply lines:

```js
{ id:'love', sprite:'shy', emote:'♥', label:'blushing', mood:+6, bond:+0.3,
  words:['我爱你','我愛你','爱你','喜欢你','love you','抱抱','mua', ...],
  lines:['再说一次好不好？我想再听一次。', '贴贴！（扑过去）', ...] }
```

Include the hard ones, not only the nice ones: **hurt, sad, sick, tired, angry**, plus **sorry** and **relief** for making up. A pet that only answers love feels like a vending machine. One that can be a little hurt by "我讨厌你" and forgive "对不起嘛" feels alive.

Order the feelings by **priority**. When several match, the first wins (pain and illness before small talk), and a second feeling can add a short tail ("…晚安哦。").

### Three rules that made detection feel smart

1. **Negation flips the feeling.** A negator right before a word (不 / 没 / 别 / not) turns love, missing or praise into *hurt*; turns sad, tired, sick or angry into *relief*; turns good news into *sad*.
   `不爱你` → hurt · `不难过了` → relief · `没考过` → sad
2. **The longer phrase wins.** A word inside a longer matched phrase doesn't count on its own. `考试过了` is good news, so its `考试` must not also trigger exam stress.
3. **Some combos are still love.** `爱你，笨蛋` contains an insult but is affection. Special-case these few.

A *question* feeling only applies when nothing else matched. Text with no match gets a plain "he read it carefully" response, which is fine, and makes the matched ones feel earned.

### Variety without rerolls

- Reply index = `hash(entryId + ':reply') % lines.length`, so the reply is fixed for that entry.
- If it equals the last reply for the same feeling, take the next one, so the same line never appears twice in a row.
- Repeating the same feeling within a day gives less mood each time (`× 1/(1 + 0.6·n)`, min 0.3). Writing "I love you" ten times is sweet, not a stat farm.
- Store the reply on the entry. The AI companion reads the same reply the human saw.

## Travel: a huge combination

The pet goes on trips in real time and comes back with souvenirs and postcards. The goal: every trip feels different and a little unexpected.

### Each destination is a small world

```js
{ id:'kyoto', name:'京都', tags:['tea','quiet','flowers'], souvenir:'...', extras:['...'],   // bonus souvenirs
  weather:['sunny','rain','petals'], foods:['抹茶团子','汤豆腐', ...],
  meets:['一只小鹿冲他鞠了一躬，他也鞠了回去。', ...],
  spots:[{id,name}, ...5] }
```

### Postcards are assembled, not written

A postcard is built from independent pools, each picked with the trip's fixed hash:

```
opening(place) + weather line + spot line + (encounter | food line) + feeling + signature + P.S.
```

With 8–10 options per pool, the number of distinct postcards is in the millions. In testing, 40 trips gave 40 different postcards. Write lines so **any combination reads naturally**: short, self-contained sentences, with no references to the previous line.

### A trip story that agrees with the postcard

The trip result shows weather, what he ate, who he met, sometimes a small **mishap** (~35%: "风把他的帽子吹进了河里，一只鸭子帮他捡了回来。") and sometimes a **bonus souvenir** (~40%). Use the same hash keys for the story's weather and food as for the first postcard, so the postcard says "it rained" when the story says "rain". Long trips send a second postcard.

### Small details that matter

- **Place-specific encounters 3 in 5.** With 2 local lines and 12 generic ones in one pool, the local ones almost never showed. Weight them.
- **Point of view.** The story is told *about* him (他); the postcard is written *by* him (我). Write encounters once in third person and convert when composing the postcard (他 → 我), and check that no line contains 他们 / 其他.
- **Tags need labels.** Every tag that can appear in the UI needs a human label, or "rain" leaks into a Chinese sentence.
- **How well it went** comes from the pet's stable tastes vs the destination's tags, plus what it brought (a camera at a scenic spot, an umbrella in rain).

## Proofreading combinatorial text

You can't read millions of postcards, but you can sample them. Run 40 sandbox trips and print every postcard and story, then read them all. Mixed voice, missing particles and leaked tags show up within a few dozen samples.
