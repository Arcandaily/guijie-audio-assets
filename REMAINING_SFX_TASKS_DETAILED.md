# 剩余短音效详细处理任务清单 (Detailed Handoff)

> **版本**: v2.0 | **为 Grok Build 准备的超详细版本**
> 每个任务包含：精确来源链接 + 具体处理步骤 + ffmpeg 命令

---

## 总体处理流程

1. 访问推荐页面下载原始文件
2. **Audacity** 打开后按下方步骤处理
3. 使用下方 ffmpeg 命令强制转换
4. 放入对应路径后提交

**ffmpeg 基础命令**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" final.wav
mv final.wav [target-filename].wav
```

---

## UI 音效详细任务

### UI-01: ui_button_click.wav
**来源**: https://pixabay.com/sound-effects/search/ui/ 或 https://mixkit.co/free-sound-effects/click/
**选择**: 最清脆的按钮声，避免太音乐化
**Audacity 处理**:
- 修剪到 0.08~0.12s
- 正规化到 -6dB 左右
**ffmpeg**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" ui_button_click.wav
```

### UI-08: ui_panel_open.wav
**来源**: 搜索 "paper rustling" 或 "book page turn" on Pixabay
**Audacity 处理**:
- 保留纸张翻动的自然感
- 可适当增加低频混响
**ffmpeg** 同上

（其他 UI 任务可参照此模式处理，具体可在后续补充）

---

## Events 音效详细任务

### EVT-01: event_red_girl_laugh.wav
**来源**: https://pixabay.com/sound-effects/search/children-laughing/
**选择建议**: 选择带远距离感、空间感的版本（非过于清晰的近距离童声）
**Audacity 处理**:
1. 修剪到4-8秒
2. 添加 Reverb 增强远距离感
3. 整体降低音量 6-9dB
4. 可适当添加低频滚动
**ffmpeg** 同上

### EVT-05: event_phone_booth_ring.wav
**来源**: https://mixkit.co/free-sound-effects/phone-ring/
**选择**: "Old telephone ring" 或 "Vintage telephone ringtone"
**Audacity 处理**:
- 修剪到5-8s
- 接听后部分加低频静电噪声和响度衰减
**ffmpeg** 同上

### EVT-03: event_barber_mirror_resonance.wav
**难度**: 很高
**建议**: 直接使用 Suno 生成。
**Suno Prompt 参考**:
```
High frequency glass resonance building up gradually from subtle hum to sharp ringing tone, slow frequency sweep, psychological tension, seamless loop or one-shot, horror atmosphere
```

（其他事件音效可按照类似方式处理，需要更多细节可请求补充）

---

## Critical 音效

### CRT-01: critical_anomaly_detected.wav
**处理重点**: 极低频脉冲 (0.4-0.6s) + 环境音瞬间衰减 (Duck)
**Audacity**: 使用 Envelope 或自动化编辑实现 Duck 效果

### CRT-02 ~ CRT-06
部分可以使用 Suno 生成对应情绪的音乐段落，再进行后期处理。

---

## 提示

本文档为详细版本，可作为 Grok Build 的主要操作指南。
如需要将此内容合并到 `REMAINING_SFX_TASKS.md` 或生成更完整的手册，请告知我。