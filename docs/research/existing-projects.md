# 现有 FL Studio MCP 项目调研

> 调研日期：2026-06-18
> 调研方式：Web Search + GitHub + MCPWorld + Bilibili

## 调研渠道

| 渠道 | 用途 |
|------|------|
| GitHub 搜索 | 找到核心开源项目 |
| MCPWorld.com | MCP Server 导航站，收录了多个 FL Studio 相关 MCP |
| Bilibili | 中文教程视频，有实际演示 |
| CSDN/博客 | MCP + Agent 技术文章 |

## 项目一：kaupau/flai-mcp ⭐

- **GitHub:** <https://github.com/kaupau/flai-mcp>
- **Star 数:** 未确认（Alpha 阶段）
- **许可证:** MIT
- **状态:** Alpha，功能核心可用，活跃开发

### 架构

```
Claude Code / Claude Desktop / Cursor
 │ MCP (stdio)
 ▼
flai-mcp server (Python)
 │
 Virtual MIDI (SysEx)
 (IAC Driver / loopMIDI)
 │
 device_flai.py
 (MIDI Controller Script
 running inside FL Studio)
```

### 信号原理

使用 **MIDI SysEx 协议**进行实时双向通信：

- **请求格式:** `[0xF0, 0x7D, CMD_ID, REQ_ID, <ascii-json>, 0xF7]`
- **响应格式:** `[0xF0, 0x7D, 0x70, REQ_ID, STATUS, <ascii-json>, 0xF7]`
- `0x7D` — MIDI 规范保留给非商业/教育用途的厂商 ID
- `REQ_ID` — 0-127 轮转计数器，用于匹配请求和响应
- JSON 使用 `ensure_ascii=True` 编码，确保所有字节在 0x00-0x7F（SysEx 安全范围）
- 通信模式：异步 async，请求-响应匹配

### 功能覆盖

| 类别 | 能力 |
|------|------|
| Transport | 播放、停止、录音、调速、定位到 bar/beat |
| Mixer | 音量、声像、静音、独奏、命名、颜色（任意 track） |
| Channel Rack | 音量、声像、音高、命名、静音、Mixer 路由 |
| Patterns | 列表、重命名、改色、克隆、切换 |
| Notes | 通过 Step Sequencer API 写入音符（setGridBit + setStepParameterByIndex） |
| Plugins | 列出参数、get/set 值、切换预设 |
| Arrangement | 添加/列出时间线标记、获取 playhead 位置 |
| Playlist | 命名和改色 playlist tracks（不能放置 clip） |

### 已知局限

1. **无法在时间线上放置 Pattern clip** — FL Studio 脚本 API 不暴露 playlist 操作
2. **音符写入走 Step Sequencer** — 1/16 音符量化，支持微时序 shift，但不如 Piano Roll 灵活
3. **macOS Piano Roll 脚本无法做文件 I/O** — FL Studio 嵌入式 Python 的 open() 在 macOS 上坏了
4. **调速需要 FL Studio 21+** — 旧版本需手动操作

### 安装要点

- Windows: loopMIDI（免费虚拟 MIDI 端口）
- macOS: IAC Driver（系统内置）
- Python 3.11+
- 需要手动拷贝 `device_flai.py` 到 FL Studio Hardware 目录

---

## 项目二：karl-andres/fl-studio-mcp

- **GitHub:** <https://github.com/karl-andres/fl-studio-mcp>
- **状态:** 较新，文档偏少

### 架构

```
AI Agent
 │ MCP (stdio)
 ▼
fl-studio-mcp server (Python)
 │ 写入命令到队列文件
 │ 发送 MIDI 触发信号
 ▼
虚拟 MIDI 端口
 │ 触发消息（仅作通知）
 ▼
composewithllm.pyscript
 (FL Studio Piano Roll 脚本)
 │ 读取队列文件
 │ 调用 flpianoroll API
 ▼
FL Studio Piano Roll 更新
```

### 信号原理

使用 **文件队列 + MIDI 触发**的间接通信模式：

1. MCP Server 把 AI 的作曲指令写到磁盘队列文件
2. 通过 MIDI 发一个触发信号给 FL Studio（仅作"门铃"用）
3. FL Studio 收到触发后运行 `composewithllm.pyscript` 脚本
4. 脚本从文件读取队列 → 解析 → 调用 `flpianoroll.score.addNote()` 写入音符
5. 存在约 2 秒延迟（触发 → 脚本执行 → 音符出现）

### 功能覆盖

| 类别 | 能力 |
|------|------|
| Piano Roll 音符写入 | ✅ 完整的 flpianoroll API（音高、力度、时值、滑音等） |
| Transport | ✅ 基础控制 |
| 安装 | 一键 `./install.sh` |

### 已知局限

1. 功能覆盖面不如 flai-mcp（Mixer/Channel Rack/Plugin 等未确认）
2. 约 2 秒延迟
3. 基本单向通信（AI → FL Studio 为主）
4. 文档不完整

---

## 项目三：Bilibili 教程方案（hajimi8）

- **视频:** <https://www.bilibili.com/video/BV1c2Ei6pEfW>
- **指令网站:** FLStudio.hajimi8.ggff.net
- **面向:** 小白用户，十分钟上手
- **本质:** 应该是基于上述项目之一的封装/教程

---

## 项目四：leehave/Claude-Music-Mcp（不相关）

- **GitHub:** <https://github.com/leehave/Claude-Music-Mcp>
- **说明:** 音乐库管理 MCP（搜索/播放列表/推荐），**不涉及 FL Studio 控制**，排除。

---

## 共性技术约束

所有方案共同面临的 FL Studio API 限制：

1. **Playlist clip 操作不存在** — Image-Line 未暴露相关 API
2. **第三方插件 UI 无法脚本化** — 只能操作有参数暴露的插件
3. **导出/渲染无 API** — 需要手动操作
4. **依赖虚拟 MIDI 端口** — Windows 需 loopMIDI，macOS 需 IAC Driver
5. **必须本地运行** — 不能远程控制
