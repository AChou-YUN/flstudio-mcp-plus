# FL Studio MCP 方案对比

> 基于调研的全面对比，指导 flstudio-mcp-plus 的技术选型

## 可用方案总览

| 项目 | 核心定位 | 通信方式 | 音符写入层 | 成熟度 |
|------|---------|---------|-----------|--------|
| kaupau/flai-mcp | 全功能控制 | MIDI SysEx | Step Sequencer | Alpha，文档完善 |
| karl-andres/fl-studio-mcp | Piano Roll 作曲 | 文件队列 + MIDI 触发 | Piano Roll | 较新，文档少 |

## 功能覆盖对比

| 功能 | flai-mcp | fl-studio-mcp | 说明 |
|------|----------|---------------|------|
| 播放/停止/录音 | ✅ | ✅ | |
| 调速/定位 | ✅ | ❓ | |
| Mixer 音量/声像/静音/独奏 | ✅ | ❓ | |
| Mixer 命名/颜色 | ✅ | ❓ | |
| Channel Rack 音量/声像/音高 | ✅ | ❓ | |
| Channel Rack Mixer 路由 | ✅ | ❓ | |
| Pattern 列表/重命名/改色/克隆 | ✅ | ❓ | |
| 音符写入 | ✅ (Step Sequencer) | ✅ (Piano Roll) | 精度不同 |
| 插件参数控制 | ✅ | ❓ | |
| 插件预设切换 | ✅ | ❓ | |
| 时间线标记 | ✅ | ❓ | |
| Playlist clip 放置 | ❌ | ❌ | FL API 不支持 |
| 导出/渲染 | ❌ | ❌ | FL API 不支持 |
| Windows 支持 | ✅ (loopMIDI) | ✅ (loopMIDI) | |
| macOS 支持 | ✅ (IAC Driver) | ✅ (IAC Driver) | |

## 操作层面对应关系

| 用户日常操作 | 对应 FL API 层 | 对应方案 |
|-------------|---------------|---------|
| Piano Roll 里画音符 | `flpianoroll.score.addNote()` | fl-studio-mcp |
| Step Sequencer 点格子 | `channels.setGridBit()` | flai-mcp |
| 拖 Mixer 推子 | `mixer.setTrackVolume()` | flai-mcp |
| Channel Rack 调音量 | `channels.setChannelVolume()` | flai-mcp |

## 适用场景对比

### flai-mcp 更适合

- **鼓组编写** — Step Sequencer 天然适合鼓机思维
- **Mixer 控制** — 音量/声像/静音/独奏/路由全支持
- **工程管理** — 命名/改色/克隆 Pattern
- **音色设计** — 插件参数调用、预设切换
- **Transport 控制** — 播放/停止/调速/定位

### fl-studio-mcp 更适合

- **旋律编写** — Piano Roll API 精度更高，支持连音、滑音
- **和弦编写** — 完整的音符属性（音高、力度、起止时间）
- **Bassline** — 亚音符级时值控制
- **用户习惯** — 贴近鼠标画音符的操作方式

## 痛点分析

### 共有痛点（FL Studio API 限制）

1. **无法在时间线上排列 Pattern** — 最大痛点，编曲核心操作
2. **无法操作第三方插件 UI** — 混音/音色插件没有脚本 API
3. **无法导出音频** — 最终渲染需手动
4. **无法"听"** — AI 盲操作，不能根据听感调整
5. **依赖虚拟 MIDI** — 需额外安装 loopMIDI / IAC Driver
6. **必须本地运行** — 不支持远程

### flai-mcp 独有痛点

1. **旋律精度不够** — Step Sequencer 写旋律不直观
2. **macOS Piano Roll 文件 I/O 坏了** — FL Studio 嵌入式 Python 的 bug

### fl-studio-mcp 独有痛点

1. **~2 秒延迟** — 文件触发机制的固有延迟
2. **功能覆盖窄** — Mixer/Channel/Plugin 等未确认
3. **文档少** — 上手门槛高

## flstudio-mcp-plus 的差异化方向

### 短期（先部署体验）

1. **部署 flai-mcp** — 体验全功能控制，掌握鼓组 + Mixer
2. **部署 fl-studio-mcp** — 体验 Piano Roll 写旋律
3. **记录使用痛点** — 为后续开发积累需求

### 中期（差异化开发）

4. **融合双方案** — SysEx 做控制通道 + Piano Roll 脚本做音符通道
5. **突破 Playlist 限制** — 探索 PyFLP 操作 .flp 文件
6. **中文自然语言支持** — 理解中文音乐术语

### 长期（杀手级功能）

7. **导出分轨** — 绕过 API 限制实现自动导出
8. **听觉反馈** — 接入音频分析模型
9. **风格模板** — 预设 progressive house / future bass 等风格的编曲模板
