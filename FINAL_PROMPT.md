# HTML MV 制作任务书：world.execute(me);

> 给执行模型：这是一份完整的单文件 MV 创作任务（制作规范与节奏/音频分析数据齐备）。**歌词原文与音频字节不在本文中**，会在生成时由使用者单独提供（见 §4）：音频是必需输入，最终必须内联进 HTML；歌词文本可选，提供则逐字显示，不提供则只做节奏视觉。不要联网、不要索取额外文件、不要询问用户。请先完成导演构思与方案比较，再交付完整可运行的 standalone HTML，最后附自检与“信息不足”清单。

- 版本：v2.0（无歌词原文 + 外部音频内联）
- 生成日期：2026-11-14
- 本提示词包含：129 行歌词节奏/时序、音频分析数据（节拍/能量/分段/静音）、制作规范。
- 本提示词不包含：歌词原文（按使用者要求省略）、音频字节。
- 生成时外部输入：音频（必需，最终必须内联）；歌词原文（可选）。

## 0. 任务

把 Mili 的《world.execute(me);》做成一个 HTML MV。

- **产物**：一个 standalone 单文件 HTML：CSS/JS/歌词/音频全部内联，可离线双击运行；不是片段、不是伪代码、不是设计稿，也不是“HTML + 音频文件”两个文件。
- **输入**：音频和歌词文本由使用者在生成时单独提供；本提示词只提供歌词节奏与音频分析。
- **硬约束**：若提供歌词则逐字锁定；行级时间戳是歌词出现的唯一时钟；视觉全部由代码实时生成；不引用任何外部资源；信息不足必须写明“信息不足”，不得编造。
- **流程**：先提出互相冲突的视觉方案并比较，再写下视觉主张、选定方案并执行；然后交付完整 HTML；最后逐项自检。
- **授权**：除 §5、§6 的硬约束外，所有美学与工程决策由你做出，不要询问用户，不要等待确认。

## 1. 歌曲与时间轴概览

| 项 | 值 |
|---|---|
| 曲名 | world.execute(me); |
| 艺人 | Mili |
| 专辑 | Miracle Milk |
| 年份 | 2016 |
| 时长 | 211.9067 秒（3:31.907） |
| BPM | 130（素材独立校验值 129.99，置信度 high；另有一个被否决的 tracker 估计值 130.835，不要使用） |
| 歌词节奏行数 | 129 行（meta.lineCount = 129，已核对；原文不在本提示词中） |
| 歌词 section 分布 | section 0：12 行；section 1：74 行；section 2：42 行；section 3：1 行 |
| 音频分析分段 | arrangement 14 段（段名见下） |
| 节拍 | 459 个 beat、115 个 downbeat；首拍 0.344s、末拍 211.729s；周期 0.461538s（= 60/130） |

arrangement 的 14 个段名（素材原文，依次为）：intro、groove、verse 1、pre-chorus、verse 2、breakdown 1、build、chorus 2、breakdown 2、rhythmic chant、count-in、final chorus、outro、silence。具体起止与能量统计见 §3。

关键时间事实（均来自素材，可逐项核对）：

- 第一行 start = 0.100s；最后一行（i=128）start = 205.811s、end = 209.811s。
- 三段时间较长的无歌词器乐窗：20.000–29.709 / 135.224–147.660 / 195.356–205.811（秒）。
- 尾部器乐窗 209.811–211.907 落在静音段 207.467–211.907 之内（silenceGaps 唯一一条）。
- 最后一行时间窗结束（209.811）之后仍有约 2.1 秒；静音检测显示 207.467 起已静音，实际人声可能更早结束、留白更长。这段静音属于作品时间轴，不是可以截掉的空白。
- 歌词 section 边界与 arrangement 分段边界接近但不相等（例如 59.223 对 59.2、147.66 对 147.7）；两套数据是分别给出的，边界不必相等，不要混为同一事实。
- 生成时外部输入：音频（必需，最终内联进 HTML）；歌词原文（可选，提供则逐字显示，不提供则只做节奏视觉）。

## 2. 数据 A：歌词节奏与行级时间戳（129 行，不含歌词原文）

### 2.1 使用规则

1. 以下 129 行按 i 升序排列。不得重排、合并、拆分、增删行。
2. **本表没有 text 字段**：歌词原文由使用者另行提供。
   - 若提供：必须按 i 与节奏表一一对应，逐字使用；不得翻译、润色、补写、调序；不得改用模型记忆中的歌词、网络检索结果、官方 MV 字幕或其他版本。
   - 若未提供：成品中不得出现任何歌词原文（包括画面、隐藏 DOM、注释、aria-label、HTML title、控制台日志等），也不得用相近文本替代；只做节奏/能量/结构视觉，并在交付说明中写明“歌词原文未提供”。**例外：曲名 world.execute(me); 作为作品标识，允许出现在 HTML title、片头、元数据与 §2/§3 数据的 meta.title 中；不得据此扩展显示/编造其他歌词。**
3. start/end 单位为秒，是作品时间轴上的绝对位置。start 是该行出现的唯一时钟；end 是该行时间窗的结束。不得整体平移、缩放、四舍五入到 0.1s 或重算。
4. emphasis 是素材标注的强调行（共 49 行），只作为视觉强调的参考信号；如何呈现由你决定。
5. section 是素材给出的行分组编号（0–3），其语义（是否对应曲式、角色、段落名）未在素材中说明；不得自行宣称它是什么曲式段落。
6. instrumentalWindows 内不得出现任何歌词行；它们属于纯视觉段落。
7. 行时长可由 end − start 得出；行内节奏的推断规则见 §6。

以下 JSON 是歌词节奏数据（lines 一项一行）：


```json
{
  "meta": {
    "title": "world.execute(me);",
    "artist": "Mili",
    "album": "Miracle Milk",
    "year": 2016,
    "bpm": 130,
    "durationSeconds": 211.9067,
    "lineCount": 129,
    "content": "line-level rhythmic timing only (i/start/end/section/emphasis); lyric text intentionally omitted",
    "source": "derived from the supplied lyric timing data; text fields omitted"
  },
  "lines": [
    {"i":0,"start":0.1,"end":1.74,"section":0,"emphasis":false},
    {"i":1,"start":1.74,"end":2.92,"section":0,"emphasis":false},
    {"i":2,"start":2.92,"end":3.873,"section":0,"emphasis":true},
    {"i":3,"start":3.873,"end":5.491,"section":0,"emphasis":false},
    {"i":4,"start":5.491,"end":6.38,"section":0,"emphasis":false},
    {"i":5,"start":6.38,"end":7.446,"section":0,"emphasis":true},
    {"i":6,"start":7.446,"end":10.091,"section":0,"emphasis":false},
    {"i":7,"start":10.091,"end":11.095,"section":0,"emphasis":true},
    {"i":8,"start":11.095,"end":12.906,"section":0,"emphasis":false},
    {"i":9,"start":12.906,"end":13.891,"section":0,"emphasis":false},
    {"i":10,"start":13.891,"end":16,"section":0,"emphasis":true},
    {"i":11,"start":16,"end":20,"section":0,"emphasis":false},
    {"i":12,"start":29.709,"end":31.116,"section":1,"emphasis":false},
    {"i":13,"start":31.116,"end":32.682,"section":1,"emphasis":false},
    {"i":14,"start":32.682,"end":33.412,"section":1,"emphasis":true},
    {"i":15,"start":33.412,"end":34.646,"section":1,"emphasis":false},
    {"i":16,"start":34.646,"end":36.287,"section":1,"emphasis":false},
    {"i":17,"start":36.287,"end":37.067,"section":1,"emphasis":true},
    {"i":18,"start":37.067,"end":38.596,"section":1,"emphasis":false},
    {"i":19,"start":38.596,"end":40.049,"section":1,"emphasis":false},
    {"i":20,"start":40.049,"end":40.706,"section":1,"emphasis":true},
    {"i":21,"start":40.706,"end":42.346,"section":1,"emphasis":false},
    {"i":22,"start":42.346,"end":43.507,"section":1,"emphasis":false},
    {"i":23,"start":43.507,"end":44.452,"section":1,"emphasis":true},
    {"i":24,"start":44.452,"end":45.85,"section":1,"emphasis":false},
    {"i":25,"start":45.85,"end":47.672,"section":1,"emphasis":false},
    {"i":26,"start":47.672,"end":49.534,"section":1,"emphasis":false},
    {"i":27,"start":49.534,"end":51.363,"section":1,"emphasis":false},
    {"i":28,"start":51.363,"end":53.225,"section":1,"emphasis":false},
    {"i":29,"start":53.225,"end":55.083,"section":1,"emphasis":false},
    {"i":30,"start":55.083,"end":56.916,"section":1,"emphasis":false},
    {"i":31,"start":56.916,"end":59.223,"section":1,"emphasis":false},
    {"i":32,"start":59.223,"end":59.687,"section":1,"emphasis":false},
    {"i":33,"start":59.687,"end":61.958,"section":1,"emphasis":false},
    {"i":34,"start":61.958,"end":62.589,"section":1,"emphasis":true},
    {"i":35,"start":62.589,"end":63.535,"section":1,"emphasis":false},
    {"i":36,"start":63.535,"end":65.397,"section":1,"emphasis":false},
    {"i":37,"start":65.397,"end":66.601,"section":1,"emphasis":true},
    {"i":38,"start":66.601,"end":68.252,"section":1,"emphasis":false},
    {"i":39,"start":68.252,"end":69.259,"section":1,"emphasis":false},
    {"i":40,"start":69.259,"end":70.084,"section":1,"emphasis":true},
    {"i":41,"start":70.084,"end":71.764,"section":1,"emphasis":false},
    {"i":42,"start":71.764,"end":73.169,"section":1,"emphasis":false},
    {"i":43,"start":73.169,"end":74.045,"section":1,"emphasis":true},
    {"i":44,"start":74.045,"end":75.422,"section":1,"emphasis":false},
    {"i":45,"start":75.422,"end":76.959,"section":1,"emphasis":false},
    {"i":46,"start":76.959,"end":77.576,"section":1,"emphasis":true},
    {"i":47,"start":77.576,"end":79.226,"section":1,"emphasis":false},
    {"i":48,"start":79.226,"end":80.62,"section":1,"emphasis":false},
    {"i":49,"start":80.62,"end":81.351,"section":1,"emphasis":true},
    {"i":50,"start":81.351,"end":82.833,"section":1,"emphasis":false},
    {"i":51,"start":82.833,"end":84.268,"section":1,"emphasis":false},
    {"i":52,"start":84.268,"end":85.078,"section":1,"emphasis":true},
    {"i":53,"start":85.078,"end":86.538,"section":1,"emphasis":false},
    {"i":54,"start":86.538,"end":87.922,"section":1,"emphasis":false},
    {"i":55,"start":87.922,"end":88.587,"section":1,"emphasis":true},
    {"i":56,"start":88.587,"end":90.197,"section":1,"emphasis":false},
    {"i":57,"start":90.197,"end":92.015,"section":1,"emphasis":false},
    {"i":58,"start":92.015,"end":93.953,"section":1,"emphasis":false},
    {"i":59,"start":93.953,"end":95.465,"section":1,"emphasis":false},
    {"i":60,"start":95.465,"end":97.739,"section":1,"emphasis":false},
    {"i":61,"start":97.739,"end":99.349,"section":1,"emphasis":false},
    {"i":62,"start":99.349,"end":101.474,"section":1,"emphasis":false},
    {"i":63,"start":101.474,"end":103.489,"section":1,"emphasis":false},
    {"i":64,"start":103.489,"end":104.197,"section":1,"emphasis":false},
    {"i":65,"start":104.197,"end":106.293,"section":1,"emphasis":false},
    {"i":66,"start":106.293,"end":107.22,"section":1,"emphasis":true},
    {"i":67,"start":107.22,"end":107.903,"section":1,"emphasis":false},
    {"i":68,"start":107.903,"end":110.221,"section":1,"emphasis":false},
    {"i":69,"start":110.221,"end":110.9,"section":1,"emphasis":true},
    {"i":70,"start":110.9,"end":112.22,"section":1,"emphasis":false},
    {"i":71,"start":112.22,"end":113.1,"section":1,"emphasis":false},
    {"i":72,"start":113.1,"end":114.18,"section":1,"emphasis":false},
    {"i":73,"start":114.18,"end":114.92,"section":1,"emphasis":false},
    {"i":74,"start":114.92,"end":115.78,"section":1,"emphasis":false},
    {"i":75,"start":115.78,"end":117.274,"section":1,"emphasis":false},
    {"i":76,"start":117.274,"end":118.333,"section":1,"emphasis":true},
    {"i":77,"start":118.333,"end":118.979,"section":1,"emphasis":false},
    {"i":78,"start":118.979,"end":120.86,"section":1,"emphasis":false},
    {"i":79,"start":120.86,"end":121.728,"section":1,"emphasis":true},
    {"i":80,"start":121.728,"end":122.714,"section":1,"emphasis":false},
    {"i":81,"start":122.714,"end":124.89,"section":1,"emphasis":false},
    {"i":82,"start":124.89,"end":125.708,"section":1,"emphasis":true},
    {"i":83,"start":125.708,"end":128.661,"section":1,"emphasis":false},
    {"i":84,"start":128.661,"end":131.224,"section":1,"emphasis":false},
    {"i":85,"start":131.224,"end":135.224,"section":1,"emphasis":true},
    {"i":86,"start":147.66,"end":148.6,"section":2,"emphasis":true},
    {"i":87,"start":148.6,"end":149.52,"section":2,"emphasis":true},
    {"i":88,"start":149.52,"end":150.54,"section":2,"emphasis":true},
    {"i":89,"start":150.54,"end":151.52,"section":2,"emphasis":true},
    {"i":90,"start":151.52,"end":152.28,"section":2,"emphasis":true},
    {"i":91,"start":152.28,"end":153.16,"section":2,"emphasis":true},
    {"i":92,"start":153.16,"end":153.98,"section":2,"emphasis":true},
    {"i":93,"start":153.98,"end":155.2,"section":2,"emphasis":true},
    {"i":94,"start":155.2,"end":156.08,"section":2,"emphasis":true},
    {"i":95,"start":156.08,"end":157.04,"section":2,"emphasis":true},
    {"i":96,"start":157.04,"end":158,"section":2,"emphasis":true},
    {"i":97,"start":158,"end":158.9,"section":2,"emphasis":true},
    {"i":98,"start":158.9,"end":159.321,"section":2,"emphasis":true},
    {"i":99,"start":159.321,"end":159.657,"section":2,"emphasis":true},
    {"i":100,"start":159.657,"end":160.244,"section":2,"emphasis":true},
    {"i":101,"start":160.244,"end":160.693,"section":2,"emphasis":true},
    {"i":102,"start":160.693,"end":161.124,"section":2,"emphasis":true},
    {"i":103,"start":161.124,"end":161.584,"section":2,"emphasis":true},
    {"i":104,"start":161.584,"end":162.632,"section":2,"emphasis":true},
    {"i":105,"start":162.632,"end":163.315,"section":2,"emphasis":false},
    {"i":106,"start":163.315,"end":165.166,"section":2,"emphasis":false},
    {"i":107,"start":165.166,"end":166.016,"section":2,"emphasis":true},
    {"i":108,"start":166.016,"end":167.022,"section":2,"emphasis":false},
    {"i":109,"start":167.022,"end":168.911,"section":2,"emphasis":false},
    {"i":110,"start":168.911,"end":169.824,"section":2,"emphasis":true},
    {"i":111,"start":169.824,"end":171.868,"section":2,"emphasis":false},
    {"i":112,"start":171.868,"end":172.712,"section":2,"emphasis":false},
    {"i":113,"start":172.712,"end":173.643,"section":2,"emphasis":true},
    {"i":114,"start":173.643,"end":174.975,"section":2,"emphasis":false},
    {"i":115,"start":174.975,"end":177.246,"section":2,"emphasis":false},
    {"i":116,"start":177.246,"end":178.173,"section":2,"emphasis":false},
    {"i":117,"start":178.173,"end":179.929,"section":2,"emphasis":false},
    {"i":118,"start":179.929,"end":180.857,"section":2,"emphasis":true},
    {"i":119,"start":180.857,"end":181.901,"section":2,"emphasis":false},
    {"i":120,"start":181.901,"end":183.646,"section":2,"emphasis":false},
    {"i":121,"start":183.646,"end":184.54,"section":2,"emphasis":true},
    {"i":122,"start":184.54,"end":187.665,"section":2,"emphasis":false},
    {"i":123,"start":187.665,"end":188.483,"section":2,"emphasis":true},
    {"i":124,"start":188.483,"end":189.746,"section":2,"emphasis":false},
    {"i":125,"start":189.746,"end":190.801,"section":2,"emphasis":false},
    {"i":126,"start":190.801,"end":191.356,"section":2,"emphasis":false},
    {"i":127,"start":191.356,"end":195.356,"section":2,"emphasis":true},
    {"i":128,"start":205.811,"end":209.811,"section":3,"emphasis":true}
  ],
  "instrumentalWindows": [
    {"start":20,"end":29.709},
    {"start":135.224,"end":147.66},
    {"start":195.356,"end":205.811},
    {"start":209.811,"end":211.907}
  ]
}
```


## 3. 数据 B：音频分析（不含音频字节）

### 3.1 状态声明（重要）

本提示词不含音频字节；生成时音频由使用者单独提供（附件、本地文件或 base64 均可），最终必须内联进 HTML（见 §4.3）。本节是随素材提供的音频分析结果，用于推断行内节奏、能量与结构；它不是音频文件本身。

因此：

- 不得联网获取歌曲、音频、封面、歌词页、乐评、官方 MV 或任何参考资料；
- 不得用其他曲目、合成音频或伪造波形替代使用者提供的原曲；
- 统一口径：**音频的正式用途是播放、时间轴对齐与完整性校验**；“行内节奏”的数值依据以 §2 节奏数据与 §3 分析数据为准（若提供歌词，可用文本长度作辅助）；如果执行环境确实做了额外的音频分析并使用，必须说明方法、工具与局限，不得与给定数据矛盾，也不得声称得到未提供的逐字、音高或频谱细节；没有实际分析时，不得声称分析过音频；
- 本节数据支撑不了的判断，按 §4 处理。

### 3.2 字段字典

| 字段 / 数据块 | 含义 | 使用注意 |
|---|---|---|
| meta | 曲名、时长、BPM 与节拍网格 | bpmIndependent 为独立校验值；被否决的 130.835 不要使用 |
| meta.grid | bpm=130、t0=0.344、period=0.461538、downbeatPhase=0 | beat k 的时刻约为 t0 + k × period；t0 不是 0 |
| beats | count=459、first=0.344、last=211.729、downbeats=115、period、downbeatPhase | 由 459 beat / 115 downbeat 推得平均约每 4 拍一个 downbeat；拍号未在素材中说明 |
| arrangement | 14 段：label、start、end、meanRms、maxRms、meanLow、meanMid、meanHigh、onsetDensity | 能量与频段数值是相对强度；标度、归一化方法、单位均未说明，不要当 dB 或绝对声压 |
| perLineEnergy | 129 条，按 i 与 §2 节奏行一一对应：rmsMean、rmsMax、lowMean、midMean、highMean | 已校验 i/start/end 与 §2 一致；它是行窗口统计，不含行内时间分布 |
| energyCurve | stepSeconds=0.5、columns=[time,rms,low,mid,high]、424 行、time 覆盖 0–211.5s | 相邻采样间隔 0.5s；最后约 0.4067s 无采样 |
| silenceGaps | [[207.467, 211.907]] | 素材中唯一被标为静音的区间 |
| instrumentalWindows | 4 段无歌词窗（在 §2 数据中） | 4 段分别是 20–29.709、135.224–147.66、195.356–205.811、209.811–211.907 |

注意：所有能量、频段、onsetDensity 的物理标度与生成参数都未在素材中说明。它们可用于相对比较、趋势响应与节奏密度判断，不可当作绝对测量值引用。

### 3.3 音频分析 JSON（完整）


```json
{
  "meta": {
    "title": "world.execute(me);",
    "artist": "Mili",
    "durationSeconds": 211.9067,
    "bpm": 130,
    "bpmIndependent": 129.99,
    "bpmConfidence": "high (+/-0.02); a tracker estimate of 130.835 was rejected",
    "grid": {
      "bpm": 130,
      "t0": 0.344,
      "period": 0.461538,
      "downbeatPhase": 0
    },
    "source": "song materials supplied with the question; tempo independently checked"
  },
  "beats": {"count":459,"first":0.344,"last":211.729,"downbeats":115,"period":0.461538,"downbeatPhase":0},
  "arrangement": [
    {"label":"intro","start":0,"end":14.95,"meanRms":0.354,"maxRms":1,"meanLow":0.241,"meanMid":0.285,"meanHigh":0.143,"onsetDensity":6.62},
    {"label":"groove","start":14.95,"end":29.7,"meanRms":0.528,"maxRms":1,"meanLow":0.344,"meanMid":0.511,"meanHigh":0.479,"onsetDensity":5.29},
    {"label":"verse 1","start":29.7,"end":59.2,"meanRms":0.486,"maxRms":1,"meanLow":0.322,"meanMid":0.466,"meanHigh":0.242,"onsetDensity":5.66},
    {"label":"pre-chorus","start":59.2,"end":74,"meanRms":0.607,"maxRms":1,"meanLow":0.341,"meanMid":0.718,"meanHigh":0.642,"onsetDensity":3.72},
    {"label":"verse 2","start":74,"end":96,"meanRms":0.485,"maxRms":1,"meanLow":0.275,"meanMid":0.561,"meanHigh":0.382,"onsetDensity":4.32},
    {"label":"breakdown 1","start":96,"end":103.5,"meanRms":0.435,"maxRms":1,"meanLow":0.122,"meanMid":0.674,"meanHigh":0.294,"onsetDensity":3.73},
    {"label":"build","start":103.5,"end":118.3,"meanRms":0.539,"maxRms":1,"meanLow":0.305,"meanMid":0.643,"meanHigh":0.404,"onsetDensity":5.54},
    {"label":"chorus 2","start":118.3,"end":134,"meanRms":0.555,"maxRms":1,"meanLow":0.312,"meanMid":0.668,"meanHigh":0.424,"onsetDensity":5.35},
    {"label":"breakdown 2","start":134,"end":147.7,"meanRms":0.494,"maxRms":1,"meanLow":0.32,"meanMid":0.505,"meanHigh":0.246,"onsetDensity":7.3},
    {"label":"rhythmic chant","start":147.7,"end":158.9,"meanRms":0.506,"maxRms":1,"meanLow":0.31,"meanMid":0.548,"meanHigh":0.346,"onsetDensity":6.96},
    {"label":"count-in","start":158.9,"end":162.6,"meanRms":0.442,"maxRms":1,"meanLow":0.254,"meanMid":0.512,"meanHigh":0.262,"onsetDensity":4.05},
    {"label":"final chorus","start":162.6,"end":191.4,"meanRms":0.602,"maxRms":1,"meanLow":0.332,"meanMid":0.724,"meanHigh":0.641,"onsetDensity":2.88},
    {"label":"outro","start":191.4,"end":207.5,"meanRms":0.401,"maxRms":0.974,"meanLow":0.273,"meanMid":0.319,"meanHigh":0.294,"onsetDensity":5.96},
    {"label":"silence","start":207.5,"end":211.907,"meanRms":0,"maxRms":0.009,"meanLow":0,"meanMid":0.001,"meanHigh":0.001,"onsetDensity":0}
  ],
  "perLineEnergy": [
    {"i":0,"start":0.1,"end":1.74,"rmsMean":0.331,"rmsMax":0.981,"lowMean":0.228,"midMean":0.24,"highMean":0.145},
    {"i":1,"start":1.74,"end":2.92,"rmsMean":0.37,"rmsMax":0.965,"lowMean":0.264,"midMean":0.288,"highMean":0.106},
    {"i":2,"start":2.92,"end":3.873,"rmsMean":0.352,"rmsMax":0.952,"lowMean":0.23,"midMean":0.308,"highMean":0.133},
    {"i":3,"start":3.873,"end":5.491,"rmsMean":0.375,"rmsMax":0.981,"lowMean":0.255,"midMean":0.295,"highMean":0.154},
    {"i":4,"start":5.491,"end":6.38,"rmsMean":0.366,"rmsMax":1,"lowMean":0.264,"midMean":0.275,"highMean":0.17},
    {"i":5,"start":6.38,"end":7.446,"rmsMean":0.346,"rmsMax":1,"lowMean":0.237,"midMean":0.262,"highMean":0.135},
    {"i":6,"start":7.446,"end":10.091,"rmsMean":0.376,"rmsMax":1,"lowMean":0.261,"midMean":0.296,"highMean":0.134},
    {"i":7,"start":10.091,"end":11.095,"rmsMean":0.355,"rmsMax":0.98,"lowMean":0.228,"midMean":0.315,"highMean":0.185},
    {"i":8,"start":11.095,"end":12.906,"rmsMean":0.375,"rmsMax":0.979,"lowMean":0.258,"midMean":0.288,"highMean":0.144},
    {"i":9,"start":12.906,"end":13.891,"rmsMean":0.362,"rmsMax":0.992,"lowMean":0.253,"midMean":0.303,"highMean":0.146},
    {"i":10,"start":13.891,"end":16,"rmsMean":0.374,"rmsMax":1,"lowMean":0.225,"midMean":0.4,"highMean":0.306},
    {"i":11,"start":16,"end":20,"rmsMean":0.524,"rmsMax":1,"lowMean":0.344,"midMean":0.5,"highMean":0.437},
    {"i":12,"start":29.709,"end":31.116,"rmsMean":0.407,"rmsMax":0.915,"lowMean":0.209,"midMean":0.492,"highMean":0.348},
    {"i":13,"start":31.116,"end":32.682,"rmsMean":0.388,"rmsMax":0.92,"lowMean":0.213,"midMean":0.443,"highMean":0.16},
    {"i":14,"start":32.682,"end":33.412,"rmsMean":0.387,"rmsMax":0.92,"lowMean":0.234,"midMean":0.43,"highMean":0.219},
    {"i":15,"start":33.412,"end":34.646,"rmsMean":0.391,"rmsMax":0.924,"lowMean":0.235,"midMean":0.411,"highMean":0.199},
    {"i":16,"start":34.646,"end":36.287,"rmsMean":0.396,"rmsMax":0.918,"lowMean":0.236,"midMean":0.451,"highMean":0.137},
    {"i":17,"start":36.287,"end":37.067,"rmsMean":0.497,"rmsMax":0.91,"lowMean":0.34,"midMean":0.455,"highMean":0.27},
    {"i":18,"start":37.067,"end":38.596,"rmsMean":0.536,"rmsMax":0.926,"lowMean":0.378,"midMean":0.497,"highMean":0.216},
    {"i":19,"start":38.596,"end":40.049,"rmsMean":0.523,"rmsMax":0.978,"lowMean":0.367,"midMean":0.471,"highMean":0.186},
    {"i":20,"start":40.049,"end":40.706,"rmsMean":0.546,"rmsMax":1,"lowMean":0.382,"midMean":0.489,"highMean":0.203},
    {"i":21,"start":40.706,"end":42.346,"rmsMean":0.526,"rmsMax":1,"lowMean":0.386,"midMean":0.405,"highMean":0.222},
    {"i":22,"start":42.346,"end":43.507,"rmsMean":0.567,"rmsMax":0.975,"lowMean":0.418,"midMean":0.452,"highMean":0.166},
    {"i":23,"start":43.507,"end":44.452,"rmsMean":0.528,"rmsMax":0.981,"lowMean":0.365,"midMean":0.475,"highMean":0.319},
    {"i":24,"start":44.452,"end":45.85,"rmsMean":0.494,"rmsMax":0.96,"lowMean":0.321,"midMean":0.487,"highMean":0.369},
    {"i":25,"start":45.85,"end":47.672,"rmsMean":0.492,"rmsMax":0.947,"lowMean":0.337,"midMean":0.44,"highMean":0.295},
    {"i":26,"start":47.672,"end":49.534,"rmsMean":0.515,"rmsMax":0.942,"lowMean":0.341,"midMean":0.519,"highMean":0.251},
    {"i":27,"start":49.534,"end":51.363,"rmsMean":0.476,"rmsMax":0.937,"lowMean":0.327,"midMean":0.426,"highMean":0.279},
    {"i":28,"start":51.363,"end":53.225,"rmsMean":0.5,"rmsMax":0.981,"lowMean":0.321,"midMean":0.53,"highMean":0.262},
    {"i":29,"start":53.225,"end":55.083,"rmsMean":0.537,"rmsMax":0.978,"lowMean":0.379,"midMean":0.468,"highMean":0.24},
    {"i":30,"start":55.083,"end":56.916,"rmsMean":0.496,"rmsMax":0.972,"lowMean":0.338,"midMean":0.464,"highMean":0.253},
    {"i":31,"start":56.916,"end":59.223,"rmsMean":0.493,"rmsMax":0.994,"lowMean":0.311,"midMean":0.48,"highMean":0.235},
    {"i":32,"start":59.223,"end":59.687,"rmsMean":0.443,"rmsMax":1,"lowMean":0.221,"midMean":0.548,"highMean":0.468},
    {"i":33,"start":59.687,"end":61.958,"rmsMean":0.605,"rmsMax":1,"lowMean":0.34,"midMean":0.723,"highMean":0.633},
    {"i":34,"start":61.958,"end":62.589,"rmsMean":0.562,"rmsMax":1,"lowMean":0.294,"midMean":0.713,"highMean":0.613},
    {"i":35,"start":62.589,"end":63.535,"rmsMean":0.612,"rmsMax":0.976,"lowMean":0.344,"midMean":0.716,"highMean":0.659},
    {"i":36,"start":63.535,"end":65.397,"rmsMean":0.634,"rmsMax":1,"lowMean":0.344,"midMean":0.78,"highMean":0.615},
    {"i":37,"start":65.397,"end":66.601,"rmsMean":0.604,"rmsMax":1,"lowMean":0.338,"midMean":0.708,"highMean":0.664},
    {"i":38,"start":66.601,"end":68.252,"rmsMean":0.601,"rmsMax":0.964,"lowMean":0.341,"midMean":0.712,"highMean":0.639},
    {"i":39,"start":68.252,"end":69.259,"rmsMean":0.607,"rmsMax":0.984,"lowMean":0.326,"midMean":0.769,"highMean":0.587},
    {"i":40,"start":69.259,"end":70.084,"rmsMean":0.598,"rmsMax":0.969,"lowMean":0.34,"midMean":0.708,"highMean":0.639},
    {"i":41,"start":70.084,"end":71.764,"rmsMean":0.636,"rmsMax":1,"lowMean":0.377,"midMean":0.736,"highMean":0.617},
    {"i":42,"start":71.764,"end":73.169,"rmsMean":0.598,"rmsMax":0.936,"lowMean":0.351,"midMean":0.659,"highMean":0.744},
    {"i":43,"start":73.169,"end":74.045,"rmsMean":0.648,"rmsMax":0.955,"lowMean":0.377,"midMean":0.731,"highMean":0.762},
    {"i":44,"start":74.045,"end":75.422,"rmsMean":0.389,"rmsMax":0.71,"lowMean":0.094,"midMean":0.613,"highMean":0.541},
    {"i":45,"start":75.422,"end":76.959,"rmsMean":0.415,"rmsMax":0.963,"lowMean":0.179,"midMean":0.532,"highMean":0.485},
    {"i":46,"start":76.959,"end":77.576,"rmsMean":0.481,"rmsMax":0.633,"lowMean":0.248,"midMean":0.618,"highMean":0.428},
    {"i":47,"start":77.576,"end":79.226,"rmsMean":0.529,"rmsMax":1,"lowMean":0.318,"midMean":0.571,"highMean":0.566},
    {"i":48,"start":79.226,"end":80.62,"rmsMean":0.509,"rmsMax":0.993,"lowMean":0.312,"midMean":0.548,"highMean":0.464},
    {"i":49,"start":80.62,"end":81.351,"rmsMean":0.513,"rmsMax":0.921,"lowMean":0.354,"midMean":0.478,"highMean":0.289},
    {"i":50,"start":81.351,"end":82.833,"rmsMean":0.492,"rmsMax":0.918,"lowMean":0.318,"midMean":0.534,"highMean":0.301},
    {"i":51,"start":82.833,"end":84.268,"rmsMean":0.456,"rmsMax":1,"lowMean":0.274,"midMean":0.525,"highMean":0.248},
    {"i":52,"start":84.268,"end":85.078,"rmsMean":0.42,"rmsMax":0.511,"lowMean":0.223,"midMean":0.574,"highMean":0.25},
    {"i":53,"start":85.078,"end":86.538,"rmsMean":0.526,"rmsMax":0.895,"lowMean":0.366,"midMean":0.504,"highMean":0.24},
    {"i":54,"start":86.538,"end":87.922,"rmsMean":0.581,"rmsMax":1,"lowMean":0.427,"midMean":0.471,"highMean":0.281},
    {"i":55,"start":87.922,"end":88.587,"rmsMean":0.467,"rmsMax":0.616,"lowMean":0.237,"midMean":0.542,"highMean":0.352},
    {"i":56,"start":88.587,"end":90.197,"rmsMean":0.405,"rmsMax":0.518,"lowMean":0.102,"midMean":0.661,"highMean":0.462},
    {"i":57,"start":90.197,"end":92.015,"rmsMean":0.441,"rmsMax":1,"lowMean":0.217,"midMean":0.585,"highMean":0.389},
    {"i":58,"start":92.015,"end":93.953,"rmsMean":0.518,"rmsMax":0.793,"lowMean":0.323,"midMean":0.596,"highMean":0.301},
    {"i":59,"start":93.953,"end":95.465,"rmsMean":0.566,"rmsMax":1,"lowMean":0.394,"midMean":0.52,"highMean":0.398},
    {"i":60,"start":95.465,"end":97.739,"rmsMean":0.496,"rmsMax":1,"lowMean":0.19,"midMean":0.721,"highMean":0.299},
    {"i":61,"start":97.739,"end":99.349,"rmsMean":0.443,"rmsMax":0.996,"lowMean":0.13,"midMean":0.698,"highMean":0.332},
    {"i":62,"start":99.349,"end":101.474,"rmsMean":0.446,"rmsMax":1,"lowMean":0.129,"midMean":0.673,"highMean":0.318},
    {"i":63,"start":101.474,"end":103.489,"rmsMean":0.38,"rmsMax":0.584,"lowMean":0.069,"midMean":0.608,"highMean":0.264},
    {"i":64,"start":103.489,"end":104.197,"rmsMean":0.519,"rmsMax":1,"lowMean":0.334,"midMean":0.546,"highMean":0.312},
    {"i":65,"start":104.197,"end":106.293,"rmsMean":0.556,"rmsMax":1,"lowMean":0.341,"midMean":0.623,"highMean":0.342},
    {"i":66,"start":106.293,"end":107.22,"rmsMean":0.528,"rmsMax":0.971,"lowMean":0.295,"midMean":0.646,"highMean":0.378},
    {"i":67,"start":107.22,"end":107.903,"rmsMean":0.592,"rmsMax":1,"lowMean":0.342,"midMean":0.728,"highMean":0.332},
    {"i":68,"start":107.903,"end":110.221,"rmsMean":0.568,"rmsMax":1,"lowMean":0.325,"midMean":0.693,"highMean":0.337},
    {"i":69,"start":110.221,"end":110.9,"rmsMean":0.523,"rmsMax":0.886,"lowMean":0.305,"midMean":0.611,"highMean":0.454},
    {"i":70,"start":110.9,"end":112.22,"rmsMean":0.542,"rmsMax":0.991,"lowMean":0.312,"midMean":0.636,"highMean":0.4},
    {"i":71,"start":112.22,"end":113.1,"rmsMean":0.515,"rmsMax":1,"lowMean":0.271,"midMean":0.634,"highMean":0.477},
    {"i":72,"start":113.1,"end":114.18,"rmsMean":0.499,"rmsMax":0.939,"lowMean":0.288,"midMean":0.579,"highMean":0.452},
    {"i":73,"start":114.18,"end":114.92,"rmsMean":0.513,"rmsMax":1,"lowMean":0.263,"midMean":0.634,"highMean":0.421},
    {"i":74,"start":114.92,"end":115.78,"rmsMean":0.484,"rmsMax":0.966,"lowMean":0.248,"midMean":0.613,"highMean":0.452},
    {"i":75,"start":115.78,"end":117.274,"rmsMean":0.504,"rmsMax":0.961,"lowMean":0.272,"midMean":0.615,"highMean":0.456},
    {"i":76,"start":117.274,"end":118.333,"rmsMean":0.609,"rmsMax":0.917,"lowMean":0.323,"midMean":0.758,"highMean":0.54},
    {"i":77,"start":118.333,"end":118.979,"rmsMean":0.522,"rmsMax":0.907,"lowMean":0.29,"midMean":0.639,"highMean":0.478},
    {"i":78,"start":118.979,"end":120.86,"rmsMean":0.55,"rmsMax":1,"lowMean":0.329,"midMean":0.626,"highMean":0.412},
    {"i":79,"start":120.86,"end":121.728,"rmsMean":0.537,"rmsMax":0.907,"lowMean":0.311,"midMean":0.641,"highMean":0.374},
    {"i":80,"start":121.728,"end":122.714,"rmsMean":0.579,"rmsMax":0.966,"lowMean":0.339,"midMean":0.685,"highMean":0.351},
    {"i":81,"start":122.714,"end":124.89,"rmsMean":0.568,"rmsMax":0.991,"lowMean":0.309,"midMean":0.726,"highMean":0.384},
    {"i":82,"start":124.89,"end":125.708,"rmsMean":0.566,"rmsMax":0.941,"lowMean":0.312,"midMean":0.679,"highMean":0.479},
    {"i":83,"start":125.708,"end":128.661,"rmsMean":0.572,"rmsMax":1,"lowMean":0.306,"midMean":0.721,"highMean":0.418},
    {"i":84,"start":128.661,"end":131.224,"rmsMean":0.538,"rmsMax":1,"lowMean":0.327,"midMean":0.582,"highMean":0.436},
    {"i":85,"start":131.224,"end":135.224,"rmsMean":0.533,"rmsMax":1,"lowMean":0.305,"midMean":0.625,"highMean":0.4},
    {"i":86,"start":147.66,"end":148.6,"rmsMean":0.519,"rmsMax":0.992,"lowMean":0.329,"midMean":0.544,"highMean":0.411},
    {"i":87,"start":148.6,"end":149.52,"rmsMean":0.504,"rmsMax":0.951,"lowMean":0.319,"midMean":0.53,"highMean":0.346},
    {"i":88,"start":149.52,"end":150.54,"rmsMean":0.514,"rmsMax":1,"lowMean":0.324,"midMean":0.542,"highMean":0.333},
    {"i":89,"start":150.54,"end":151.52,"rmsMean":0.52,"rmsMax":0.993,"lowMean":0.319,"midMean":0.579,"highMean":0.338},
    {"i":90,"start":151.52,"end":152.28,"rmsMean":0.511,"rmsMax":1,"lowMean":0.32,"midMean":0.544,"highMean":0.294},
    {"i":91,"start":152.28,"end":153.16,"rmsMean":0.517,"rmsMax":0.929,"lowMean":0.336,"midMean":0.525,"highMean":0.329},
    {"i":92,"start":153.16,"end":153.98,"rmsMean":0.486,"rmsMax":0.928,"lowMean":0.287,"midMean":0.562,"highMean":0.314},
    {"i":93,"start":153.98,"end":155.2,"rmsMean":0.512,"rmsMax":1,"lowMean":0.316,"midMean":0.56,"highMean":0.327},
    {"i":94,"start":155.2,"end":156.08,"rmsMean":0.485,"rmsMax":0.99,"lowMean":0.31,"midMean":0.488,"highMean":0.37},
    {"i":95,"start":156.08,"end":157.04,"rmsMean":0.487,"rmsMax":1,"lowMean":0.271,"midMean":0.574,"highMean":0.354},
    {"i":96,"start":157.04,"end":158,"rmsMean":0.507,"rmsMax":0.982,"lowMean":0.308,"midMean":0.549,"highMean":0.343},
    {"i":97,"start":158,"end":158.9,"rmsMean":0.498,"rmsMax":0.987,"lowMean":0.272,"midMean":0.577,"highMean":0.387},
    {"i":98,"start":158.9,"end":159.321,"rmsMean":0.474,"rmsMax":0.791,"lowMean":0.255,"midMean":0.595,"highMean":0.374},
    {"i":99,"start":159.321,"end":159.657,"rmsMean":0.399,"rmsMax":0.463,"lowMean":0.244,"midMean":0.459,"highMean":0.193},
    {"i":100,"start":159.657,"end":160.244,"rmsMean":0.487,"rmsMax":1,"lowMean":0.289,"midMean":0.522,"highMean":0.23},
    {"i":101,"start":160.244,"end":160.693,"rmsMean":0.434,"rmsMax":0.953,"lowMean":0.215,"midMean":0.541,"highMean":0.22},
    {"i":102,"start":160.693,"end":161.124,"rmsMean":0.375,"rmsMax":0.535,"lowMean":0.209,"midMean":0.472,"highMean":0.239},
    {"i":103,"start":161.124,"end":161.584,"rmsMean":0.497,"rmsMax":0.956,"lowMean":0.311,"midMean":0.543,"highMean":0.185},
    {"i":104,"start":161.584,"end":162.632,"rmsMean":0.422,"rmsMax":0.592,"lowMean":0.245,"midMean":0.481,"highMean":0.316},
    {"i":105,"start":162.632,"end":163.315,"rmsMean":0.572,"rmsMax":1,"lowMean":0.333,"midMean":0.633,"highMean":0.587},
    {"i":106,"start":163.315,"end":165.166,"rmsMean":0.593,"rmsMax":0.94,"lowMean":0.309,"midMean":0.745,"highMean":0.631},
    {"i":107,"start":165.166,"end":166.016,"rmsMean":0.574,"rmsMax":1,"lowMean":0.32,"midMean":0.701,"highMean":0.634},
    {"i":108,"start":166.016,"end":167.022,"rmsMean":0.652,"rmsMax":1,"lowMean":0.384,"midMean":0.743,"highMean":0.619},
    {"i":109,"start":167.022,"end":168.911,"rmsMean":0.612,"rmsMax":1,"lowMean":0.312,"midMean":0.781,"highMean":0.629},
    {"i":110,"start":168.911,"end":169.824,"rmsMean":0.623,"rmsMax":0.972,"lowMean":0.362,"midMean":0.727,"highMean":0.697},
    {"i":111,"start":169.824,"end":171.868,"rmsMean":0.578,"rmsMax":0.945,"lowMean":0.32,"midMean":0.699,"highMean":0.625},
    {"i":112,"start":171.868,"end":172.712,"rmsMean":0.602,"rmsMax":1,"lowMean":0.337,"midMean":0.738,"highMean":0.539},
    {"i":113,"start":172.712,"end":173.643,"rmsMean":0.63,"rmsMax":1,"lowMean":0.374,"midMean":0.683,"highMean":0.652},
    {"i":114,"start":173.643,"end":174.975,"rmsMean":0.632,"rmsMax":0.954,"lowMean":0.369,"midMean":0.746,"highMean":0.599},
    {"i":115,"start":174.975,"end":177.246,"rmsMean":0.62,"rmsMax":0.905,"lowMean":0.347,"midMean":0.744,"highMean":0.683},
    {"i":116,"start":177.246,"end":178.173,"rmsMean":0.588,"rmsMax":0.944,"lowMean":0.294,"midMean":0.731,"highMean":0.758},
    {"i":117,"start":178.173,"end":179.929,"rmsMean":0.583,"rmsMax":1,"lowMean":0.288,"midMean":0.73,"highMean":0.69},
    {"i":118,"start":179.929,"end":180.857,"rmsMean":0.628,"rmsMax":0.992,"lowMean":0.36,"midMean":0.752,"highMean":0.582},
    {"i":119,"start":180.857,"end":181.901,"rmsMean":0.565,"rmsMax":0.975,"lowMean":0.302,"midMean":0.691,"highMean":0.663},
    {"i":120,"start":181.901,"end":183.646,"rmsMean":0.615,"rmsMax":0.983,"lowMean":0.361,"midMean":0.691,"highMean":0.65},
    {"i":121,"start":183.646,"end":184.54,"rmsMean":0.593,"rmsMax":0.875,"lowMean":0.326,"midMean":0.717,"highMean":0.658},
    {"i":122,"start":184.54,"end":187.665,"rmsMean":0.591,"rmsMax":0.983,"lowMean":0.31,"midMean":0.736,"highMean":0.661},
    {"i":123,"start":187.665,"end":188.483,"rmsMean":0.576,"rmsMax":0.974,"lowMean":0.315,"midMean":0.694,"highMean":0.573},
    {"i":124,"start":188.483,"end":189.746,"rmsMean":0.646,"rmsMax":0.979,"lowMean":0.377,"midMean":0.758,"highMean":0.58},
    {"i":125,"start":189.746,"end":190.801,"rmsMean":0.602,"rmsMax":0.991,"lowMean":0.344,"midMean":0.716,"highMean":0.615},
    {"i":126,"start":190.801,"end":191.356,"rmsMean":0.541,"rmsMax":0.882,"lowMean":0.304,"midMean":0.627,"highMean":0.672},
    {"i":127,"start":191.356,"end":195.356,"rmsMean":0.444,"rmsMax":0.974,"lowMean":0.291,"midMean":0.383,"highMean":0.357},
    {"i":128,"start":205.811,"end":209.811,"rmsMean":0.134,"rmsMax":0.905,"lowMean":0.072,"midMean":0.152,"highMean":0.165}
  ],
  "energyCurve": {
    "stepSeconds": 0.5,
    "columns": ["time", "rms", "low", "mid", "high"],
    "rows": [
      [0,0.202,0.116,0.127,0.178],
      [0.5,0.356,0.264,0.239,0.14],
      [1,0.377,0.276,0.278,0.104],
      [1.5,0.319,0.209,0.273,0.104],
      [2,0.37,0.243,0.355,0.106],
      [2.5,0.364,0.272,0.243,0.102],
      [3,0.345,0.217,0.326,0.114],
      [3.5,0.348,0.238,0.276,0.141],
      [4,0.394,0.274,0.267,0.132],
      [4.5,0.412,0.304,0.298,0.133],
      [5,0.35,0.206,0.328,0.22],
      [5.5,0.36,0.253,0.295,0.161],
      [6,0.362,0.263,0.259,0.163],
      [6.5,0.373,0.27,0.256,0.12],
      [7,0.307,0.2,0.253,0.157],
      [7.5,0.357,0.247,0.303,0.156],
      [8,0.381,0.284,0.252,0.106],
      [8.5,0.352,0.237,0.301,0.152],
      [9,0.358,0.228,0.32,0.114],
      [9.5,0.379,0.262,0.318,0.115],
      [10,0.402,0.277,0.308,0.21],
      [10.5,0.383,0.239,0.328,0.175],
      [11,0.377,0.26,0.312,0.224],
      [11.5,0.373,0.284,0.199,0.121],
      [12,0.376,0.272,0.275,0.117],
      [12.5,0.346,0.198,0.358,0.1],
      [13,0.362,0.243,0.321,0.144],
      [13.5,0.369,0.266,0.298,0.156],
      [14,0.282,0.185,0.275,0.184],
      [14.5,0.278,0.157,0.319,0.123],
      [15,0.473,0.279,0.505,0.463],
      [15.5,0.476,0.282,0.515,0.505],
      [16,0.505,0.322,0.489,0.418],
      [16.5,0.547,0.355,0.537,0.471],
      [17,0.523,0.365,0.444,0.398],
      [17.5,0.525,0.32,0.532,0.457],
      [18,0.533,0.373,0.476,0.417],
      [18.5,0.545,0.364,0.527,0.435],
      [19,0.502,0.32,0.505,0.46],
      [19.5,0.515,0.335,0.488,0.442],
      [20,0.547,0.381,0.474,0.487],
      [20.5,0.536,0.36,0.511,0.436],
      [21,0.551,0.375,0.518,0.499],
      [21.5,0.555,0.352,0.57,0.544],
      [22,0.554,0.365,0.54,0.517],
      [22.5,0.516,0.337,0.472,0.534],
      [23,0.512,0.323,0.535,0.485],
      [23.5,0.5,0.314,0.533,0.441],
      [24,0.482,0.289,0.5,0.464],
      [24.5,0.583,0.405,0.522,0.463],
      [25,0.576,0.406,0.491,0.439],
      [25.5,0.465,0.259,0.536,0.536],
      [26,0.533,0.359,0.49,0.483],
      [26.5,0.523,0.338,0.518,0.534],
      [27,0.509,0.342,0.477,0.443],
      [27.5,0.499,0.313,0.49,0.526],
      [28,0.555,0.369,0.496,0.472],
      [28.5,0.601,0.41,0.556,0.514],
      [29,0.549,0.338,0.582,0.619],
      [29.5,0.582,0.371,0.586,0.584],
      [30,0.369,0.179,0.442,0.368],
      [30.5,0.402,0.198,0.506,0.239],
      [31,0.343,0.176,0.405,0.178],
      [31.5,0.372,0.175,0.467,0.152],
      [32,0.382,0.232,0.404,0.154],
      [32.5,0.404,0.239,0.479,0.136],
      [33,0.374,0.223,0.418,0.25],
      [33.5,0.367,0.247,0.328,0.213],
      [34,0.414,0.215,0.49,0.226],
      [34.5,0.394,0.24,0.421,0.113],
      [35,0.399,0.241,0.444,0.135],
      [35.5,0.425,0.268,0.45,0.147],
      [36,0.429,0.259,0.478,0.197],
      [36.5,0.508,0.361,0.442,0.281],
      [37,0.497,0.342,0.47,0.268],
      [37.5,0.554,0.396,0.513,0.185],
      [38,0.544,0.388,0.484,0.234],
      [38.5,0.551,0.372,0.556,0.191],
      [39,0.508,0.358,0.444,0.163],
      [39.5,0.503,0.362,0.424,0.178],
      [40,0.563,0.399,0.479,0.155],
      [40.5,0.496,0.326,0.504,0.282],
      [41,0.476,0.33,0.422,0.215],
      [41.5,0.587,0.455,0.364,0.191],
      [42,0.54,0.413,0.38,0.224],
      [42.5,0.626,0.482,0.427,0.133],
      [43,0.507,0.349,0.488,0.181],
      [43.5,0.555,0.38,0.527,0.224],
      [44,0.501,0.344,0.434,0.449],
      [44.5,0.504,0.317,0.504,0.528],
      [45,0.48,0.317,0.465,0.291],
      [45.5,0.471,0.31,0.49,0.187],
      [46,0.506,0.311,0.54,0.285],
      [46.5,0.518,0.369,0.408,0.382],
      [47,0.461,0.327,0.394,0.219],
      [47.5,0.517,0.371,0.411,0.383],
      [48,0.512,0.341,0.514,0.184],
      [48.5,0.542,0.343,0.592,0.224],
      [49,0.5,0.339,0.491,0.256],
      [49.5,0.508,0.351,0.467,0.295],
      [50,0.456,0.307,0.424,0.235],
      [50.5,0.442,0.295,0.423,0.331],
      [51,0.482,0.339,0.405,0.264],
      [51.5,0.48,0.29,0.549,0.365],
      [52,0.51,0.343,0.498,0.206],
      [52.5,0.54,0.349,0.584,0.265],
      [53,0.512,0.349,0.477,0.153],
      [53.5,0.492,0.322,0.515,0.208],
      [54,0.548,0.399,0.434,0.295],
      [54.5,0.595,0.438,0.461,0.233],
      [55,0.488,0.311,0.516,0.326],
      [55.5,0.481,0.331,0.43,0.194],
      [56,0.487,0.342,0.434,0.289],
      [56.5,0.526,0.363,0.504,0.204],
      [57,0.547,0.366,0.511,0.382],
      [57.5,0.482,0.303,0.475,0.203],
      [58,0.51,0.294,0.465,0.242],
      [58.5,0.583,0.408,0.527,0.172],
      [59,0.298,0.146,0.362,0.216],
      [59.5,0.572,0.303,0.701,0.676],
      [60,0.624,0.335,0.776,0.678],
      [60.5,0.576,0.315,0.708,0.61],
      [61,0.633,0.386,0.699,0.634],
      [61.5,0.596,0.321,0.753,0.61],
      [62,0.569,0.301,0.708,0.613],
      [62.5,0.592,0.325,0.714,0.682],
      [63,0.62,0.361,0.701,0.632],
      [63.5,0.583,0.299,0.722,0.681],
      [64,0.656,0.333,0.86,0.528],
      [64.5,0.681,0.424,0.737,0.603],
      [65,0.638,0.343,0.809,0.614],
      [65.5,0.609,0.323,0.738,0.655],
      [66,0.574,0.323,0.679,0.666],
      [66.5,0.603,0.349,0.694,0.684],
      [67,0.572,0.334,0.654,0.644],
      [67.5,0.594,0.318,0.734,0.639],
      [68,0.605,0.339,0.73,0.634],
      [68.5,0.631,0.331,0.825,0.575],
      [69,0.604,0.318,0.772,0.611],
      [69.5,0.574,0.332,0.663,0.625],
      [70,0.629,0.376,0.701,0.698],
      [70.5,0.687,0.44,0.737,0.536],
      [71,0.638,0.355,0.762,0.675],
      [71.5,0.616,0.38,0.69,0.598],
      [72,0.618,0.358,0.714,0.672],
      [72.5,0.581,0.343,0.614,0.8],
      [73,0.615,0.354,0.667,0.776],
      [73.5,0.65,0.369,0.761,0.791],
      [74,0.463,0.18,0.662,0.65],
      [74.5,0.382,0.064,0.638,0.492],
      [75,0.332,0.046,0.549,0.491],
      [75.5,0.372,0.064,0.588,0.59],
      [76,0.509,0.326,0.459,0.499],
      [76.5,0.378,0.167,0.554,0.346],
      [77,0.474,0.237,0.617,0.461],
      [77.5,0.524,0.323,0.59,0.473],
      [78,0.615,0.414,0.521,0.587],
      [78.5,0.485,0.259,0.597,0.626],
      [79,0.464,0.26,0.564,0.543],
      [79.5,0.55,0.354,0.566,0.471],
      [80,0.508,0.318,0.52,0.422],
      [80.5,0.476,0.295,0.525,0.336],
      [81,0.525,0.376,0.466,0.255],
      [81.5,0.489,0.313,0.507,0.405],
      [82,0.506,0.322,0.588,0.222],
      [82.5,0.494,0.318,0.551,0.297],
      [83,0.516,0.344,0.518,0.274],
      [83.5,0.441,0.265,0.503,0.192],
      [84,0.366,0.162,0.548,0.239],
      [84.5,0.423,0.23,0.567,0.252],
      [85,0.551,0.378,0.526,0.312],
      [85.5,0.485,0.323,0.504,0.211],
      [86,0.53,0.381,0.498,0.195],
      [86.5,0.548,0.406,0.435,0.313],
      [87,0.618,0.449,0.507,0.257],
      [87.5,0.571,0.428,0.446,0.264],
      [88,0.465,0.245,0.539,0.382],
      [88.5,0.422,0.072,0.697,0.529],
      [89,0.385,0.106,0.619,0.464],
      [89.5,0.403,0.11,0.663,0.415],
      [90,0.407,0.143,0.651,0.309],
      [90.5,0.414,0.169,0.643,0.296],
      [91,0.436,0.264,0.448,0.479],
      [91.5,0.489,0.251,0.645,0.456],
      [92,0.459,0.236,0.616,0.375],
      [92.5,0.536,0.34,0.613,0.233],
      [93,0.554,0.385,0.546,0.275],
      [93.5,0.522,0.333,0.592,0.322],
      [94,0.538,0.388,0.458,0.303],
      [94.5,0.617,0.409,0.63,0.482],
      [95,0.548,0.388,0.485,0.424],
      [95.5,0.552,0.241,0.72,0.402],
      [96,0.503,0.189,0.706,0.398],
      [96.5,0.428,0.146,0.691,0.217],
      [97,0.494,0.133,0.826,0.134],
      [97.5,0.485,0.244,0.607,0.321],
      [98,0.443,0.125,0.726,0.354],
      [98.5,0.391,0.071,0.662,0.38],
      [99,0.54,0.215,0.782,0.243],
      [99.5,0.41,0.062,0.684,0.458],
      [100,0.535,0.282,0.619,0.472],
      [100.5,0.376,0.049,0.669,0.203],
      [101,0.394,0.034,0.703,0.167],
      [101.5,0.349,0.046,0.614,0.159],
      [102,0.367,0.026,0.641,0.284],
      [102.5,0.366,0.017,0.655,0.21],
      [103,0.441,0.19,0.522,0.409],
      [103.5,0.549,0.365,0.555,0.278],
      [104,0.527,0.31,0.598,0.424],
      [104.5,0.535,0.331,0.6,0.331],
      [105,0.54,0.356,0.55,0.342],
      [105.5,0.541,0.329,0.632,0.291],
      [106,0.583,0.327,0.711,0.34],
      [106.5,0.556,0.32,0.666,0.306],
      [107,0.534,0.302,0.648,0.478],
      [107.5,0.6,0.345,0.737,0.305],
      [108,0.574,0.336,0.669,0.337],
      [108.5,0.572,0.327,0.71,0.312],
      [109,0.577,0.337,0.697,0.321],
      [109.5,0.557,0.308,0.71,0.34],
      [110,0.507,0.26,0.657,0.434],
      [110.5,0.514,0.287,0.623,0.46],
      [111,0.555,0.323,0.638,0.359],
      [111.5,0.528,0.316,0.606,0.435],
      [112,0.516,0.293,0.584,0.456],
      [112.5,0.552,0.291,0.693,0.478],
      [113,0.482,0.262,0.579,0.448],
      [113.5,0.538,0.322,0.59,0.45],
      [114,0.468,0.251,0.566,0.427],
      [114.5,0.529,0.256,0.704,0.406],
      [115,0.452,0.218,0.596,0.449],
      [115.5,0.481,0.259,0.586,0.425],
      [116,0.509,0.306,0.567,0.439],
      [116.5,0.494,0.243,0.637,0.463],
      [117,0.566,0.306,0.706,0.518],
      [117.5,0.642,0.335,0.806,0.523],
      [118,0.59,0.301,0.743,0.598],
      [118.5,0.52,0.325,0.586,0.446],
      [119,0.577,0.327,0.696,0.409],
      [119.5,0.514,0.313,0.571,0.361],
      [120,0.53,0.313,0.59,0.442],
      [120.5,0.547,0.327,0.636,0.391],
      [121,0.55,0.32,0.662,0.351],
      [121.5,0.551,0.323,0.642,0.399],
      [122,0.556,0.305,0.694,0.386],
      [122.5,0.573,0.319,0.716,0.347],
      [123,0.571,0.325,0.709,0.346],
      [123.5,0.605,0.328,0.771,0.411],
      [124,0.55,0.299,0.696,0.314],
      [124.5,0.586,0.342,0.687,0.46],
      [125,0.568,0.301,0.699,0.505],
      [125.5,0.569,0.319,0.681,0.458],
      [126,0.572,0.296,0.753,0.458],
      [126.5,0.559,0.269,0.749,0.336],
      [127,0.564,0.297,0.723,0.443],
      [127.5,0.547,0.288,0.699,0.385],
      [128,0.586,0.338,0.717,0.422],
      [128.5,0.498,0.312,0.496,0.413],
      [129,0.54,0.301,0.637,0.423],
      [129.5,0.572,0.357,0.613,0.408],
      [130,0.569,0.36,0.579,0.496],
      [130.5,0.565,0.346,0.642,0.403],
      [131,0.503,0.3,0.55,0.465],
      [131.5,0.561,0.291,0.729,0.436],
      [132,0.569,0.329,0.667,0.523],
      [132.5,0.567,0.289,0.718,0.525],
      [133,0.535,0.253,0.735,0.489],
      [133.5,0.513,0.286,0.609,0.422],
      [134,0.48,0.308,0.505,0.215],
      [134.5,0.487,0.309,0.534,0.258],
      [135,0.517,0.353,0.485,0.2],
      [135.5,0.506,0.321,0.548,0.246],
      [136,0.535,0.322,0.605,0.265],
      [136.5,0.511,0.339,0.513,0.223],
      [137,0.496,0.317,0.528,0.22],
      [137.5,0.483,0.317,0.494,0.227],
      [138,0.502,0.313,0.556,0.28],
      [138.5,0.499,0.32,0.515,0.189],
      [139,0.481,0.309,0.517,0.276],
      [139.5,0.475,0.287,0.551,0.219],
      [140,0.492,0.284,0.586,0.297],
      [140.5,0.445,0.266,0.488,0.308],
      [141,0.418,0.237,0.477,0.286],
      [141.5,0.519,0.363,0.453,0.228],
      [142,0.456,0.284,0.494,0.253],
      [142.5,0.52,0.357,0.436,0.257],
      [143,0.498,0.329,0.512,0.25],
      [143.5,0.493,0.324,0.501,0.279],
      [144,0.51,0.325,0.485,0.203],
      [144.5,0.551,0.407,0.434,0.23],
      [145,0.439,0.29,0.434,0.176],
      [145.5,0.566,0.4,0.533,0.29],
      [146,0.476,0.3,0.505,0.214],
      [146.5,0.483,0.333,0.444,0.271],
      [147,0.489,0.321,0.478,0.266],
      [147.5,0.508,0.303,0.57,0.389],
      [148,0.539,0.369,0.483,0.424],
      [148.5,0.526,0.325,0.574,0.299],
      [149,0.499,0.318,0.513,0.387],
      [149.5,0.52,0.314,0.588,0.274],
      [150,0.512,0.334,0.505,0.38],
      [150.5,0.519,0.336,0.556,0.306],
      [151,0.514,0.303,0.58,0.377],
      [151.5,0.506,0.328,0.521,0.278],
      [152,0.499,0.304,0.565,0.343],
      [152.5,0.511,0.328,0.528,0.291],
      [153,0.514,0.32,0.557,0.334],
      [153.5,0.492,0.295,0.552,0.33],
      [154,0.551,0.342,0.594,0.296],
      [154.5,0.518,0.327,0.541,0.344],
      [155,0.475,0.29,0.514,0.313],
      [155.5,0.483,0.307,0.493,0.4],
      [156,0.495,0.305,0.558,0.3],
      [156.5,0.464,0.232,0.57,0.422],
      [157,0.506,0.302,0.551,0.309],
      [157.5,0.508,0.318,0.539,0.38],
      [158,0.489,0.283,0.55,0.351],
      [158.5,0.515,0.263,0.634,0.43],
      [159,0.441,0.258,0.516,0.296],
      [159.5,0.478,0.308,0.45,0.237],
      [160,0.392,0.15,0.588,0.187],
      [160.5,0.456,0.294,0.484,0.261],
      [161,0.475,0.286,0.534,0.198],
      [161.5,0.395,0.219,0.488,0.211],
      [162,0.424,0.233,0.495,0.389],
      [162.5,0.564,0.347,0.601,0.461],
      [163,0.542,0.295,0.639,0.697],
      [163.5,0.577,0.281,0.756,0.648],
      [164,0.593,0.306,0.745,0.686],
      [164.5,0.602,0.337,0.727,0.572],
      [165,0.639,0.346,0.824,0.597],
      [165.5,0.547,0.312,0.628,0.633],
      [166,0.596,0.318,0.721,0.665],
      [166.5,0.717,0.459,0.766,0.574],
      [167,0.606,0.313,0.756,0.628],
      [167.5,0.613,0.281,0.832,0.607],
      [168,0.601,0.339,0.705,0.647],
      [168.5,0.627,0.323,0.829,0.625],
      [169,0.61,0.337,0.738,0.713],
      [169.5,0.635,0.391,0.684,0.724],
      [170,0.606,0.338,0.729,0.683],
      [170.5,0.553,0.304,0.662,0.609],
      [171,0.56,0.311,0.68,0.568],
      [171.5,0.559,0.285,0.725,0.579],
      [172,0.618,0.364,0.73,0.514],
      [172.5,0.644,0.338,0.801,0.617],
      [173,0.656,0.44,0.62,0.612],
      [173.5,0.592,0.344,0.685,0.637],
      [174,0.657,0.368,0.82,0.584],
      [174.5,0.61,0.359,0.702,0.619],
      [175,0.616,0.355,0.712,0.68],
      [175.5,0.601,0.35,0.685,0.658],
      [176,0.593,0.319,0.725,0.739],
      [176.5,0.658,0.355,0.826,0.695],
      [177,0.597,0.289,0.8,0.696],
      [177.5,0.568,0.278,0.701,0.801],
      [178,0.603,0.292,0.764,0.747],
      [178.5,0.581,0.282,0.713,0.698],
      [179,0.619,0.327,0.741,0.701],
      [179.5,0.567,0.312,0.695,0.574],
      [180,0.631,0.364,0.741,0.618],
      [180.5,0.587,0.326,0.728,0.545],
      [181,0.561,0.302,0.707,0.596],
      [181.5,0.573,0.296,0.69,0.764],
      [182,0.605,0.36,0.68,0.628],
      [182.5,0.588,0.323,0.696,0.704],
      [183,0.645,0.4,0.679,0.621],
      [183.5,0.615,0.357,0.716,0.694],
      [184,0.6,0.326,0.726,0.635],
      [184.5,0.579,0.308,0.72,0.608],
      [185,0.627,0.297,0.852,0.669],
      [185.5,0.585,0.287,0.774,0.63],
      [186,0.553,0.267,0.708,0.647],
      [186.5,0.618,0.363,0.703,0.655],
      [187,0.601,0.369,0.624,0.741],
      [187.5,0.578,0.286,0.758,0.608],
      [188,0.564,0.316,0.664,0.568],
      [188.5,0.67,0.414,0.75,0.529],
      [189,0.629,0.348,0.777,0.628],
      [189.5,0.593,0.329,0.717,0.62],
      [190,0.632,0.365,0.731,0.653],
      [190.5,0.587,0.351,0.656,0.584],
      [191,0.574,0.326,0.651,0.666],
      [191.5,0.557,0.283,0.715,0.588],
      [192,0.532,0.268,0.641,0.635],
      [192.5,0.394,0.284,0.308,0.31],
      [193,0.42,0.316,0.248,0.219],
      [193.5,0.364,0.263,0.234,0.285],
      [194,0.392,0.282,0.301,0.211],
      [194.5,0.414,0.312,0.236,0.278],
      [195,0.374,0.25,0.293,0.211],
      [195.5,0.396,0.272,0.312,0.281],
      [196,0.424,0.313,0.269,0.168],
      [196.5,0.443,0.339,0.278,0.294],
      [197,0.375,0.228,0.349,0.22],
      [197.5,0.402,0.292,0.306,0.257],
      [198,0.407,0.306,0.237,0.234],
      [198.5,0.404,0.3,0.267,0.23],
      [199,0.346,0.226,0.29,0.269],
      [199.5,0.386,0.272,0.294,0.306],
      [200,0.42,0.32,0.232,0.306],
      [200.5,0.386,0.264,0.301,0.255],
      [201,0.391,0.255,0.319,0.32],
      [201.5,0.413,0.304,0.266,0.225],
      [202,0.42,0.307,0.271,0.301],
      [202.5,0.395,0.253,0.338,0.232],
      [203,0.402,0.29,0.305,0.269],
      [203.5,0.412,0.321,0.21,0.235],
      [204,0.41,0.302,0.268,0.255],
      [204.5,0.361,0.21,0.361,0.26],
      [205,0.381,0.262,0.323,0.204],
      [205.5,0.385,0.281,0.274,0.296],
      [206,0.408,0.252,0.378,0.411],
      [206.5,0.377,0.157,0.512,0.584],
      [207,0.171,0.095,0.196,0.21],
      [207.5,0.003,0,0.004,0.005],
      [208,0,0,0,0],
      [208.5,0,0,0,0],
      [209,0,0,0,0],
      [209.5,0,0,0,0],
      [210,0,0,0,0],
      [210.5,0,0,0,0],
      [211,0,0,0,0],
      [211.5,0,0,0,0]
    ]
  },
  "silenceGaps": [[207.467,211.907]]
}
```


## 4. 外部输入、音频内联与“信息不足”处理

### 4.1 生成前的外部输入清单

| 输入 | 是否必需 | 形式 | 在最终成品中的位置 |
|---|---|---|---|
| 音频 | 必需 | 使用者单独提供：附件/本地文件/base64 均可 | 必须内联为 data URI，不能外链 |
| 歌词原文 | 必须 | 必须与 §2 节奏表行数与顺序一致 | 若提供：必须在 HTML 内联并逐字显示；若不提供：不得出现任何歌词原文 |

本提示词不含歌词原文、不含音频字节，这是刻意设计，不是数据事故。

### 4.2 歌词外部协议

- 使用者提供的歌词应共 129 行，按 i=0–128 对应 §2。
- 若歌词行数或顺序与节奏表不一致：**视为外部歌词不可用，改走无词模式**（成品不显示任何歌词），并标记“信息不足”；不得自行删行、补行、调序，也不得从其他版本拼接。
- 歌词文本只能来自使用者的外部提供；禁止使用模型记忆中的歌词、网络搜索、官方 MV 字幕、其他翻唱版本。
- 无词模式下，即使工作区/附件中存在含歌词文本的素材文件（例如本资料包源文件 lyric_timing_exam.json），也不得读取、引用或显示其中的歌词；只有使用者明确指定为“歌词输入”的内容才可使用。
- 若未提供歌词：成品中不得出现任何歌词原文（含画面、隐藏文本、注释、aria-label、HTML title、控制台日志等），也不得用相近文本替代；视觉依据节奏/能量/结构展开。**例外：曲名 world.execute(me); 作为作品标识，允许出现在 HTML title、片头与元数据中；§2/§3 数据里的 meta.title 也是曲名元数据，允许保留；不得据此扩展显示/编造其他歌词。**
- 若提供了歌词且行数/顺序校验通过：HTML 内必须包含完整 129 行文本数据（i + text）并逐字渲染；不得翻译、润色、补写、调序。

### 4.3 音频内联协议（最终成品的关键）

- 将使用者提供的音频**原样**转为 base64 data URI：data:audio/...;base64,...
- 在 HTML 中保留一个全局常量 AUDIO_DATA_URI 保存该 data URI；播放器从它取源。该常量不得为空（除非按 §4.5 判定信息不足）、不得是 http(s) 地址、相对路径或文件名。
- 音频只作听觉层；歌词出现仍只由 start 时间戳决定。可用 audio.currentTime 作为全局时间 t 的来源。
- 不得重新编码、裁剪、变速、变调或降质（除非使用者提供的本身就是最终版）；不得替换为其他曲目或合成音频。
- base64 体积约为原文件的 4/3，成品 HTML 达到数 MB 属正常；不得因为体积大而删减音频或改为外链。
- 如果执行环境具备文件读写/脚本能力：建议直接用脚本读取音频并写入 HTML，避免把数 MB base64 抄进对话。如果只能纯聊天输出、无法完成内联：明确说明平台限制，不得用外链、占位符或假 base64 冒充完成品。
- 最终 HTML 必须能在 Chrome 中实际播放该音频；若音频无法解码或时长异常，先检查 MIME 与数据完整性，按信息不足处理。
- 音频版本：如果使用者没有说明其音频与素材分析所用版本一致，不得假定完全对齐；发现整体偏移时按信息不足处理，不得改动时间戳去迁就音频。

### 4.4 最终成品形态

- 一个 standalone .html 文件：HTML/CSS/JS/歌词（若提供）/音频（base64）全部在内；双击 file:// 可运行。
- 不产生第二个文件（不输出单独音频、字体、图片、JS、CSS）；不引用本地或远程外部路径。
- 页面必须提供可点击的播放/开始入口：浏览器自动播放策略通常禁止无手势播放音频；不得用“静音自动播放”伪装成片；用户点击后，音频与时间轴一起开始。
- 音频结束或时间轴到达 211.9067s 后，进入明确的结束状态，并可重播；不得在结尾黑屏/卡死。

### 4.5 必须标注“信息不足”的清单（最低要求）

在交付说明中逐条说明“缺什么 / 影响了什么判断 / 你如何在不编造的前提下处理”。至少覆盖：

1. 音频文件缺失、损坏、无法读取或无法内联；
2. 歌词原文未提供、行数与节奏表不符、或来源可疑；
3. 逐字/逐音素/逐音的演唱时刻与音高——本包只有行级时间窗与行级能量；
4. 起音（onset）事件列表——只有 arrangement 的 onsetDensity；该密度的单位也未说明；
5. 人声/伴奏分离、和声、调性、和弦进行；
6. arrangement 标签的生成方法、section 编号的语义；
7. 音频分析所用的窗长、归一化、频段划分、算法参数；
8. 官方 MV、创作背景、乐评、任何外部参考（本任务禁止引用）；
9. 音频版本/对齐信息——无法确认提供的音频与素材分析所用版本一致时，行时间戳可能整体偏移，需重新对齐或标注信息不足；
10. 任何其他你试图使用但数据中没有的信息。

“信息不足”不是免责声明：已有的数据仍要用来做大胆而具体的判断；只是不要把缺失的信息伪装成事实。

### 4.6 缺失时的交付底线

- 缺音频：不得用外链、替代曲或“假装已内联”的假静音冒充完成；应明确记录“音频未内联”，并说明这是未完成项。
- 缺歌词：可以做“无词节奏版”，但必须在交付说明中写明“歌词原文未提供，成品不含歌词”，且不得编造任何歌词。
- 两者都缺：只能交付节奏/结构视觉原型，并明确标注未完成。

## 5. 不可协商的技术契约

### 5.1 核心契约（保留原始措辞）

- 歌词逐字锁定，不翻译、不润色、不补写、不调序。
- 行级时间戳是歌词出现的唯一时钟。行内节奏由你自己从音频中分析决定。
- 单文件 HTML，内联 CSS/JS，双击在 Chrome 中可运行。
- 不引用任何外部图片、视频、字体、音效、乐评或参考资料。所有视觉由代码生成，所有判断基于你收到的数据和音频本身。
- 如果某项信息不足以支撑一个判断，明确写出“信息不足”，不要编造。

> 注：v2 中音频是外部输入；上述第 2 条与 §5.2 的口径一起执行——音频用于播放/对齐/校验，行内节奏的数值依据以 §2/§3 为主；只有实际做了额外音频分析，才可按 §3.1 说明使用。

### 5.2 本资料包的执行口径（不改变原意，只补足素材缺口）

- 第 1 条：歌词原文由使用者外部提供。提供时必须逐字锁定、按 i 对齐；未提供时不得显示或编造歌词。
- 第 2 条：音频由使用者外部提供并内联，用于播放、时间轴对齐与完整性校验；“行内节奏”的数值依据以 §2 节奏数据、§3 音频分析为准（若提供歌词，可用文本长度作辅助）。如果确实做了额外的音频分析并使用，必须说明方法与局限，不得与给定数据矛盾；不得编造未提供的细节；缺失处写“信息不足”。
- 第 3 条：单文件 HTML 指最终成品一个文件，且音频必须以 base64 data URI 内联在其中（见 §4.3）。
- 第 4 条：音频是外部输入，不是运行时外部引用；最终 HTML 不得在运行时请求任何外部资源，也不得把“音频外部提供”误解为“HTML 可以外链音频”。
- 第 5 条：至少出现在交付说明里，HTML 源码注释中留一份亦可。该做的判断仍要基于已有数据大胆做。

### 5.3 工程硬约束（必须满足）

1. 最终产物是一个可保存为 .html 的 standalone 完整源码（本轮以对话中的完整代码块或工作区文件交付）；HTML/CSS/JS/歌词/音频全部内联；无构建步骤、无依赖安装。
2. 无任何网络请求：不得使用 fetch、XMLHttpRequest、WebSocket、EventSource、外部 URL 的 script/link/img/video/audio/iframe 引用、@import、@font-face 外链、外部 CSS url()、遥测或统计。内联 data: URI 的音频不在禁止之列。Chrome DevTools 的 Network 面板应为 0 个请求（file:// 自身除外）。
3. 必须能通过 file:// 双击打开；不得依赖本地服务器、模块加载器、CORS 或跨文件读取。
4. 视觉全部由代码生成：Canvas 2D、WebGL、SVG、CSS、程序化绘制均可；不得使用外部图片、视频、GIF、贴图、字体文件、音效文件（§4.3 的 data: URI 音频是唯一例外：仅内联、只作听觉层）。
5. 字体：使用系统字体栈，或直接用代码绘制字形；不得 @font-face 加载外部字体。
6. 数据内联：§2 节奏数据与 §3 中你实际引用到的分析数值必须内联且保持原值；歌词（若提供且行数/顺序校验通过）必须完整内联 129 行；音频必须内联为 base64 data URI；不得运行时从文件加载。§2 节奏数据可整块内联，其 meta.title 为曲名元数据，允许保留。不要求把 §3 全表逐条抄进 HTML，但不得改写、取整或伪造数值。
7. 文本渲染必须保留字符：大小写、撇号、连字符、括号、分号、空格都不许改；HTML 转义只能影响源码写法，最终显示必须一字不差。
8. 处理窗口缩放与高 DPI（devicePixelRatio）；缩放不得重置时间轴或跳帧。
9. 调试/未完成状态不得白屏或抛出未捕获错误：音频缺失时应显示醒目的“未完成：音频未内联”状态，并允许静音浏览视觉；该形态不是交付物，不得被当作静音版成品交付。
10. 性能以最低不低于30fps为目标，但不得卡死、爆内存或黑屏。
11. 不要使用 alert/confirm/prompt 阻塞流程；不要自动全屏；不要自动下载文件；不得自动播放带声音的音频（必须由用户手势启动）。
12. 交付代码必须能直接保存运行：不得有“此处省略”“TODO”“与上面相同”之类占位；AUDIO_DATA_URI 必须是真实可解码的音频数据，不得用假字符串充数。

### 5.4 与视觉主张冲突时

若某条技术契约与你的视觉主张冲突，可以提出替代方案并说明理由，但：产物必须可运行；不得改动歌词、歌词顺序、行级时间戳；不得破坏单文件离线、无外部资源与音频内联的底线。

## 6. 时间轴与行内节奏的实现规则

1. 全局时间轴 t 单位为秒，范围 [0, 211.9067]。
2. 时间源：最终 HTML 含内联音频时，用户点击开始后用 audio.currentTime 作为 t；若音频加载失败，则用 requestAnimationFrame + performance.now 作为降级时间源，并显示“音频未提供/加载失败”的提示。两种方式都不得改变歌词 start/end 的对应关系。
3. 歌词可见性（仅在提供了歌词时）：t < start 时完全不可见（不透明度 0 或不绘制），不得提前露出可读文字；t 到达 start 时进入可见状态，可以开始入场动画；start <= t < end 时作为当前行呈现（端点归下一行；若下一行从同一时刻开始则由下一行接管）；t >= end 后不再作为当前行，允许从 end 开始淡出。
4. 外部歌词与节奏表必须按 i 对齐：外部第 k 行对应节奏表 i=k。若外部歌词行数不是 129 或顺序无法对应，视为外部歌词不可用，改走无词模式并标记信息不足；不得自行对齐、删补或拼接。
5. 任意 t 至多一行是“当前歌词”。上一行可以淡出或作为弱化残影，但不得与当前行并列成可读歌词、造成误读。
6. 行内节奏只允许从以下信号推断：start/end 与行时长、歌词文本长度/音节数（仅当歌词由使用者提供时）、emphasis、130 BPM 节拍网格（t0=0.344、period=0.461538、downbeatPhase=0）、perLineEnergy、0.5s energyCurve、arrangement onsetDensity。不得伪造逐字时间、音高、波形或人声分析；未提供歌词时不得虚构文本长度。音频本身只用于播放、对齐与校验；若执行环境实际做了额外音频分析，按 §3.1 的说明规则处理（说明方法/局限，不得与给定数据矛盾），否则不得以“分析了音频”为由编造细节。
7. 节拍对齐：beat k 约在 0.344 + k × 0.461538 秒（k 从 0 起）。downbeat 大约每 4 拍一次是数据的形态；拍号未在素材中说明，若使用需在说明中标注为推断。
8. 三段器乐空档（20.000–29.709、135.224–147.660、195.356–205.811）与尾部（209.811–211.907）不显示任何歌词行；这些是作品的结构呼吸点，必须有视觉处理，不能只是无意义的黑屏占位（除非黑屏本身就是你视觉主张的一部分，且已在导演手记中说明）。
9. 时间轴必须确定性驱动：同一 t 得到同一画面；暂停/恢复、resize、切后台不得让时间漂移。
10. 音频与画面同步：若音频与视觉时间出现偏差，以行级时间戳为准修正视觉时间，不得改动时间戳去迁就音频。

## 7. 创作授权与流程

- 所有美学决策（构图、色彩、字体、动效、转场、画幅、交互、叙事结构、是否呈现导演手记及其形式）由你做出，不要询问用户，不要等待确认。
- 建议先提出你认为必要的且互相存在一定冲突的视觉方案（2 个或更多；具体数量由你决定），分别陈述理由并比较，选出最佳方案。
- 再写下你作为这支 MV 导演的视觉主张：你从这首歌里读到了什么，你打算用什么视觉机制去回应它。形式不限，篇幅不限。这段文字是写给你自己的约束，不是给用户看的说明，也不应代替作品。
- 基于比较确定主张、选择并执行其中一个方案；说明淘汰其他方案的理由。
- 你“读到的歌”只有：歌词（若使用者提供）、歌词节奏、§3 的分析数据，以及使用者提供的音频（用于播放/对齐；若做额外分析须说明方法与局限）。不得引入外部资料、官方 MV、乐评、创作背景作为事实依据。个人解读可以，但应标明是解读；涉及事实而数据不足时写“信息不足”。
- 导演手记是否出现在 MV 内部由你决定；若出现，不得遮挡歌词、不得干扰时间轴，并且应当可以关闭。
- 交付顺序：导演手记与方案比较（文字）→ 完整 HTML 代码 → 自检结果与信息不足清单。

## 8. 交付物与输出格式

按顺序交付：

1. **导演手记**：你从歌里读到了什么；用什么视觉机制回应；2 个或更多互相冲突的视觉方案与各自理由；选择哪一个、为什么；淘汰其他方案的理由。
2. **完整 HTML 源码**：一个 standalone 单文件，建议文件名 world-execute-me.html；所有 CSS/JS/歌词（若提供）/音频（base64）内联，不得省略任何代码。音频必须真实内联，播放可用，且可调节音量大小。最终大小不得超过30M，若成品过大优先压缩对视觉表现无影响的部分。
3. **自检结果与信息不足清单**：按 §9 逐项标注通过/不通过；按 §4.5 列出缺什么、影响什么、如何处理。

输出/执行长度受限时：

- 若执行环境有文件读写或脚本能力：优先用脚本完成音频编码与 HTML 写入，交付 .html 文件本身，不要把数 MB base64 塞进对话。
- 若只能纯聊天输出且无法内联数 MB 音频：明确说明平台限制，不要用外链、假 base64 或“稍后自行填入”冒充完成；此时任务应判定为未完成，而不是交付一个不合规的成品。
- 无论何种方式，最终交付物都必须是**一个**可保存并运行的 standalone HTML。

## 9. 验收与自检清单

- [ ] 最终只有一个 HTML 文件；HTML/CSS/JS/歌词（若提供）/音频全部内联；无第二个文件、无外部依赖。
- [ ] 音频以 data URI 内嵌，AUDIO_DATA_URI 非空且可解码；离线播放正常；DevTools Network 0 请求。
- [ ] 播放由用户手势启动；音频与时间轴同步；结束后有明确结束状态，可重播。
- [ ] 若提供了歌词且行数/顺序校验通过：129 行全部出现、逐字一致、顺序与 i 一致、无翻译/补写/改写；文本在 start 之前不可见、end 之后不再作为当前行。
- [ ] 若未提供歌词：除曲名元数据（HTML title、片头、§2/§3 的 meta.title）外，成品任何位置（画面/DOM/注释/aria-label/console）都不得出现歌词原文，也不得编造或使用模型记忆中的歌词。
- [ ] 129 个 start/end 使用原值，没有整体平移、缩放或取整到 0.1s。
- [ ] 器乐窗（20–29.709、135.224–147.66、195.356–205.811、209.811–211.907）内不出现歌词。
- [ ] 行内节奏只基于 §6 允许的数据；没有伪造逐字时间、波形、频谱或人声分析。
- [ ] 视觉全部由代码生成，无外部图片、视频、字体、音效、乐评或参考资料。
- [ ] 使用 130 BPM 网格（t0=0.344、period=0.461538）时没有擅自“修正”数据。
- [ ] 处理窗口缩放、高 DPI；时间轴不因 resize 重置。
- [ ] 控制台无未捕获错误；无明显卡顿、内存泄漏或黑屏。
- [ ] 信息不足清单存在，且没有与数据矛盾的断言；音频/歌词缺失情况如实标注。
- [ ] 交付内容包含完整 HTML 源码，而不是说明文档或设计稿。

## 10. 已知数据边界与怪点（使用数据前必读）

1. 最后一行（i=128）的 end=209.811 落在静音窗 207.467–211.907 内；它的 perLineEnergy.rmsMean=0.134 被静音拉低（rmsMax=0.905）。end 是素材的时间窗结束，不等于人声实际结束。
2. perLineEnergy 与 §2 节奏行逐项一致（i/start/end 相同，已校验）；但它是行窗口统计，不含行内时间分布。
3. energyCurve 以 0.5s 步长覆盖到 211.5s；最后约 0.4067s 没有采样。
4. 节拍网格 t0=0.344 不是 0；beat 0 在 0.344s。不要假设第一拍在 0。
5. arrangement 分段与歌词 section 边界接近但不相等（59.2 vs 59.223、147.7 vs 147.66 等）；不要把 label 直接当作歌词段落名。
6. section 编号 0–3 的含义未在素材中定义；只能当分组 id 使用。
7. onsetDensity 的单位、能量/频段的归一化方式、arrangement 的生成方法均未说明；只能作相对比较。
8. 现有素材中的歌词文本为 ASCII（含撇号 '）；外部提供的歌词以实际文本为准，转义可以，但不得改字符。
9. silenceGaps 只有一条；不要假设所有低能量段都是数字静音。
10. 素材未提供音频文件（音频由使用者另行提供并在成品内联）；素材也未提供调性、和弦、人声/乐器分离、官方 MV、创作背景、乐评；按 §4.5 处理。
11. silenceGaps 起点 207.467s 与 arrangement 里 silence 段的起点 207.5s 相差 0.033s；两者不是同一口径，不要当成同一个边界。
12. 首行歌词 start=0.100s 早于首拍 0.344s；按 beat 公式推 k 会小于 0。使用节拍网格时需处理开头 0.244s 的“网格之前”区域。

## 11. 来源、日期与边界

- 节奏数据从素材 lyric_timing_exam.json 的 i/start/end/section/emphasis 派生；**歌词 text 字段按使用者要求不写入本提示词**，生成时由使用者外部提供。
- 音频分析数据来自 audio_features_exam.json；不含歌词文本。
- 音频文件不在本提示词中，也不是本资料包内的文件；生成时由使用者单独提供并内联。
- 本任务书由“用户原始任务书 + 上述两个数据文件 + 本轮新增约束（不含歌词原文、外部音频内联、standalone 成品）”合并生成，日期 2026-11-14。
- 曲名“world.execute(me);”作为作品标识保留在元数据中；它同时是一句歌词，这是元数据层面的重叠，本提示词不含其余歌词正文。
- 歌词与乐曲版权归原权利人；本任务书仅用于用户指定的个人创作，不包含音频与歌词正文。
