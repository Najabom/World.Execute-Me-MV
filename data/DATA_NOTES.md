# DATA_NOTES — 数据字段、口径与已知怪点

> 使用前必读。本文件说明数据怎么读、哪里不可靠；**不构成创作约束**（创作见 [CREATIVE.md](../CREATIVE.md)，硬约束见 [CONTRACT.md](../CONTRACT.md)）。

## 文件清单

| 文件 | 内容 | 规模 |
|---|---|---|
| [lyrics_timing.json](lyrics_timing.json) | `meta` / `lines` / `instrumentalWindows` | 129 行；4 个器乐窗 |
| [audio_features.json](audio_features.json) | `meta` / `beats` / `arrangement` / `perLineEnergy` / `energyCurve` / `silenceGaps` | 14 段；129 条；424 行 |

## 只读规则

JSON 只读：不得编辑、重排、取整、改名，也不得"格式化后回写"。

## 字段字典

| 字段 / 数据块 | 含义 | 使用注意 |
|---|---|---|
| `meta`（lyrics） | 曲名、艺人、专辑、年份、BPM、时长、行数 | `meta.title` 为曲名元数据，允许保留 |
| `lines` | 129 条行级时间窗：`i` / `start` / `end` / `section` / `emphasis` | 不含歌词文本；按 `i` 升序，行级对齐 |
| `instrumentalWindows` | 4 段无歌词器乐窗 | 窗内规则见 CONTRACT.md §1 |
| `meta`（audio） | 曲名、时长、BPM、独立校验值、节拍网格 | `bpmIndependent=129.99`；被否决的 130.835 不要使用 |
| `meta.grid` | bpm=130、t0=0.344、period=0.461538、downbeatPhase=0 | beat k 约在 t0 + k × period；t0 不是 0 |
| `beats` | count=459、first=0.344、last=211.729、downbeats=115、period、downbeatPhase | 由 459 beat / 115 downbeat 推得平均约每 4 拍一个 downbeat；拍号未在素材中说明 |
| `arrangement` | 14 段：label、start、end、meanRms、maxRms、meanLow、meanMid、meanHigh、onsetDensity | 能量与频段数值是相对强度；标度、归一化方法、单位均未说明，不要当 dB 或绝对声压 |
| `perLineEnergy` | 129 条，按 `i` 与 `lines` 一一对应：rmsMean、rmsMax、lowMean、midMean、highMean | 已校验 i/start/end 与 `lines` 一致；它是行窗口统计，不含行内时间分布 |
| `energyCurve` | stepSeconds=0.5、columns=[time,rms,low,mid,high]、424 行、time 覆盖 0–211.5s | 相邻采样间隔 0.5s；最后约 0.4067s 无采样 |
| `silenceGaps` | `[[207.467, 211.907]]` | 素材中唯一被标为静音的区间 |
| `instrumentalWindows` | 4 段：20–29.709、135.224–147.66、195.356–205.811、209.811–211.907 | 位于 `lyrics_timing.json` |

## 口径与单位（未说明项）

所有能量、频段、`onsetDensity` 的物理标度与生成参数都未在素材中说明：可用于**相对比较、趋势响应与节奏密度判断**，不可当作 dB、绝对声压或测量值引用。

## 标注不是判据

`emphasis`（49 行）、`arrangement` 标签、`section` 编号都是**素材标注**，可参考也可忽略。它们不代表"什么重要"。与你的听感或结构判断冲突时，以你的判断为准，并在导演手记中说明。

## 已知怪点（使用前必读）

1. 最后一行（i=128）的 `end=209.811` 落在静音窗 207.467–211.907 内；它的 `perLineEnergy.rmsMean=0.134` 被静音拉低（`rmsMax=0.905`）。`end` 是素材的时间窗结束，不等于人声实际结束。
2. `perLineEnergy` 与 `lines` 逐项一致（i/start/end 相同，已校验）；但它是行窗口统计，不含行内时间分布。
3. `energyCurve` 以 0.5s 步长覆盖到 211.5s；最后约 0.4067s 没有采样。
4. 节拍网格 `t0=0.344` 不是 0；beat 0 在 0.344s。不要假设第一拍在 0。
5. `arrangement` 分段与歌词 `section` 边界接近但不相等（59.2 vs 59.223、147.7 vs 147.66 等）；不要把 label 直接当作歌词段落名。
6. `section` 编号 0–3 的含义未在素材中定义；只能当分组 id 使用。
7. `onsetDensity` 的单位、能量/频段的归一化方式、`arrangement` 的生成方法均未说明；只能作相对比较。
8. 现有素材中的歌词文本为 ASCII（含撇号 '）；外部提供的歌词以实际文本为准，转义可以，但不得改字符。
9. `silenceGaps` 只有一条；不要假设所有低能量段都是数字静音。
10. 素材未提供音频文件（音频由使用者另行提供并在成品内联）；素材也未提供调性、和弦、人声/乐器分离、官方 MV、创作背景、乐评；按 [delivery/FORMAT.md](../delivery/FORMAT.md) 的边界交代处理。
11. `silenceGaps` 起点 207.467s 与 `arrangement` 里 silence 段的起点 207.5s 相差 0.033s；两者不是同一口径，不要当成同一个边界。
12. 首行歌词 `start=0.100s` 早于首拍 0.344s；按 beat 公式推 k 会小于 0。使用节拍网格时需处理开头 0.244s 的"网格之前"区域。

## 与音频的关系

本仓库不含音频字节；音频与歌词由使用者放在**仓库根目录**（任意文件名/常见格式，协议见 [input/README.md](../input/README.md)）。数据文件本身不是音频，也不能替代音频。
