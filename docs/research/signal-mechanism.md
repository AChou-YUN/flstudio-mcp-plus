# 信号原理对比分析

> 分析 flai-mcp 与 fl-studio-mcp 两种方案的核心通信机制差异

## 方案一：flai-mcp — SysEx 实时双向通信

### 数据流

```
AI Agent
  │ MCP (stdio, JSON-RPC)
  ▼
flai-mcp Python Server
  │ 构造 SysEx 消息
  │ [0xF0, 0x7D, CMD_ID, REQ_ID, <ascii-json>, 0xF7]
  ▼
虚拟 MIDI 端口 (loopMIDI / IAC Driver)
  │ MIDI 字节流
  ▼
device_flai.py（FL Studio MIDI Controller Script）
  │ 解析 SysEx → JSON
  │ 调用 FL Studio Scripting API
  │ 打包响应 → SysEx 回传
  ▼
FL Studio 执行操作
```

### 核心协议

```
Request:  [F0, 7D, CMD_ID, REQ_ID, <ascii-json-payload>, F7]
Response: [F0, 7D, 70, REQ_ID, STATUS, <ascii-json-payload>, F7]
```

- `0xF0` — SysEx 起始标志
- `0x7D` — 厂商 ID（非商业/教育用途保留）
- `CMD_ID` — 命令编号
- `REQ_ID` — 0-127 轮转计数器，匹配请求与响应
- `0xF7` — SysEx 结束标志
- JSON 使用 `ensure_ascii=True`，所有字节保证在 0x00-0x7F

### 特点

- **实时性:** MIDI 流式协议，毫秒级到达
- **双向:** 请求-响应模式，异步 async
- **有状态:** REQ_ID 追踪每个请求生命周期
- **带宽:** 标准 MIDI 31.25 KB/s，虚拟端口无此限制

---

## 方案二：fl-studio-mcp — 文件队列 + MIDI 触发

### 数据流

```
AI Agent
  │ MCP (stdio, JSON-RPC)
  ▼
fl-studio-mcp Python Server
  │ 写入命令到队列文件（磁盘）
  │ 发送 MIDI 触发信号
  ▼
虚拟 MIDI 端口 (loopMIDI / IAC Driver)
  │ 触发消息（仅作通知，不携带数据）
  ▼
composewithllm.pyscript（FL Studio Piano Roll 脚本）
  │ 被触发后运行
  │ 读取队列文件
  │ 调用 flpianoroll API 写入音符
  ▼
FL Studio Piano Roll 更新（~2秒延迟）
```

### 核心机制

1. MCP Server 将 AI 指令写入磁盘队列文件
2. 发送 MIDI 触发信号（充当"门铃"）
3. FL Studio 运行 `composewithllm.pyscript`
4. 脚本读取队列 → 解析 → 调用 `flpianoroll.score.addNote()`
5. 音符出现在 Piano Roll（~2秒延迟）

### 特点

- **间接通信:** 数据走文件，MIDI 仅做触发
- **单向为主:** AI → FL Studio，回传状态有限
- **精度更高:** flpianoroll API 支持完整音符属性

---

## 核心差异对比

| 维度 | flai-mcp (SysEx) | fl-studio-mcp (文件队列) |
|------|-------------------|--------------------------|
| 数据通道 | MIDI SysEx 消息本身携带数据 | 数据写磁盘，MIDI 仅触发 |
| 实时性 | 毫秒级 | ~2 秒延迟 |
| 双向通信 | ✅ 请求-响应模式 | ❌ 基本单向 |
| 音符精度 | Step Sequencer（1/16 量化） | Piano Roll API（亚音符级） |
| 复杂度 | 高（SysEx 协议 + 异步桥接） | 低（写文件 + 触发脚本） |
| 可靠性 | 需处理消息丢失/乱序 | 文件系统保证持久化 |
| 扩展性 | 命令枚举易扩展 | 受限于 pyscript 脚本能力 |

## 对 flstudio-mcp-plus 的启示

- **实时控制**（Mixer/Transport/Channel）→ 借鉴 SysEx 方案
- **音符写入**（旋律/和弦/Bass）→ 借鉴 Piano Roll 脚本方案
- **最优解:** 融合两者，SysEx 做控制通道 + Piano Roll 脚本做音符通道
