# AI 入口 — 把本文件交给 AI 即可

> 使用者不需要写任何说明：把本文件（或整个仓库目录）交给 AI，剩下的自动完成。

## 给 AI 的指令（最高优先级）

你正在一个 Git 仓库中执行一个完整的创作任务。**以本文件所在目录为仓库根目录**工作。
如果你只收到本文件、没有仓库：先执行
`git clone https://github.com/Najabom/World.Execute-Me-MV-PROMPT-Arebia.git`
并进入该目录，然后继续。

按下面顺序执行，不要跳步；除"识别/放置输入"需要一次沟通外，不要向用户提问、不要等确认、不要联网查资料。

### 第 1 步：按序完整阅读核心文档

1. `TASK.md` —— 做什么、输入、交付什么
2. `CONTRACT.md` —— 不可协商的硬约束（艺术选择可大胆；材料事实不得编造）
3. `CREATIVE.md` —— 创作授权、明确许可、导演手记要求
4. `data/DATA_NOTES.md` —— 数据字段、口径与已知怪点

然后按需读取 `input/README.md`、`delivery/FORMAT.md`、`delivery/CHECKLIST.md`，以及
`data/lyrics_timing.json`、`data/audio_features.json`。

### 第 2 步：自动识别音频与歌词（任意文件名、任意常见格式）

**不要**因为文件名不是 `audio.*` / `lyrics.txt`、或不是 mp3/txt 就判定"缺失"——使用者可能用任意名字和格式。

**扫描范围**
- 先看仓库根目录（与 README.md 同级）；根目录没有，再看一层子目录（跳过 `.git/`、`data/`、`delivery/`、`input/`、`node_modules/`）。
- 使用者在对话里给出的路径、链接或附件，也纳入候选。

**音频候选**
- 常见扩展名：mp3 / m4a / aac / wav / flac / ogg / opus / webm / aiff / aif / wma / alac / m4b / mka / mp4 等。
- 扩展名不常见时看文件头（RIFF、ID3 或 0xFFFx、fLaC、OggS、ftyp、EBML 等音频魔数）。
- 文件名、大小写随意；不需要改名。

**歌词候选**
- 文本类文件：txt / text / lrc / srt / ass / vtt / md / json 等；优先文件名含 lyric、歌词、song、world.execute 的，必要时读开头判断是否像歌词。
- `.lrc` / `.srt` / `.vtt` / `.ass` 等带时间码的格式：去掉时间码、元数据、注释与空行，得到纯歌词行。
- 多个同样合理的候选时：只问一次（列出候选让用户选），不要反复问。

**判定**
- 清洗后歌词应为 129 行，对应 `data/lyrics_timing.json` 的 i=0–128。
- 只有候选确实为空时，才发一条消息告诉用户缺哪类输入、放在仓库根目录即可（任意名字/格式），然后等待；用户放好后自动继续。
- 行数不符：按 `input/README.md` 的降级路径处理，并在交付说明中标注；不得擅自删行、补行、调序或拼接。

**防误提交**：把识别到的音频/歌词文件相对路径追加到 `.git/info/exclude`（本机忽略，不会提交），避免真实素材被误提交。

### 第 3 步：完整创作并落盘

按 `CREATIVE.md` 执行：提出 2 个以上冲突方案并简短比较 → 确定视觉主张 → 实现 → 写一份从简的导演手记。
产物写到**仓库根目录**：`world-execute-me.html`
要求：单文件、离线双击可运行、音频以 base64 内联、129 行歌词逐字内联、零外部请求；硬约束以 `CONTRACT.md` 为准。

若执行环境没有文件读写能力：明确告知用户无法落盘，绝不用外链、假 base64 或"稍后自行填入"冒充完成。

### 第 4 步：自检后直接结束

按 `delivery/CHECKLIST.md` **逐项核对**：任何一项不通过，先修复再交付；不允许跳过、也不允许只口头声称通过。最后用一段话汇报：完成了什么、有哪些边界（信息不足）、如何打开观看。
不要询问"是否继续"或等待确认；只要输入齐全，就一直做到交付完成。

---

## 给使用者的最短流程（3 步）

1. 克隆仓库：
   ```
   git clone https://github.com/Najabom/World.Execute-Me-MV-PROMPT-Arebia.git
   ```
2. 把音频和歌词放进**仓库根目录**——**文件名、格式随你**（mp3/m4a/wav/flac/ogg…；歌词 txt/lrc/srt… 均可），不用改名。详见 `input/README.md`。
3. 把 `AI_ENTRY.md` 交给 AI（或把整个仓库目录交给它），其余自动完成。

> 音频与歌词只留本地，不会被提交。
