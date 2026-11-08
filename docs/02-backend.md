# 02 · 后端

一个小而朴素、很难写错的后端。

## 结构

```
┌──────────── 你的 App ────────────┐        ┌──── AI 伴侣 ─────────┐
│ 页面：房间、按钮、弹出页          │        │ 聊天运行时 / 工具     │
└──────────────┬───────────────────┘        └──────────┬───────────┘
               │ POST /api/<capability>/<operation>    │ MCP：read / write
               └───────────────┬───────────────────────┘
                       ┌───────▼────────┐
                       │ capability 模块 │  manifest + handle({operation, args, helpers})
                       │ （近乎纯 JS）   │  串行队列 · 通过 helpers 读写 JSON
                       └───────┬────────┘
                               │
                       一份 JSON 文档：{ events: [...], checkpoint, ... }
```

一个 **capability 模块**负责这只小宠物。它导出 `manifest`（操作列表，每个是 `read` 或 `write`，带说明）和一个 `handle()` 函数。一个很薄的网关把它暴露两次：HTTP 给 App，MCP 给 AI。两条路走到同一份代码、同一份文档，所以只有一个真相来源。

所有操作通过**串行队列**一个一个执行，两个照顾者同时操作也不会把写入搅在一起。

## 事件日志

发生的每件事都追加成一个事件：

```js
{ id, at, type: 'act' | 'seen' | 'gift' | 'diary' | 'trip_start' | 'dress' | ...,
  by: 'human' | 'companion' | 'self',  via: 'app' | 'chat' | 'reading' | 'watch' | ...,
  args: {...},  meta: {...} }      // meta：当下做出的决定（反应、回复、结果）
```

状态从不存成“当前数值”，而是**推算**出来：

```js
state = fold(blank(), events, now)
// fold：按时间顺序处理每个事件
//   advance(state, event.at)   – 让时间流过：饿、精力、清洁、想你
//   apply(state, event)        – 这个事件做了什么
// 最后 advance(state, now)
```

`advance` 以小步长（5 分钟就很好）走过时间。这样不管你每分钟看一次还是两天后才看，睡眠、清醒时段、下限和阈值的表现都一样。

### 为什么用事件日志

- **真实时间是白送的**：读取就是“推算到现在”。
- **可解释**：每个数字都能追溯到事件，每个显示的状态都能给出证据。
- **好调参**：在一张参数表里改一个速率，整段历史会一致地重新推算。
- **两个写入者也安全**：追加不会冲突。

### 保持快：检查点

每次读取都推算一整年的事件会变慢。定期把旧的部分折叠成一个**检查点**（时间 T 时的状态），只保留之后的事件。读取变成 `fold(checkpoint, recentEvents, now)`。

### 决定一次，存下决定

有些东西在某一刻决定后就永远不能变：礼物的反应、小本子的回复、旅行去哪里、带回哪张明信片。在**写入事件的那一刻**用固定哈希算好，存进 `meta`。以后改代码也改写不了过去，AI 读历史时看到的，也正是人当时看到的。

## 参数集中在一处

所有速率、阈值、上限都放在一张表里，例如：

```js
TUNING = {
  absence:  [{from_h:0,level:'present'}, {from_h:2,level:'a_little'}, {from_h:6,level:'waiting'},
             {from_h:12,level:'missing'}, {from_h:24,level:'long_wait'}],     // 每级带心情惩罚
  reunion:  { after_h: 6 },                               // 离开 6 小时后回来算重逢，只显示一次
  rates_per_h: { fullness_awake: 6.2, fullness_asleep: 2.4, energy_awake: 5, energy_asleep: 15, ... },
  floors:   { fullness: 8, hygiene: 15, energy: 4, health: 55 },   // 不会更低
  affection:{ daily_cap: 3, asymptote: 110 },             // 慢慢来、有上限的亲密度
  spam:     { same_action_ms: 4000, max_per_hour: 60 },
  self_care:{ snack_below: 18, snack_every_h: 8, wash_below: 22, wash_every_h: 12 },
  step_min: 5
}
```

再通过一个 `rules` 读操作把它公开出来，AI 就能解释它**为什么**现在是这样。

## 操作（一组示例）

| 读 | 写 |
|---|---|
| `state`：现在推算出的状态，带显示和证据 | `seen`：人来了（打开 / 发消息 / 工作） |
| `history`：最近的事件，谁、在哪里 | `act`：喂食、零食、喝水、摸摸、抱抱、洗澡、玩、睡觉、起床 |
| `wardrobe`、`gifts`、`collection` | `give_gift`、`dress`、`shell`、`background` |
| `travel`、`notebook`、`rules` | `write_diary`、`start_trip`、`recall_trip`、`choose`（事件选项） |
| `simulate`：沙盒（见下） | `observe`（音乐等）、`watch_presence`、`celebrate`、`give_item`（仅 AI） |

操作的规则：

- **写入不具破坏性。** 通用接口里没有删除、没有重置。出错就用新事件来修正。
- **拒绝也是回答。** `{ ok:false, refused:{ reason:'asleep', zh:'他睡着了', en:'He is asleep' } }`，App 和 AI 都把原因展示出来。
- **区分调用方。** 网关告诉模块是谁在调用（App 还是 AI）。有些操作只给 App（沙盒、美术包），有些只给 AI（送出它自己的物品）；AI 的调用永远不算人的陪伴。
- **写入后返回新状态**，调用方不用再读一次。

## 来自真实信号的陪伴

| 信号 | 来源 | 算多久 |
|---|---|---|
| 打开 / 使用 App | 页面在打开和使用时调用 `seen` | 直到下一个离开阈值 |
| 发出消息 / 任务 | 聊天和任务输入框里的钩子 | 同上 |
| 读书 | 读书模块上报真实进度 | 最后一次进度后约 20 分钟 |
| 一起看片 | 看片会话的心跳 | 约 75 秒没有心跳就过期 |
| 音乐 | 播放器真的在播 | 约 2 分钟后过期 |

离开很久再回来，只会产生**一次重逢**（存下来，由 App 确认已显示），而不是每次刷新页面都重逢一次。

## 沙盒：`simulate`

一个读操作：在一只全新的内存小宠物上，用**虚拟时钟**跑一段**脚本**：

```js
simulate({ seed:'qa', start:{hour:20}, steps:[
  { op:'act', args:{action:'feed'} },
  { set_stats:{ energy:10 } },                 // 只有沙盒才有的辅助步骤
  { op:'start_trip', args:{ destination:'random', length:'short' } },
  { advance_min: 400 },
  { op:'write_diary', args:{ text:'...' } }
], reads:['travel','notebook'] })
```

实现要点：

- 在串行队列内部设置一个模块级上下文 `SIM = { now, n, tag }`。它存在时，`now()` 返回虚拟时间，id 由计数器生成，存储 helpers 换成内存版。在 `finally` 里清掉。
- 跑的是**真正的**操作，不是逻辑的复制品。
- 沙盒专用步骤（`set_stats`、`unlock_all`、强制触发事件）只在这里存在。
- 只给 App 用：AI 不需要一只假的宠物。
- 要测试：大规模模拟前后，对所有真实读取做快照，断言什么都没变。

App 里的沙盒页面可以是真房间的克隆，只是绑定到 `simulate`，再加上“快进 8 小时”“让他饿一点”“全部解锁”这样的按钮。每一个页面都是这样做 QA 的，从不碰真正的小宠物。

## 内容即数据

这些都不要写进引擎：

- 稳定的**喜好**（它喜欢的标签，例如 `stars`、`tea`、`rain`、`books`）
- 带标签的**礼物**、带部位和解锁规则的**衣柜**
- **目的地**：景点、天气、食物、偶遇、纪念品
- 小本子各种情绪的**回复台词**、事件文案

引擎应该能跑任何一份内容包，这样它才能被分享。

## 测试

用一个测试框架启动真正的网关，带临时数据目录和可控时钟（`world(at)`、`w.later(ms)`）：

- 在身边 → 想你 → 等了好久 → 只有一次重逢；亲密度保留
- 重启后数据还在；精力来自真实的睡眠时间
- 刷不满数值；拒绝的理由说得通
- AI 只以自己的身份行动；同样的输入得到同样的输出（确定性）
- 沙盒永远不改变真实记录
