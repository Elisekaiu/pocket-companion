# Pocket Companion · 口袋里的小伙伴

一份设计笔记：怎么做一只住在你和 AI 伴侣之间的像素小宠物。它既在你的 App 里，也在 AI 的工具里。

> English version: [`english` branch](https://github.com/Elisekaiu/pocket-companion/tree/english)

这不是一个拿来就能用的 App，而是一个每天都在用的小宠物背后的做法：原则、后端设计、换装系统、文字系统、像素渲染，还有一路踩过的坑。角色、美术、喜好和台词属于一段私人的关系，所以没有放进来。**系统可以共用，小伙伴应该是你自己的。**

## 它和普通的电子宠物有什么不一样

- **只对“真的在”有反应。** 它知道你来过，是因为你真的打开了 App、发了消息、读了书、一起看了片。不会为了显得热闹而假装。
- **活在真实时间里。** 饿、困、想你，都是按真实时钟从事件日志里推算出来的。不管什么时候打开，它都处在对的状态，不需要后台定时器。
- **随机，但永不重抽。** 每一个“随机”（回复、明信片、事件）都是由它的 id 算出的固定哈希。刷新不会改变发生过的事，也没人能刷出更好的结果。
- **一只宠物，两个照顾者。** 你的 App 和你的 AI 伴侣通过同一份后端接口（和 MCP）读写**同一份**状态。AI 可以喂它、送它东西、读它的小本子，但只能以自己的身份行动，不能冒充你。
- **一起做的事都算数。** 一起看片、一起读同一本书（借助 plus-one 和 coread 这样的工具，见致谢），它就会窝在沙发上或台灯下陪你们。
- **旅行是一个超大的组合。** 它按真实时间出门旅行，带回来的明信片由很多小池子拼成：地点、天气、景点、遇见了谁、吃了什么、小意外、P.S.，所以每次旅行都不一样。地点和喜好是很私人的东西，请换成你们自己的。
- **温柔，有边界。** 数值有下限，亲密度每天有上限，重复的动作收益递减；它不会死，也不会惩罚你。你离开太久，它会想你，但不会受苦。

## 目录

| | |
|---|---|
| [docs/01-principles.md](docs/01-principles.md) | 原则：后面所有设计都从这里来 |
| [docs/02-backend.md](docs/02-backend.md) | 后端：事件日志、按时间推算状态、操作接口、沙盒 |
| [docs/03-mcp-for-companions.md](docs/03-mcp-for-companions.md) | 包装成 MCP，让 AI 伴侣自己照顾小宠物（也照顾自己） |
| [docs/04-wardrobe.md](docs/04-wardrobe.md) | 像素换装：一个基准身体、只含衣服的图层、校准 |
| [docs/05-text-systems.md](docs/05-text-systems.md) | 会回应你的小本子；由很多小块拼成的旅行明信片 |
| [docs/06-pixel-rendering.md](docs/06-pixel-rendering.md) | 网页里的清晰像素：字形图集、整数缩放、统一按钮框、日夜模式 |
| [docs/07-doing-things-together.md](docs/07-doing-things-together.md) | 一起看片、一起读书：把真实的共同活动变成陪伴和心情 |
| [docs/08-pitfalls.md](docs/08-pitfalls.md) | 踩过的坑 |
| [art/ART-PROMPT-KIT.md](art/ART-PROMPT-KIT.md) | 生图提示词模板，用来设计你自己的小伙伴（英文，生图模型对英文提示词更稳） |

## 怎么用这些笔记

1. 先设计你的小伙伴（`art/ART-PROMPT-KIT.md` 第 0 节）：非人类、缩小后还认得出、属于你。
2. 读 `01-principles.md`，决定要保留哪些原则。它们会影响后面的一切。
3. 后端用“事件日志 + 纯函数”来做（`02-backend.md`），把内容（喜好、地点、台词）放在数据里，不要写死在代码里。
4. 让 AI 伴侣通过同一组操作接入（`03-mcp-for-companions.md`）。
5. 核心循环顺了，再加换装、文字系统和渲染细节。

## 致谢

- “一起看片”的想法来自 [corcordata/plus-one](https://github.com/corcordata/plus-one)：和你的 AI 伴侣一起看网上的任何东西。
- “一起读书”的想法来自 [meowmana/coread](https://github.com/meowmana/coread)：AI 与人类的共读批注系统。
- 像素字体使用 TakWolf 的 [Fusion Pixel Font](https://github.com/TakWolf/fusion-pixel-font)（SIL OFL 1.1）。
- `art/ART-PROMPT-KIT.md` 由这只小宠物所属的 AI 伴侣撰写。

## 许可

本仓库文字采用 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.zh-hans)：可以使用、修改，请注明出处。
