# 🏗️ 架构设计

> 最后更新：2026-06-15

## 整体架构

```
┌─────────────────┐     ┌─────────────────────────────────────────┐
│   MCP Client    │────▶│           FastMCP Server                │
│  (OpenClaw等)   │     │                                         │
└─────────────────┘     │  ┌─────────────────┐  ┌──────────────┐  │
                        │  │ MIDI + JSON     │  │ Piano Roll   │  │
                        │  │ Tools           │  │ Tools (JSON) │  │
                        │  └────────┬────────┘  └──────┬───────┘  │
                        └───────────┼──────────────────┼──────────┘
                                    │                  │
                               MIDI + JSON        JSON Files +
                                    │              Keystroke
                                    ▼                  ▼
                        ┌─────────────────────────────────────────┐
                        │              FL Studio                   │
                        │  ┌──────────────┐  ┌──────────────────┐ │
                        │  │FLStudioMCP   │  │ Piano Roll Script│ │
                        │  │(MIDI Ctrl)   │  │ (ComposeWithLLM) │ │
                        │  └──────────────┘  └──────────────────┘ │
                        └─────────────────────────────────────────┘
```

## 通信方案

### Transport/Mixer/Channel/Plugin

1. MCP Server 写命令到 JSON 文件
2. 发送 MIDI trigger note 到 FL Studio
3. FL Studio Controller Script 读取 JSON，执行 API，写入响应
4. MCP Server 读取响应

### Piano Roll

1. MCP Server 写音符请求到 JSON 文件
2. 发送快捷键（Cmd+Opt+Y / Ctrl+Alt+Y）触发 FL Studio 脚本
3. Piano Roll Script 读取 JSON 并修改音符

## 目录结构

```
src/
├── server/              ← MCP Server 入口
│   └── main.py
├── bridge/              ← FL Studio Bridge 脚本
│   ├── device_FLStudioMCP.py    ← MIDI Controller Script
│   └── ComposeWithLLM.pyscript  ← Piano Roll Script
├── tools/               ← MCP 工具实现
│   ├── transport.py     ← Transport 控制
│   ├── mixer.py         ← Mixer 控制
│   ├── channels.py      ← Channel Rack 控制
│   ├── plugins.py       ← Plugin 控制
│   └── piano_roll.py    ← Piano Roll 控制
└── utils/               ← 工具函数
    ├── connection.py    ← 连接管理
    ├── midi.py          ← MIDI 通信
    └── trigger.py       ← 触发机制
```

---

*此文件记录架构设计*
