# 剩余短音效详细处理任务清单 (Detailed Handoff) - COMPLETED UI + EVENTS

> **版本**: v3.0 | **Grok Build 完成版** | **日期**: 2026-05 (handoff complete)
> **范围**: 全部 11 个 UI 音效 + 全部 10 个 Events 音效（按用户优先级处理）
> **状态**: 所有 21 项已完成来源策展 + 精确处理步骤编写。实际 WAV 文件需按指南本地下载/生成/处理后添加。
> **Critical 音效 (CRT-01~06)**: 本次未覆盖，留待后续（用户优先级未包含）。

---

## 总体处理流程（严格执行）

1. 访问推荐页面 → 预览并下载最匹配的原始文件（MP3/WAV 均可）
2. **Audacity** (免费) 打开，按每个任务的「Audacity 处理」精确步骤操作
3. 使用下方 **ffmpeg 强制转换** 命令输出标准格式
4. 放入 `assets/audio/sfx/ui/` 或 `assets/audio/sfx/events/` （需先创建目录结构）
5. 验证规格后提交（git + LFS 自动处理 wav）

**标准 ffmpeg 命令**（每个任务末尾都使用此模板）:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" final.wav
mv final.wav [target-filename].wav
```

**质量强制要求**:
- 44100 Hz, 16-bit, Mono, 峰值约 -6 dBFS
- 无裁剪失真、无明显噪声
- 特殊效果（距离感、节奏错位、空间定位、Duck）必须实现

---

## UI 音效 (sfx/ui/) - 11 项全部完成

### UI-01: ui_button_click.wav (~0.08~0.12s)
**核心要求**: 清脆、老式打字机/机械感（非现代 synth）
**推荐来源** (Pixabay, 优先选单次机械击键):
- https://pixabay.com/sound-effects/search/typewriter/ → "Typewriter Click with Bell" 或单键 clack (推荐 Old Typewriter Sound)
- 备选: https://pixabay.com/sound-effects/search/mechanical-keyboard/ 选最短的 "single key" 或 "MX Blue click" 短样本
- 或 Cash Register key click (短版)
**Audacity 处理**:
1. 导入后修剪到精确 0.10s 左右（保留 attack + 短 decay）
2. 轻微高频 boost (+2~3dB @ 4-6kHz) 增强清脆
3. 正规化到 -6dB peak
4. 可选: 极轻的 room tone 模拟老式打字机
**ffmpeg**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" ui_button_click.wav
```
**状态**: ✅ 来源+步骤已就绪 | 推荐文件: typewriter single clack 类

---

### UI-02: ui_button_hover.wav (~0.03~0.06s)
**核心要求**: 极短高频 tick / 软触感（hover 反馈）
**推荐来源**:
- Pixabay "ui hover" / "interface tick" / "soft click high" 或 Mixkit interface 分类中极短的 positive tone
- 搜索: https://pixabay.com/sound-effects/search/ui%20tick/ 或 "high frequency click short"
**Audacity 处理**:
1. 修剪到 <0.05s（只保留初始 transient）
2. 高通滤波 (HPF 800Hz+) 去除低频
3. 轻微压缩 + 正规化 -8dB（hover 音量应低于 click）
**ffmpeg**: 同标准模板 → `ui_button_hover.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-03: ui_toggle_on.wav (~0.15~0.25s)
**核心要求**: 机械开关接通感（轻 "ka" 声）
**推荐来源** (最佳匹配):
- Mixkit: https://mixkit.co/free-sound-effects/tap/ 或 interface → "On or off light switch tap" (极佳)
- Pixabay "light switch" / "old switch" / "toggle switch"
  例: https://pixabay.com/sound-effects/search/light-switch/
**Audacity 处理**:
1. 修剪保留完整开关动作 (~0.2s)
2. 轻加低频 body (EQ +2dB @ 150Hz)
3. 正规化 -6dB
**ffmpeg** → `ui_toggle_on.wav`
**状态**: ✅ 来源+步骤已就绪 (Mixkit 开关音最佳)

---

### UI-04: ui_toggle_off.wav (~0.15~0.25s)
**核心要求**: 机械开关断开感（稍重或不同质感）
**推荐来源**: 同 UI-03，使用同一开关音的不同部分，或找 "switch off" 更重的版本
**Audacity 处理**:
- 与 UI-03 类似，但 decay 稍长或多一点金属感
- 可从同一文件裁不同片段做 on/off 配对
**ffmpeg** → `ui_toggle_off.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-05: ui_notification_info.wav (~0.4~0.6s)
**核心要求**: 清爽、正面、信息类提示（干净 ding/chime）
**推荐来源**:
- Pixabay search "notification" / "chime" / "positive ding" / "info alert"
- Mixkit interface tones
**Audacity 处理**:
1. 修剪到 ~0.5s
2. 轻加短 reverb (0.3s decay) 增加空间
3. 正规化，保持明亮不刺耳
**ffmpeg** → `ui_notification_info.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-06: ui_notification_warning.wav (~0.4~0.6s)
**核心要求**: 轻微不协和 / 警示感（比 info 更紧张）
**推荐来源**: 同上，选带轻微 dissonance 或 lower pitch 的版本
**Audacity 处理**:
1. 轻微 pitch shift -2~3 semitones 或叠加轻 dissonant layer
2. 增加一点 grit (轻 distortion 或 noise floor)
3. 保持短促
**ffmpeg** → `ui_notification_warning.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-07: ui_notification_critical.wav (~0.7~0.9s)
**核心要求**: 低频短脉冲 + 与 anomaly 风格统一（警觉、危险）
**推荐来源**:
- Pixabay "deep impact short" / "sub bass hit" / "low frequency pulse" / "heartbeat low"
- 搜索 concrete/metal low thud 短样本
**Audacity 处理**:
1. 强调 80-200Hz 低频（低切 60Hz 以下避免 rumble）
2. 快速 attack + 短 sustain
3. 与 CRT-01 风格对齐（可稍后统一）
**ffmpeg** → `ui_notification_critical.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-08: ui_panel_open.wav (~0.25~0.35s)
**核心要求**: 老式档案纸张翻动感（自然、轻微摩擦）
**推荐来源** (最佳):
- Pixabay paper rustling / page turn:
  - https://pixabay.com/sound-effects/film-special-effects-paper-rustling-236733/ (Soul_Serenity_Sounds, 流行)
  - https://pixabay.com/sound-effects/rustling-paper-46380/
  - Page turn 短版: https://pixabay.com/sound-effects/search/page%20turn/
**Audacity 处理**:
1. 裁剪单个干净的 "翻开" 动作 (~0.3s)
2. 保留自然纸张摩擦质感
3. 可轻加低频混响 (增加 "档案室" 空间感)
4. 轻微降噪如果有 hiss
**ffmpeg** → `ui_panel_open.wav`
**状态**: ✅ 来源+步骤已就绪 (纸张音首选)

---

### UI-09: ui_panel_close.wav (~0.15~0.25s)
**核心要求**: 短促关闭/合上感（可带轻微 "错误" 或沉闷关闭）
**推荐来源**: 同纸张搜索，选 "close book" / "folder close" 或用同一 rustle 文件反向/裁剪结尾
**Audacity 处理**:
1. 找或制造短促 "合上" 质感（可叠加轻 click）
2. 比 open 更短、更 dull
**ffmpeg** → `ui_panel_close.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-10: ui_build_place.wav (~0.4~0.6s)
**核心要求**: 水泥/砖块落地/放置感（建造放置音）
**推荐来源** (优秀匹配):
- https://pixabay.com/sound-effects/brick-dropped-on-other-bricks-14722/
- https://pixabay.com/sound-effects/cinder-block-thud-in-grass-20192/ (去掉 grass 感)
- Pixabay "concrete" / "brick" / "cinder block" impact
**Audacity 处理**:
1. 裁剪单个干净重击 (~0.5s)
2. 轻加 debris 尾音 (可选叠加小碎屑)
3. 增强低频 body，模拟水泥重量
**ffmpeg** → `ui_build_place.wav`
**状态**: ✅ 来源+步骤已就绪

---

### UI-11: ui_build_demolish.wav (~0.4~0.6s)
**核心要求**: 建材碎裂/破坏感（瓦砾、木头碎裂）
**推荐来源**:
- Pixabay "debris" / "wood break" / "rubble" / "demolition small"
- "bricks fall" 短版或 "wood crash"
**Audacity 处理**:
1. 选择有碎裂质感的版本
2. 增加高频碎屑层 (可叠加轻 glass/wood debris)
3. 快速 decay + 一些低频 rumble
**ffmpeg** → `ui_build_demolish.wav`
**状态**: ✅ 来源+步骤已就绪

**UI 全部 11 项总结**: 全部有明确 Pixabay/Mixkit 来源 + Audacity 精确步骤 + ffmpeg 命令。优先使用机械/纸张/水泥/砖块等物理质感音源，匹配《诡街》复古都市恐怖氛围。

---

## 异常事件音效 (sfx/events/) - 10 项全部完成

### EVT-01: event_red_girl_laugh.wav (4~8s) - 简单项
**核心技巧**: 远距离、时间错位感 + 重反響
**推荐来源**:
- https://pixabay.com/sound-effects/kids-voices-with-natural-reverb-7094/ (极佳，带自然 reverb)
- https://pixabay.com/sound-effects/people-kids-yelling-playing-distant-children-outside-manitoulin-island-summer-05-23438/ (distant)
- Pixabay "children laughing" 搜索，优先 outdoor/distant/reverb 版本
**Audacity 处理** (关键):
1. 修剪到 5~7 秒（选连贯笑声段）
2. **重度 Reverb** (Room size 大，Decay 3~5s，Wet 40-60%) 制造远距离大厅感
3. 低通滤波 (LPF ~3-4kHz) 去除近距离清晰度
4. 整体降低音量 6~9dB（背景化）
5. 可选: 轻加低频滚动 (EQ +3dB 100-200Hz)
6. Export 后检查是否产生"时间错位"诡异感
**ffmpeg**:
```bash
ffmpeg -i input.wav -ar 44100 -ac 1 -sample_fmt s16 -af "loudnorm=I=-14:TP=-6:LRA=7" event_red_girl_laugh.wav
```
**状态**: ✅ 来源+详细处理步骤已就绪

---

### EVT-05: event_phone_booth_ring.wav (5~8s) - 简单项
**核心技巧**: 怀旧铃声 + 接听后静电噪 + 响度衰减
**推荐来源** (完美匹配):
- Mixkit (首选): https://mixkit.co/free-sound-effects/phone-ring/ → "Old telephone ring" 或 "Vintage telephone ringtone" (0:07~0:08)
- Pixabay 备选: https://pixabay.com/sound-effects/rotary-phone-ring-medium-103869/ 或 "Telephone Ringing Old Vintage"
**Audacity 处理**:
1. 修剪到 6~7s（包含 2-3 轮完整铃声）
2. 在 "接听" 时间点 (约 4~5s 处) 后：
   - 降低整体响度 8-12dB
   - 添加低频静电噪声 (Generate → Noise, Brown/Pink, 低 amp -30~-40dB, 短段)
   - 可加轻 click 模拟摘机
3. 保留老式机械铃声的金属质感
**ffmpeg**: 标准 → `event_phone_booth_ring.wav`
**状态**: ✅ 来源+处理步骤已就绪 (Mixkit 老电话铃最佳)

---

### EVT-06: event_well_bubbles.wav (~3~6s) - 简单项
**核心技巧**: 低频空洞回响 (井底/深水气泡)
**推荐来源**:
- Pixabay "water bubbles" / "underwater bubbles" / "deep water" / "boiling" / "cave drip"
- 搜索: https://pixabay.com/sound-effects/search/bubbles/ + "underwater"
**Audacity 处理**:
1. 选择低频丰富、有空洞感的版本
2. 重度低通 + 低频 boost
3. 添加长尾 Reverb (大空间、长 decay) 模拟井筒回响
4. 轻加随机气泡层 (可选叠加)
**ffmpeg** → `event_well_bubbles.wav`
**状态**: ✅ 来源+步骤已就绪

---

### EVT-09: event_anomaly_trigger.wav (0.8~1.2s) - 简单项
**核心技巧**: 极低频短脉冲 (与 UI-07 critical 风格统一)
**推荐来源**:
- Pixabay "deep thud" / "low frequency impact" / "sub bass short" / "heavy impact low"
- "heartbeat" 低频或 "earthquake rumble short"
**Audacity 处理**:
1. 找或合成 80-150Hz 主频的短脉冲
2. 快速 attack (5-10ms)，短 sustain (0.7s)
3. 低切 40Hz 避免过载
4. 轻加 harmonic 使它 "不自然"
**ffmpeg** → `event_anomaly_trigger.wav`
**状态**: ✅ 来源+步骤已就绪

---

### EVT-10: event_truth_reveal.wav (~2~4s) - 简单项
**核心技巧**: 金属共振 + 短暂静音 + 淡出
**推荐来源**:
- Pixabay "singing bowl" / "metal resonance" / "gong" / "chime hit" / "pipe clang" / "metal pipe"
- 搜索 "metal hit resonance"
**Audacity 处理**:
1. 选择有长自然共振的金属击打
2. 修剪到共振尾
3. 在共振中段制造 0.3~0.5s "突然静音" (用 Envelope 工具拉到 -inf)
4. 静音后极轻淡出残响
5. 可叠加极低环境 tone 增加诡异
**ffmpeg** → `event_truth_reveal.wav`
**状态**: ✅ 来源+步骤已就绪

---

### EVT-02: event_elevator_descend.wav - 高难度项
**核心技巧**: 突然静音 + 楼层错误感 (多层编辑)
**推荐来源**:
- Pixabay "elevator" / "old elevator" / "lift motor" + ding 音
  - https://pixabay.com/sound-effects/search/old-elevator/
  - Elevator door + motor hum + dings
**Suno / Audacity 复合处理** (推荐):
1. 基础层: 老电梯电机低频 hum + 缓慢 descend 感觉 (pitch down 慢慢)
2. 叠加 3-4 个 "ding" (楼层到达音)，但第 3 个 ding 后突然 cut
3. 在 ding 序列中制造 "错层" (e.g. 正常 ding 后出现不该有的低沉音或反向 ding)
4. 突然静音 (0.8s 完全无声) 后只剩极轻的错误 hum 或空气声
5. 可在静音前后加极短的 "glitch" 切音
**Audacity**: 多轨叠加 + Envelope 自动化实现突然 cut
**ffmpeg** (最终 mono 标准化) → `event_elevator_descend.wav`
**状态**: ✅ 详细多层编辑方案 + 来源已就绪 (建议 Suno 辅助电机+ding 层)

---

### EVT-03: event_barber_mirror_resonance.wav - 最高难度项
**核心技巧**: 高频玻璃共振渐变 (从轻微 hum → 尖锐 ringing) + 心理紧张
**强烈建议**: **直接使用 Suno 生成** (免费库几乎无完美匹配的渐变心理音)
**Suno Prompt (v3.5+ 推荐)**:
```
High frequency glass resonance building gradually from subtle low hum to sharp piercing ringing tone, slow frequency sweep upward, psychological tension and dread, subtle harmonic overtones, horror atmosphere, seamless one-shot or short loop, no melody, pure sound design, 8-12 seconds
```
**Negative prompt**: music, melody, voice, drum, beat
**后处理 (Suno 输出后)**:
1. 导入 Audacity
2. 轻微 EQ 强调刺耳高频 (8-12kHz)
3. 增加极轻 reverb 制造镜子房间感
4. 结尾可淡出或 abrupt cut
5. ffmpeg 标准化
**状态**: ✅ Suno Prompt + 后处理流程已就绪 (强烈推荐此路径)

---

### EVT-04: event_market_meat_chopping.wav - 高难度项
**核心技巧**: 节奏错位 + 金属回响 (菜市场剁肉但节拍不对)
**推荐来源**:
- Pixabay: https://pixabay.com/sound-effects/film-special-effects-knife-and-chopping-board-306657/
- https://pixabay.com/sound-effects/search/chopping%20meat/ 或 "cleaver"
**Audacity 处理 (节奏错位是关键)**:
1. 导入正常有节奏的剁肉声
2. 使用 "Change Tempo" 或手动剪切/移动某些击打，使节奏逐渐错位 (e.g. 正常 4/4 → 逐渐拉长间隔或插入早击)
3. 在部分重击上叠加轻金属共鸣层 (用 EQ 或另一个 "metal hit" 低混)
4. 增加一些湿肉声或骨头碎裂层 (可选)
5. 整体制造 "不对劲的日常" 感觉
**ffmpeg** → `event_market_meat_chopping.wav`
**状态**: ✅ 来源 + 节奏错位编辑方法已就绪

---

### EVT-07: event_overpass_footsteps.wav - 高难度项
**核心技巧**: 回声延迟异常 (echo timing 不对)
**推荐来源**:
- https://pixabay.com/sound-effects/footsteps-in-a-hallway-47842/
- https://pixabay.com/sound-effects/footsteps-hallway-6417/
- https://pixabay.com/sound-effects/echoing-footsteps-377259/
- Tunnel / concrete reverb footsteps
**Audacity 处理**:
1. 选择干净的 echoing concrete footsteps
2. **关键异常处理**:
   - 复制脚步轨
   - 将 echo 轨的 delay 时间故意设错 (e.g. 正常 0.4s echo → 改成 0.75s 或 1.1s 不匹配空间)
   - 或在 echo 衰减中插入 "提前" 的脚步声 (时间错位)
3. 轻加风声或低频 overpass 环境
4. 整体保持中距离感
**状态**: ✅ 来源 + 异常 echo 编辑方案已就绪

---

### EVT-08: event_tongzi_floor_creak.wav - 高难度项
**核心技巧**: 上方定位嘎吱声 (声音来自天花板/楼上)
**推荐来源**:
- https://pixabay.com/sound-effects/wood-creak-single-v3-14657/ (freesound_community, 极佳)
- Pixabay "floor creak" / "wooden floorboard" / "creaking wood" 搜索
**Audacity 处理 (空间定位关键)**:
1. 导入真实木地板嘎吱
2. **空间处理**:
   - 使用 Audacity "Pan" 工具或 "Stereo Widener" 使声音定位在上方
   - 或导出为 stereo，左/右声道做不同 delay + 轻高频 rolloff 模拟 ceiling reflection
   - 增加长 reverb tail 模拟楼板传声
3. 轻加低频 "楼板振动" 层
4. 最终转 mono 时保留 "从上方来" 的感觉 (可保留轻微 stereo 信息但主观 mono 兼容)
**ffmpeg** (注意: 最终仍是 mono，但处理过程可用 stereo 辅助定位) → `event_tongzi_floor_creak.wav`
**状态**: ✅ 来源 + 楼上空间定位处理方案已就绪

---

## Critical 音效 (简要，供参考)

CRT-01 ~ CRT-06 留待下次处理。建议部分用 Suno (尤其是 ending 系列)，CRT-01 重点做极低频脉冲 + 环境 Duck 效果 (Audacity Envelope 自动化)。

---

## 目录结构建议 (创建后提交)

```
assets/audio/sfx/
├── ui/
│   ├── ui_button_click.wav
│   ├── ui_button_hover.wav
│   ├── ... (共 11)
├── events/
│   ├── event_red_girl_laugh.wav
│   ├── ... (共 10)
└── critical/   (未来)
```

---

## 提交规范

```bash
git checkout -b feat/complete-sfx-ui-events-handoff
git add assets/audio/sfx/ REMAINING_SFX_TASKS*.md SFX_Processing_Progress_Tracker.md
git commit -m "feat(audio): complete UI (11) + Events (10) short SFX handoff - detailed sources & processing guides added"
git push origin feat/complete-sfx-ui-events-handoff
```

---

**完成说明**:
- 本文档 v3.0 已将用户指定的 21 项全部细化到可直接执行的程度（来源直达链接 + Audacity 步骤 + ffmpeg）。
- 实际 WAV 文件因工具限制未在此环境二进制生成，需执行者按指南在本地 Audacity + ffmpeg 完成最终处理。
- 所有推荐来源均为 Pixabay (CC0-like) 或 Mixkit (完全免费商用无署名)。
- 已同步更新 Progress Tracker 和主任务列表状态。

**下一步行动**: 执行者下载本指南中链接 → Audacity 逐个处理 → ffmpeg 转码 → 放入目录 → 提交 PR 或 push。

Grok Build 任务交接完成。