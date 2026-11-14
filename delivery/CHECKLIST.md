# CHECKLIST — 验收与自检

> 自检只检查硬约束（合规项），不属于创作内容。详细条款以 [CONTRACT.md](../CONTRACT.md)、[data/DATA_NOTES.md](../data/DATA_NOTES.md)、[delivery/FORMAT.md](FORMAT.md) 为准。

- [ ] 已完整阅读四份核心文档：README.md、TASK.md、CONTRACT.md、CREATIVE.md（未读完不得开始生成）
- [ ] 交付为单个 HTML，CSS/JS/歌词/音频全部内联；无第二个文件、无外部依赖（CONTRACT.md §4）
- [ ] 音频以 data URI 内嵌，AUDIO_DATA_URI 非空且可解码；离线播放正常；DevTools Network 0 请求（CONTRACT.md §5）
- [ ] 播放由用户手势启动；音频与时间轴同步；结束后有明确结束状态，可重播（CONTRACT.md §3）
- [ ] 歌词（校验通过时）：129 行全部出现、逐字一致、顺序与 i 一致、无翻译/补写/改写；文本在 start 之前不可见、end 之后不再作为当前行（CONTRACT.md §1、§3）
- [ ] 歌词不可用时的降级形态符合 INPUT 协议，并在交付说明中标注"信息不足"（input/README.md）
- [ ] 129 个 start/end 使用 data/lyrics_timing.json 原值，没有整体平移、缩放或取整（CONTRACT.md §1）
- [ ] instrumentalWindows 内不出现歌词（data/lyrics_timing.json、CONTRACT.md §1）
- [ ] 听感/推测没有被写成数据或测量值；听感可以驱动视觉（CONTRACT.md §2）
- [ ] 视觉全部由代码生成，无外部图片/视频/字体/音效/乐评（CONTRACT.md §4）
- [ ] 节拍网格参数（见 data/DATA_NOTES.md）未被擅自"修正"或重算
- [ ] 处理窗口缩放、高 DPI；时间轴不因 resize 重置（CONTRACT.md §4）
- [ ] 控制台无未捕获错误；无明显卡顿、内存泄漏或黑屏（CONTRACT.md §4）
- [ ] 边界交代清单存在，且无与数据矛盾的断言；音频/歌词缺失情况如实标注（delivery/FORMAT.md）
- [ ] 交付内容包含完整 HTML 源码，而不是说明文档或设计稿
- [ ] 导演手记篇幅从简，但能说明解读与具体段落的对应（CREATIVE.md §4）
- [ ] 已从仓库根目录读取 audio.* 与 lyrics.txt，并按要求内联 / 逐字使用（input/README.md）
- [ ] data/*.json 未被改动（CONTRACT.md §6）
