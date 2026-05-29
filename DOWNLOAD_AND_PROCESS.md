# 剩余音效下载与处理指南

**格式要求**: WAV, 44100Hz, 16-bit, mono, peak ~-6dBFS。

## 操作流程
1. 从下表下载原始文件
2. Audacity 修剪 + 正规化 + 添空间感
3. ffmpeg 转换并强制规格
4. 按名称放入对应路径后 push (已配 LFS)

## UI 音效
- ui_button_click / hover / toggle: Pixabay UI 或 Mixkit Click / switch tap
- ui_panel_open: paper rustle / book page
- ui_build_place/demolish: concrete impact / debris

## Events 音效
- event_red_girl_laugh: Pixabay children laughing (远距离版 + 重反響)
- event_elevator_descend: old elevator 多版本编辑
- event_phone_booth_ring: Mixkit Old telephone ring
- event_market_meat_chopping: Pixabay butcher
- event_tongzi_floor_creak / well_bubbles / overpass_footsteps: Pixabay creak + 空间处理
- event_barber_mirror_resonance: **建议 Suno 生成** (难找渐变共振)

## Critical
- anomaly_detected / chapter_reveal: 低频脉冲 + 金属共振

## 快速 ffmpeg 命令
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" output.wav
```

**难点标记**: event_barber_mirror_resonance 和中国风格声音建议用 Suno。

详细版本可后续补充。