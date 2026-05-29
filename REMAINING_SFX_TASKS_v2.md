# 剩余短音效处理任务清单 (Merged v2.1)

> **版本**: v2.1 | **状态**: 可直接交给 Grok Build 执行

**格式要求**: WAV, 44100Hz, 16-bit, Mono, 峰值 ~-6dBFS

## 整体流程
1. 下载原始文件
2. Audacity 修剪 + 正规化 + 特殊效果
3. ffmpeg 转换
4. Push to repo

**ffmpeg**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" [name].wav
```

## UI 任务

| ID | 文件名 | 推荐来源 | 处理重点 |
|-----|----------|-------------|-------------|
| UI-01 | ui_button_click.wav | Pixabay UI / Mixkit | 短清脆老式感 |
| UI-08 | ui_panel_open.wav | Paper rustling | 纸张翻动感 |

**UI-01 详细步骤**:
1. 从 Mixkit 或 Pixabay 下载清脆按钮声
2. Audacity 修剪到 0.1s 左右
3. 正规化
4. ffmpeg 转换

## Events 任务

| ID | 文件名 | 推荐来源 | 处理重点 | 难度 |
|-----|----------|-------------|-------------|--------|
| EVT-01 | event_red_girl_laugh.wav | Pixabay children-laughing | 远距离感 + 重反響 | 中 |
| EVT-03 | event_barber_mirror_resonance.wav | **Suno** | 高频共振渐变 | 高 |
| EVT-05 | event_phone_booth_ring.wav | Mixkit Old telephone ring | 怀旧铃铃声 + 静电噪 | 低 |

**EVT-01 详细步骤**:
- 从 Pixabay 选择带远距离感的童声
- Audacity: 修剪 + Reverb + 降音量
- ffmpeg 转换

**EVT-03 建议**:
直接用 Suno，Prompt: High frequency glass resonance building from subtle to sharp, horror tension

## Critical

| ID | 文件名 | 处理建议 |
|-----|----------|-------------|
| CRT-01 | critical_anomaly_detected.wav | 极低频脉冲 + Duck |

## 提交
```bash
git add assets/audio/sfx/
git commit -m "feat: processed short SFX"
git push
```

** 说明**: 此为合并详细版的 v2.1。可用此文件替换原 `REMAINING_SFX_TASKS.md`。