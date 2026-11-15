# TASK — 做什么

把 Mili 的《world.execute(me);》做成一个 HTML MV：**一个 standalone 单文件 HTML**，视觉全部由代码实时生成，音频内联，歌词逐字呈现。**成品视频即作品本体**；另附从简的**导演手记**与**自检说明**。

## 输入

| 输入 | 要求 | 提供与校验 |
|---|---|---|
| 音频 | **必需**。放仓库根目录（任意文件名、常见格式均可，自动识别），最终必须内联进 HTML | [input/README.md](input/README.md) |
| 歌词原文 | **必需**。放仓库根目录（任意文件名/文本格式；清洗后 129 行，对应 `data/lyrics_timing.json` 的 i=0–128） | [input/README.md](input/README.md) |
| 数据 | `data/*.json` 随仓库提供，**只读**，用于对齐与校验 | 字段与怪点见 [data/DATA_NOTES.md](data/DATA_NOTES.md) |

歌词行数/顺序不符时按 [input/README.md](input/README.md) 的降级路径交付；降级不是默认形态。

## 交付什么

1. **完整 HTML 源码（作品本体）**：单文件，建议文件名 `world-execute-me.html`；必须可运行，视觉与音乐响应完整呈现
2. **导演手记（补充说明，从简）**：写作要求见 [CREATIVE.md](CREATIVE.md) §4
3. **自检结果与边界交代**：格式见 [delivery/FORMAT.md](delivery/FORMAT.md)，逐项见 [delivery/CHECKLIST.md](delivery/CHECKLIST.md)

## 完成定义（交付前逐项自查）

- **全曲覆盖**：0–211.9067 s 内任意时刻都有可渲染画面；不得只做开头或局部段落，不得在后半段空白、卡死或无限循环。
- **结构覆盖**：14 个 arrangement 段与 4 个器乐窗（含尾部 209.811–211.907）都能看出对应的处理或变化；允许极简、留白或黑屏，但必须是设计选择，不能因为"没有歌词"就不处理。
- **歌词覆盖**：129 行全部在其 start/end 窗口内出现（歌词校验通过时）。
- **播放器完整**：播放/暂停、拖动进度、静音/音量、结束状态与重播可用。
- **无假完成**：没有"稍后补充"、伪 data URI、外链或未实现分支。

## 流程

1. 提出 2 个或更多互相冲突的视觉方案并简短比较（[CREATIVE.md](CREATIVE.md) §2）
2. 确定视觉主张并执行，交付完整 HTML（效果优先）
3. 写一份从简的导演手记，说明你的解读
4. 按 [delivery/CHECKLIST.md](delivery/CHECKLIST.md) 自检（合规检查）

## 硬约束摘要（条款以 [CONTRACT.md](CONTRACT.md) 为准，这里只做导航）

- 两类陈述（材料事实 vs 艺术解读）→ CONTRACT.md §0
- 歌词锁定与时间戳不可改 → CONTRACT.md §1
- 行内节奏与听感口径 → CONTRACT.md §2
- 时间轴实现规则 → CONTRACT.md §3
- 单文件、离线、无外部请求、代码生成视觉 → CONTRACT.md §4
- 音频 base64 内联 → CONTRACT.md §5
- 数据文件只读 → CONTRACT.md §6
