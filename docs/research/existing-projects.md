# 现有 FL Studio MCP 项目调研

> 调研日期：2026-06-18
> 调研方式：mimo search + Web Search + GitHub + MCPWorld + Bilibili

## 调研渠道

| 渠道 | 用途 |
|------|------|
| mimo search 联网搜索 | 中文生态、官方功能、第三方工具 |
| GitHub 搜索 | 核心开源项目 |
| MCPWorld.com / mcpmarket.com / aibase.com | MCP Server 导航站 |
| Bilibili | 中文教程视频 |
| FL Studio 中文官网 | 官方 AI 功能和插件 |

## MCP 开源项目

### 项目一：kaupau/flai-mcp ⭐

- **GitHub:** <https://github.com/kaupau/flai-mcp>
- **许可证:** MIT | **状态:** Alpha，活跃开发

**核心机制：** MIDI SysEx 实时双向通信
- 请求：`[F0, 7D, CMD_ID, REQ_ID, <ascii-json>, F7]`
- 响应：`[F0, 7D, 70, REQ_ID, STATUS, <ascii-json>, F7]`
- 通过 REQ_ID 匹配请求与响应，异步 async 模式

**功能：** Transport / Mixer / Channel Rack / Pattern / Step Sequencer 音符 / 插件参数 / 时间线标记

**关键局限：**
- 音符走 Step Sequencer（1/16 量化），写旋律不直观
- 无法排列 Playlist clip（API 不支持）
- macOS Piano Roll 脚本文件 I/O 坏了

### 项目二：karl-andres/fl-studio-mcp

- **GitHub:** <https://github.com/karl-andres/fl-studio-mcp>
- **状态:** 较新，文档偏少

**核心机制：** 文件队列 + MIDI 触发
- MCP Server 写指令到磁盘文件
- MIDI 发触发信号（"门铃"）
- FL Studio Piano Roll 脚本读取文件 → 调用 flpianoroll API
- ~2 秒延迟

**功能：** Piano Roll 精细音符写入 / 基础 Transport

**关键局限：** 功能覆盖窄、延迟、基本单向

### MCP 生态收录

| 平台 | 说明 |
|------|------|
| MCPWorld.com | 收录了 FL Studio MCP Server（两个） |
| mcpmarket.com | 收录 FL Studio MCP，描述为"通过 MCP 协议让 AI 无缝控制 FL Studio" |
| aibase.com | 收录，描述为"连接 Claude 与 FL Studio" |

## FL Studio 官方 AI 功能

| 功能 | 版本 | 能力 | 本质 |
|------|------|------|------|
| **Loop Starter** | 2025+ | 按风格生成循环音轨堆栈，一键排列 | 层次 1：素材生成 |
| **Gopher** | 2025+ | AI 问答助手，多语言，基于官方知识库 | 层次 2：问答辅助 |
| **AI 分轨** | 21.2.2+ | 导入采样智能分轨，可路由 Mixer | 单向音频分析 |
| **和弦进行工具** | 2025.1+ | 新增低音线模式 | 辅助工具 |

**结论：** 官方 AI 定位是"辅助灵感"和"降低门槛"，不做"Agent 操控 DAW"。

## 第三方 AI 工具

| 工具 | 类型 | 与 FL Studio 的关系 | 评价 |
|------|------|-------------------|------|
| **ORB Producer Suite** | VST 插件（¥199） | FL 内运行，AI 生成和弦/旋律/Bass/琶音 | 最接近"AI 编曲"的成熟产品 |
| **Pack Generator** | 采样生成器 | 下载后导入 FL | 文字→采样包，质量不错 |
| **SoundRaw** | Web 工具 | 导出音频后导入 FL | 分段落可调，日本团队 |
| **Suno** | 端到端平台 | 导出后可导入 FL 二次编辑 | 最成熟但最不可控 |
| **Riffusion** | 免费开源 | 导出后导入 FL | 文生图→图转音乐，效果一般 |

## 共性技术约束

所有方案共同面临的 FL Studio API 限制：

1. **Playlist clip 操作不存在** — Image-Line 未暴露相关 API
2. **第三方插件 UI 无法脚本化** — 只能操作有参数暴露的插件
3. **导出/渲染无 API** — 需要手动操作
4. **依赖虚拟 MIDI 端口** — Windows 需 loopMIDI，macOS 需 IAC Driver
5. **必须本地运行** — 不能远程控制
