# 剩余短音效处理任务清单 (Task Handoff)

> **项目**: 《诡街》 Godot 音频资源
> **目标**: 将所有非循环短音效按原始规格准备完成
> **接收者**: Grok Build / 音频后期代理
> **日期**: 2026-05-29

---

## 1. 任务概述

本文档列出所有需要从网上下载、处理、命名的短音效任务。
循环类音效 (U1/U2/U5) 已交由 Suno 生成代理处理。

**格式要求** (严格执行):
- **格式**: WAV
- **采样率**: 44100 Hz
- **位深**: 16-bit
- **声道**: Mono (单声道)
- **响度**: 峰值 约 -6 dBFS，建议使用 loudnorm
- **时长**: 按原始文档中指定范围

---

## 2. 整体处理流程

1. 从推荐来源下载原始音效
2. **Audacity** 中修剪、正规化、添加空间感 / 距离感 / 节奏错位
3. **ffmpeg** 强制转换为规格
4. 按文档命名存放到 `assets/audio/sfx/...` 对应目录
5. `git add` + `git commit` + `git push` (LFS 自动处理)

---

## 3. UI 音效任务列表 (sfx/ui/)

| 任务ID | 文件名 | 时长目标 | 特殊要求 | 推荐来源 | 处理重点 | 状态 |
|----------|----------|-------------|-------------|-------------|-------------|--------|
| UI-01 | ui_button_click.wav | ~0.1s | 清脆、老式打字机感 | Pixabay UI / Mixkit Click | 短、干净 | 待处理 |
| UI-02 | ui_button_hover.wav | ~0.05s | 极短高频 | ElevenLabs / Pixabay | 极短 | 待处理 |
| UI-03 | ui_toggle_on.wav | ~0.2s | 机械开关感 | Mixkit switch tap | 开关两个版本 | 待处理 |
| UI-04 | ui_toggle_off.wav | ~0.2s | 机械开关感 | 同上 | 关闭版本 | 待处理 |
| UI-05 | ui_notification_info.wav | ~0.5s | 清爽 | Zapsplat / Pixabay | 正常通知 | 待处理 |
| UI-06 | ui_notification_warning.wav | ~0.5s | 轻微不协和 | 上下文 | 警告感 | 待处理 |
| UI-07 | ui_notification_critical.wav | ~0.8s | 低频短脉冲 | 低频击打类 | 与 anomaly 风格统一 | 待处理 |
| UI-08 | ui_panel_open.wav | ~0.3s | 老式档案纸张感 | Paper rustle | 翻页感 | 待处理 |
| UI-09 | ui_panel_close.wav | ~0.2s | 短错误声 | 同上 | 关闭感 | 待处理 |
| UI-10 | ui_build_place.wav | ~0.5s | 水泥/砖块落地感 | Concrete impact | 建造感 | 待处理 |
| UI-11 | ui_build_demolish.wav | ~0.5s | 建材碎裂感 | Debris / wood break | 破坏感 | 待处理 |

---

## 4. 异常事件音效任务列表 (sfx/events/)

| 任务ID | 文件名 | 核心技巧 | 推荐来源 | 处理重点 | 难度 | 状态 |
|----------|----------|-------------|-------------|-------------|--------|--------|
| EVT-01 | event_red_girl_laugh.wav | 远距离、时间错位感 | Pixabay children laughing | 重反響 + 降音量 | 中 | 待处理 |
| EVT-02 | event_elevator_descend.wav | 突然静音 + 楼层错误 | Old elevator 多版本 | 多层编辑、突然静音 | 高 | 待处理 |
| EVT-03 | event_barber_mirror_resonance.wav | 高频共振渐变 | **建议 Suno** | 从轻到尖锐的频率渐变 | 很高 | 待处理 |
| EVT-04 | event_market_meat_chopping.wav | 节奏错位 + 金属回响 | Pixabay butcher | 拍子不对的节奏感 | 中 | 待处理 |
| EVT-05 | event_phone_booth_ring.wav | 怀旧铃铃声 + 静电噪 | Mixkit Old telephone ring | 接听后加静电 | 低 | 待处理 |
| EVT-06 | event_well_bubbles.wav | 低频空洞回响 | Deep water bubbles | 增强空洞感 | 低 | 待处理 |
| EVT-07 | event_overpass_footsteps.wav | 回声延迟异常 | Footsteps with long echo | 调整回声时间 | 中 | 待处理 |
| EVT-08 | event_tongzi_floor_creak.wav | 上方定位嘊呑声 | Pixabay wood creak | 空间音效定位在楼上 | 中 | 待处理 |
| EVT-09 | event_anomaly_trigger.wav | 极低频短脉冲 | 低频击打类 | 0.8-1s | 低 | 待处理 |
| EVT-10 | event_truth_reveal.wav | 金属共振 + 短暂静音 | Metal resonance | 短暂静音后淡出 | 低 | 待处理 |

---

## 5. 关键时刻音效任务列表 (sfx/critical/)

| 任务ID | 文件名 | 核心情绪 | 处理建议 | 状态 |
|----------|----------|-------------|-------------|--------|
| CRT-01 | critical_anomaly_detected.wav | 警觉、不对劲 | 极低频脉冲 + Duck | 待处理 |
| CRT-02 | critical_chapter_reveal.wav | 震撼、线索连接 | 长混响金属共振 | 待处理 |
| CRT-03 | critical_ending_symbiosis.wav | 不安但平静 | 不协和和弦转向接近协和 | 待处理 |
| CRT-04 | critical_ending_suppress.wav | 空洞安宁、代价感 | 低弦乐渐弱→ 完全静音→ 日常环境恢复 | 待处理 |
| CRT-05 | critical_ending_escape.wav | 恐畅、遗憾 | 脚步远去 + 环境渐消 | 待处理 |
| CRT-06 | critical_ending_surrender.wav | 绝望、被吞没 | 诡异层突破上限 + 正常环境消失 | 待处理 |

---

## 6. 工具与环境要求

- **Audacity** (免费)：修剪、添加效果、空间定位、正规化
- **ffmpeg**：格式转换、响度标准化
- **Git** + Git LFS

**ffmpeg 示例**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 \
  -af "loudnorm=I=-14:TP=-6:LRA=7" output.wav
```

---

## 7. 质量检查清单

- [ ] 文件名称与文档完全一致
- [ ] 格式为 WAV + 44100Hz + 16bit + Mono
- [ ] 响度达到要求
- [ ] 特殊技巧已实现（距离感、节奏错位、空间定位等）
- [ ] 无杂音、无剪辑痕迹

---

## 8. 提交规范

```bash
git add assets/audio/sfx/
git commit -m "feat(audio): add processed short SFX (UI + Events + Critical)"
git push
```

---

**注意**: 如果某个音效在免费库中难以完美实现特殊效果（如 event_barber_mirror_resonance），可以在处理后注明并建议使用 Suno 补充。

该文档已上传到 GitHub 仓库，可直接交给 Grok Build 或其他代理执行。