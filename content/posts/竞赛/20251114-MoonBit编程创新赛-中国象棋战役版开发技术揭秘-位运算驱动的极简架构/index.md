---
title: "2025年全球 MoonBit 编程创新赛：《中国象棋战役版》开发技术揭秘：位运算驱动的极简架构"
date: 2025-11-14 00:00:00
update: 2025-11-14 00:00:00
author: Spaceack
categories: ["竞赛"]
tags:
  - 游戏
  - 算法
  - moonbit
---

{{< image src="01-缘起.png" alt="MoonBit 编程创新赛与中国象棋战役版" width="640" height="270" >}}

## 一、缘起：从井子棋到象棋战役
- 2024年MGPIC赛事中，CS赏金猎手战队曾基于wasm4开发井子棋游戏，却因平台限制无法实现复杂象棋逻辑而遗憾搁置。2025年赛事规则迎来关键突破——允许结合JavaScript构建前端界面，这为复杂交互打开了大门。

- 战队迅速抓住机遇，提出“原生 WebAssembly 实现象棋 AI 内核 + JavaScript 渲染棋盘”的极简架构。通过合理抽象与状态压缩，将庞大规则体系收敛为单函数、单整数的通信模型。凭借这一高度凝练的设计，CS 赏金猎手战队成为本届赛事首个提交完整作品的团队，也成为这场技术探索中“第一个吃螃蟹的人”。

- 灵感不止于技术。赛前观影《戏台》中“垓下之战”的悲壮叙事，激发了我们构建架空剧情的创意：玩家化身项羽，孤身闯阵，完成“一骑当千”的史诗战役。得益于此前井子棋的经验，仅用 8 小时便完成了 WASM 内核的初步框架搭建。

首先我们使用 MoonBit 的语法定义游戏状态结构和实例化对象，语法简洁且实用。
{{< image src="02-游戏状态结构.png" alt="MoonBit 定义 GameStat 游戏状态结构" width="480" height="561" >}}



```js
struct GameStat {
  mut x : UInt // 红车的 x 坐标， 范围 0~8
  mut y : UInt // 红车的 y 坐标， 范围 0~9
  mut win : Bool // 是否胜利，满足一定回合数则为胜利
  mut is_move : Bool // 红车是否可以移动
  // col : UInt      // 棋盘列数
  // row : UInt      // 棋盘行数
  gamemap : FixedArray[FixedArray[Char]] // 棋盘
  bpos : Map[(UInt, UInt), Char] // 棋子位置，类型
  rng : @random.Rand // 随机数
  mut step : Int // 回合数计数器
  ctype : Map[String, UInt] // 棋子类型
}

let gs : GameStat = GameStat::new()
```


## 二、核心挑战：32 位整数的极限编码
{{< image src="03-位段编码.png" alt="32 位整数的位段编码设计" width="420" height="497" >}}

WASM 与 JavaScript 之间的函数调用存在一个硬性限制：参数与返回值只能是数字（通常是 32 位无符号整数），且无法直接传递结构体、字符串或数组。这意味着：
>玩家棋子坐标、敌方棋子位置、棋子类型、游戏状态（胜利/可移动）、AI 决策结果…… 所有这些信息，必须压缩进一个 32 位整数中！

首先是传参，即用户落子点坐标。
使用 moonbit 进行解析
```js
fn decodePos(pos : UInt) -> (UInt, UInt) {
  let x : UInt = pos % 9
  let y : UInt = pos / 9
  return (x, y)
}
```
然后是返回值，即AI生成三个棋子的落子点和棋子类型，编码到结果。
我们对32位整数进行极致压缩，实现状态与操作的完美融合：

|位段|位宽|用途|
|-|-|-|
|31|1|胜利状态|
|30|1|落子判断|
|29-0|30|操作编码（两种编码方式）|


情况1：敌方棋子生成（每3回合触发）
```
[29-20] [19-10] [9-0]
  1st      2nd      3rd
```
每个棋子10位：位置(7位) + 类型(3位)
位置编码：pos = x + y * 9（范围0-80，7位足够）
类型编码：
  B（兵）= 0, S（士）= 1, M（马）= 2
  X（象）= 3, W（王）= 4, C（车）= 5

情况2：敌方棋子移动（非生成回合）
```
[29-18] [17-12] [11-6] [5-0]
  0        dst      src      0
```
移动编码12位：目标位置(6位) + 源位置(6位)
坐标编码：index = x + y * 9（范围0-80，但仅用6位表示0-63）
剩余18位：全为0（预留扩展）

位操作实现:
```js
///|生成敌方棋子 - 每个棋子10位，3个棋子共30位
fn gchess(ct : UInt, ge : Bool) -> UInt {
  let mut result : UInt = 0
  let pos_bits : Int = 7        // 位置信息：7位（支持0-127，实际棋盘0-89）
  let type_bits : Int = 3       // 类型信息：3位（支持8种棋子类型）
  let piece_bits : Int = pos_bits + type_bits  // 每个棋子10位
  
  // ... 棋子生成逻辑 ...
  
  for a in se {
    let piece_code : UInt = (a.0 << 3) | a.1  // 位置左移3位，与类型信息合并
    result = (result << 10) | piece_code      // 左移10位并入结果
  }
  merge_win_move() | result  // 与状态位合并
}
```

## 三、单一入口函数：极致的调用效率
单一入口函数不是简单的"棋子移动函数"，而是一个状态驱动的决策引擎。它通过检查当前游戏状态（gs.step, gs.win, gs.is_move）来决定执行路径，将游戏逻辑、状态转换和AI决策全部封装在单一函数中。这种设计避免了多函数调用带来的额外开销，使通信成本降至最低。
{{< image src="04-单一入口函数.png" alt="单一入口函数的状态机流程" width="480" height="476" >}}

```js
// 核心函数：用户落子 → AI决策
fn 单一入口函数(落子坐标 : UInt) -> UInt {
  // 1. 解码用户落子坐标
  let (x, y) = decodePos(落子坐标)
  
  // 2. 验证落子合法性（水平/垂直移动）
  if !check_move_chess(x, y) {
    return 0 // 无效落子
  }
  
  // 3. 更新红车位置
  gs.gamemap[gs.x][gs.y] = '0'
  gs.x = x; gs.y = y
  gs.gamemap[x][y] = 'x'
  
  // 4. 检查是否吃掉黑子
  if gs.bpos.get_or_default((x,y), 'n') != 'x' {
    gs.bpos.remove((x,y))
  }
  
  // 5. 检查游戏结束
  if checkLose() {
    return merge_win_move() | 0 // 失败状态
  }
  
  // 6. 回合计数
  gs.step += 1
  
  // 7. 胜利条件（8回合后）
  if gs.step >= 8 {
    gs.win = true
    return merge_win_move() // 胜利状态
  }
  
  // 8. 3回合生成新棋子
  if gs.step % 3 == 0 {
    return gchess(gs.ctype.get_or_default("B", 0), true)
  }
  
  // 9. 其他回合：AI移动棋子
  let (src, dst) = ai_move_chess()
  return merge_win_move() | moveChessEncode(src, dst)
}
```
函数内部实现了游戏状态机，将复杂的游戏逻辑转化为状态转换：
```js
if checkLose() { ... }  // 检查失败状态
else if gs.step >= 8 { ... }  // 胜利条件
else if gs.step % 3 == 0 { ... }  // 棋子生成
else { ... }  // AI移动
```
1. 这种状态机设计使函数逻辑清晰、执行路径明确，避免了复杂的条件嵌套，同时确保了函数执行路径的确定性。
2. 单一入口函数被设计为无副作用的纯函数（除了修改GameStat实例），这意味着：
 - 无需担心函数调用时的状态污染
 - 可以安全地进行并行调用（虽然本项目不需要）
 - 便于单元测试和验证


## 四、性能与哲学：大道至简，重剑无锋
设计哲学
> "当规则复杂时，用位运算简化；当接口受限时，用编码压缩。"

该篇程序涉及没有复杂的算法，仅使用简单的顺序，判断，循环语句和位运算就可实现稍稍复杂的小游戏。
正所谓：大道至简 重剑无锋 大巧不工！
Moonbit 是一门简洁实用的编程语言。我将继续关注 Moonbit 技术生态的发展，并希望能够为它的成长和壮大贡献自己的力量。

欢迎大家来赏玩体验 https://moonbitlang.github.io/MoonBit-Code-JAM-2025/
