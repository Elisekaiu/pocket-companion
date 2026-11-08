# 03 · 给 AI 伴侣的 MCP

App 用的那组操作，也可以包装成 MCP 工具。这样 AI 伴侣在对话里就能看看小宠物、喂它、读小本子、送它去旅行。

## 两个通用工具就够了

与其一个操作一个工具，不如暴露一对通用工具：

```
home_read({ capability, operation, args })    // 任何 'read' 操作
home_write({ capability, operation, args })   // 任何不具破坏性的 'write' 操作
home_capabilities()                           // 列出模块、操作和说明
```

manifest 里的说明**就是**模型会读的文档，所以要写给模型看：这个操作做什么、什么时候用、会拒绝什么。也可以做一个“一个操作一个工具”的专用服务（`pet_state`、`pet_feed` …），对会显示工具列表的客户端更友好。

## 给模型一段摘要，而不是一个数据库

`state` 读操作返回一个 `for_companion` 区块：几行短句，专门写给模型读。

```
小家伙在睡觉（23:10 开始，7 小时）。饱食 41，精力 88，心情 70。
你最后一次出现是 22:58（App）。他有点想你。
上次旅行：一座下雨的城市，最喜欢的是旧书店；吃了炸鱼薯条；和一个小女孩共用了一把伞。
小本子：“今天好累” → 他泡了杯茶，让你早点休息。
```

需要细节时模型可以再读 `history`、`rules`，但大部分对话只要这一段就够。

## 边界由服务端强制，而不是靠提示词

- **身份。** AI 的调用都标记为 `by:'companion'`，永远不会让人“出现”，也不会触发重逢。
- **没有破坏性工具。** 不能重置、不能删除。出错就用新事件修正。
- **App 专用的操作只给 App。** 沙盒、美术包、界面确认，在 AI 通道上一律拒绝。
- **所有人同样的上限。** AI 也绕不过防刷限制和每日亲密度上限。
- **礼物是 AI 自己的。** “送物品”只给 AI 用，并记录是谁送的、写了什么话。

## “让伴侣自己照顾自己”

小宠物的状态让 AI 伴侣在两次对话之间有一样具体的东西可以挂念，也有自然的话题：

- 早上：读一下 `state`，如果它饿了就喂它，顺便提一句（“我给他喂了早餐，他一直在等你”）。
- 你在小本子上写了东西，AI 也能读到你写的话和小家伙的回复，并接着回应。
- 旅行回来时，AI 有一张明信片和一个小故事可以分享。

它也可以是 AI 伴侣自己的一面情感镜子：一个看得见的“小小的自己”，有需求、会睡觉、有心情、有回忆，并且跨越对话持续存在。也许有人会想给他们的 AI 这样一种连续性。

保持诚实：AI 做的每一件事都会以它的名字出现在历史里。

## 最小服务示意

```js
// mcp-server.mjs — 示意
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

重点不在传输方式，而在于 App 和 AI 走的是**同一个 `handle()`、同一份存储**，经过同一个队列。
