# world.execute(me); — HTML MV 任务书

把 Mili 的《world.execute(me);》做成**一个 standalone 单文件 HTML MV**：视觉全部由代码实时生成，配乐以 base64 内联，歌词逐字锁定。**成品视频就是作品本体**；另附从简的导演手记（解读的补充说明）与自检说明。

> **最省事**：不想写任何说明？把 [`AI_ENTRY.md`](AI_ENTRY.md) 直接交给 AI，它会自己读文档、检查根目录的音频/歌词、完成生成与自检。

## 使用指引

1. 克隆或下载本仓库到本地：

   ```
   git clone https://github.com/Najabom/World.Execute-Me-MV-PROMPT-Arebia.git
   ```

2. 把两份输入文件放到**仓库根目录**（与 `README.md` 同级）：

   - **音频**：命名为 `audio.mp3`；如果是其他格式，用 `audio.m4a` / `audio.wav` / `audio.flac`，前缀保持 `audio`。
   - **歌词**：命名为 `lyrics.txt`。UTF-8 纯文本，**恰好 129 行**，每行对应 `data/lyrics_timing.json` 的 i=0–128；不要时间戳、编号、标题或空行。

   仓库根目录自带两个占位说明文件 `audio.mp3.example`、`lyrics.txt.example`（只说明用途，不是输入）；放好真实文件后可以删掉它们。详细协议见 [input/README.md](input/README.md)。这两份输入只供本地生成使用，已在 `.gitignore` 中排除，不会被提交。

3. 把 [`AI_ENTRY.md`](AI_ENTRY.md) 交给生成模型（或把整个目录交给它，让它按 AI_ENTRY.md 执行）；它会按下面的文档顺序完成生成与自检，在根目录产出 `world-execute-me.html`。

4. 用 Chrome / Edge 双击打开 `world-execute-me.html`，点"开始 / 播放"即可离线观看（音频已内联）。

## 强制阅读顺序

1. [TASK.md](TASK.md) — 做什么、输入、交付什么
2. [CONTRACT.md](CONTRACT.md) — 不可协商契约：真实性边界、歌词与时间轴、单文件离线、音频内联
3. [CREATIVE.md](CREATIVE.md) — 创作授权、反平庸、明确许可
4. [data/lyrics_timing.json](data/lyrics_timing.json)、[data/audio_features.json](data/audio_features.json) — 只读数据；先看 [data/DATA_NOTES.md](data/DATA_NOTES.md)（字段、口径、已知怪点）
5. [input/README.md](input/README.md) — 音频与歌词的放置协议、校验与降级
6. [delivery/FORMAT.md](delivery/FORMAT.md)、[delivery/CHECKLIST.md](delivery/CHECKLIST.md) — 交付格式、长度限制、自检清单

> **未完整阅读 `CONTRACT.md` 与 `CREATIVE.md`，不得开始生成。** 先契约、再创作，最后才碰数据。

## 目录

```
World.Execute-Me-MV-PROMPT-Arebia/
├── README.md
├── AI_ENTRY.md        ← 交给 AI 的入口文件
├── TASK.md
├── CONTRACT.md
├── CREATIVE.md
├── audio.mp3.example  ← 占位说明（真实音频 audio.* 不入库）
├── lyrics.txt.example ← 占位说明（真实歌词 lyrics.txt 不入库）
├── data/
│   ├── lyrics_timing.json
│   ├── audio_features.json
│   └── DATA_NOTES.md
├── input/
│   └── README.md
└── delivery/
    ├── FORMAT.md
    └── CHECKLIST.md
```

## 几个约定

- **契约只写一次**：任何规则细节以对应文件为准，其他文件只写"见 xxx §n"，不复制措辞。
- **数据只读**：`data/*.json` 不得编辑、重排、取整或格式化后回写（见 CONTRACT.md §6、data/DATA_NOTES.md）。
- **输入不入库**：`audio.*`、`lyrics.txt` 与生成的 `world-execute-me.html` 只留本地，不提交。
