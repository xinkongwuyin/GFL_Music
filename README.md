# 🎵 GFL_Music — : Hit the Note!  

## 🧩 Game Overview | 游戏简介  
It is a simple yet engaging rhythm game made with Unity.  
Players control a character to **hit notes in sync with the music**.  

**《FALL》** 是一款基于 Unity 开发的节奏类游戏。  
玩家需要操作角色根据节奏**击中不同轨道的音符**。  

---

## 🕹 Gameplay | 操作说明  
- **F** → Upper Track（上轨音符）  
- **Space** → Middle Track（中轨音符）  
- **J** → Lower Track（下轨音符）  


## ⚙️ Game Structure | 游戏结构  
> 以下总结自项目思维导图与代码架构  

| 模块 | 功能说明 | 英文说明 |
|------|-----------|----------|
| **GameController** | 控制游戏主流程：读谱、生成音符、计时、暂停、继续、退出 | Controls main logic: chart loading, note spawning, timing, pause & resume |
| **DataTransfer** | 负责全局变量传递，如 deltaTime、HoldTime、NoteSpeed | Stores global data such as deltaTime, HoldTime, NoteSpeed |
| **Character Controller** | 角色动画状态机，击打音符时播放攻击动画，可打断当前状态 | Character animation controller with interruptible attack states |
| **Tap / Hold Notes** | 两种音符类型：Tap为单击，Hold需持续按住直至结束 | Two note types: Tap (single hit), Hold (press and hold to complete) |
| **判定窗口 / Judgement Window** | 根据生成时间与当前游戏时间计算偏差，用于判定Perfect/Good/Miss | Uses note spawn time and current time to calculate hit accuracy |
| **UI System** | 由多个 Panel 构成：主界面、游戏界面、暂停界面 | Composed of Panels for main menu, gameplay, and pause menu |
| **音乐同步 / Music Sync** | 所有音符生成与移动都基于音乐的时间戳（chart 数据） | Notes are spawned and moved according to music timestamps |
| **Pause System** | 使用 TimeScale 与 DataTransfer.NoteSpeed 控制暂停逻辑 | Uses TimeScale & NoteSpeed to freeze or resume game flow |

---

## 💡 Core Gameplay Logic | 核心逻辑
1. **读谱 (LoadChart)**  
   - 从 TextAsset 读取每一行时间戳与音符类型。  
   - 解析出时间（timeStamps）、轨道数量（noteQuantity）、类型（noteType）。  
   - 生成音符对象并加入判定队列。

2. **音符生成 (Spawn Notes)**  
   - 按照 `timeStamps[index]` 判断时间是否到达。  
   - 到时生成相应类型的 Tap / Hold 音符。  
   - 音符携带自身轨道位置与类型。

3. **音符判定 (Judgement System)**  
   - 当音符进入判定窗口时，根据玩家按键进行判断。  
   - 若击中 → 触发动画与加分；  
   - 若超时未击中 → Miss，扣血并清除音符。
  

---

## 🧠 Design Logic Summary | 思维导图总结  

- **人物系统（Character）**：  
  - 拥有“左 / 中 / 右”三轨对应的击打动作。  
  - 动画状态机可打断上一个状态，实现连续击打。

- **音符系统（Notes）**：  
  - 分为 Tap 与 Hold 两种类型。  
  - 通过判定窗口控制命中与 Miss；  
  - Hold 音符需持续按住至尾部，松开判定为 Miss。

- **游戏控制（GameController）**：  
  - 负责时间同步、读谱、音符生成、分数记录、场景切换。  
  - 计时器通过 `myTime` 控制，与音乐播放同步。  
  - 使用 `DataTransfer` 进行跨脚本通讯。

- **游戏设置（Game Settings）**：  
  - 暂停（Pause）  
  - 退出（Quit）  
  - 加载（Load）  
  - 计时（Timer）

- **UI 设计（Panels）**：  
  - 主界面（MainMenu）  
  - 游戏中界面（PlayingPanel）  
  - 暂停界面（PausePanel）  

---

## 🧩 技术与实现 | Technical Implementation  
- **Engine:** Unity 2022  
- **Language:** C#  
- **Core Scripts:** GameController, DataTransfer, HoldScript, TapScript, NoteMover  
- **UI Framework:** Unity UI System (Canvas + Panels + Buttons)  
- **Audio Sync:** AudioSource.time 与谱面时间对齐  

---

![游戏截图1](https://github.com/xinkongwuyin/GFL_Music/blob/main/GameScene1.png?raw=true)
![游戏截图2](https://github.com/xinkongwuyin/GFL_Music/blob/main/GameScene2.png?raw=true)
![游戏截图3](https://github.com/xinkongwuyin/GFL_Music/blob/main/GameScene3.png?raw=true)
