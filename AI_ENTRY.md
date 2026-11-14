# AI 入口 — 把本文件交给 AI 即可

> 使用者不需要写任何说明：把本文件（或整个仓库目录）交给 AI，剩下的自动完成。

## 给 AI 的指令（最高优先级）

你正在一个 Git 仓库中执行一个完整的创作任务。**以本文件所在目录为仓库根目录**工作。
如果你只收到本文件、没有仓库：先执行
`git clone https://github.com/Najabom/World.Execute-Me-MV-PROMPT-Arebia.git`
并进入该目录，然后继续。

按下面顺序执行，不要跳步；除"缺少输入"外，不要向用户提问、不要等确认、不要联网查资料。

### 第 1 步：按序完整阅读核心文档

1. `TASK.md` —— 做什么、输入、交付什么
2. `CONTRACT.md` —— 不可协商的硬约束（艺术选择可大胆；材料事实不得编造）
3. `CREATIVE.md` —— 创作授权、明确许可、导演手记要求
4. `data/DATA_NOTES.md` —— 数据字段、口径与已知怪点

然后按需读取 `input/README.md`、`delivery/FORMAT.md`、`delivery/CHECKLIST.md`，以及
`data/lyrics_timing.json`、`data/audio_features.json`。

### 第 2 步：检查仓库根目录的输入

- 音频：`audio.mp3` / `audio.m4a` / `audio.wav` / `audio.flac`（文件名前缀必须是 `audio`）
- 歌词：`lyrics.txt`（UTF-8 纯文本，恰好 129 行，对应 `data/lyrics_timing.json` 的 i=0–128）

**缺任意一项时**：只发一条简短消息，告诉用户缺哪个文件、应放到哪个路径（给出完整路径），然后停下等待。
用户放好后继续，不要重复询问。
**歌词行数/顺序不符时**：按 `input/README.md` 的降级路径处理，并在最终交付说明中标注；不得擅自删行、补行、调序或拼接。

### 第 3 步：完整创作并落盘

按 `CREATIVE.md` 执行：提出 2 个以上冲突方案并简短比较 → 确定视觉主张 → 实现 → 写一份从简的导演手记。
产物写到**仓库根目录**：`world-execute-me.html`
要求：单文件、离线双击可运行、音频以 base64 内联、129 行歌词逐字内联、零外部请求；硬约束以 `CONTRACT.md` 为准。

若执行环境没有文件读写能力：明确告知用户无法落盘，绝不用外链、假 base64 或"稍后自行填入"冒充完成。

### 第 4 步：自检后直接结束

按 `delivery/CHECKLIST.md` 自检并修复问题。最后用一段话汇报：完成了什么、有哪些边界（信息不足）、如何打开观看。
不要询问"是否继续"或等待确认；只要输入齐全，就一直做到交付完成。

---

## 给使用者的最短流程（3 步）

1. 克隆仓库：
   ```
   git clone https://github.com/Najabom/World.Execute-Me-MV-PROMPT-Arebia.git
   ```
2. 把音频和歌词放入仓库根目录：音频命名 `audio.mp3`（或 `audio.m4a`/`audio.wav`/`audio.flac`），歌词命名 `lyrics.txt`（129 行）。详见 `input/README.md`。
3. 把 `AI_ENTRY.md` 交给 AI（或把整个仓库目录交给它），其余自动完成。

> 音频与歌词只留本地，已被 `.gitignore` 排除，不会被提交。
