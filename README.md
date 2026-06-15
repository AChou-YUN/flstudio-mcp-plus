# 🎹 FL Studio MCP Plus

AI 驱动的 FL Studio 全功能控制，基于 MCP（Model Context Protocol）协议。

## 项目定位

在现有 FL Studio MCP 项目基础上，实现**全功能覆盖**和**差异化创新**。

### 核心目标

- ✅ 完整的 FL Studio 控制（Transport、Mixer、Channel Rack、Plugin、Piano Roll）
- ✅ 自然语言交互，零手动干预
- 🔧 突破现有限制：插件加载、Pattern 创建
- 🔧 中文支持

### 差异化方向

| 现有项目限制 | MCP Plus 突破方向 |
|-------------|-----------------|
| 不能加载插件 | 探索替代方案（快捷键脚本/文件监控） |
| 不能创建 Pattern | 探索 FL Studio 内部 API/hack |
| 仅英文 | 中文自然语言支持 |
| 功能分散 | 统一的全功能 MCP Server |

## 目录结构

```
flstudio-mcp-plus/
├── README.md                ← 项目总览
├── docs/                    ← 调研与文档
│   ├── research/            ← 竞品调研、技术调研
│   ├── requirements/        ← 需求文档
│   └── architecture/        ← 架构设计
├── planning/                ← 规划与进度
│   ├── roadmap.md           ← 产品路线图
│   ├── milestones.md        ← 里程碑
│   └── tasks/               ← 任务追踪
├── src/                     ← 源码
│   ├── server/              ← MCP Server 核心
│   ├── bridge/              ← FL Studio Bridge 脚本
│   ├── tools/               ← MCP 工具实现
│   └── utils/               ← 工具函数
├── scripts/                 ← 安装/配置脚本
└── examples/                ← 使用示例
```

## 技术栈

- **语言：** Python 3.10+
- **MCP 框架：** FastMCP
- **MIDI：** mido
- **包管理：** uv

## 状态

🚧 规划中

---

*由 OpenClaw AI 管理*
