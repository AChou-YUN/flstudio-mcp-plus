# 🎹 FL Studio MCP Plus

AI 驱动的 FL Studio 全功能控制，基于 MCP（Model Context Protocol）协议。

## 项目定位

在现有 FL Studio MCP 项目基础上，实现**全功能覆盖**和**差异化创新**。

### 核心目标

- ✅ 完整的 FL Studio 控制（Transport、Mixer、Channel Rack、Plugin、Piano Roll）
- ✅ 自然语言交互，零手动干预
- 🔧 突破现有限制：Playlist 排列、自动导出
- 🔧 中文支持

### 差异化方向

| 现有项目限制 | MCP Plus 突破方向 |
|-------------|-----------------|
| Step Sequencer 写旋律不直观 | 融合 Piano Roll 脚本方案 |
| 不能在时间线排列 Pattern | 探索 PyFLP 操作 .flp 文件 |
| 不能导出分轨 | 绕过 API 限制实现自动导出 |
| 仅英文 | 中文自然语言支持 |
| 功能分散在两个项目 | 统一的全功能 MCP Server |

## 快速了解

- [现有项目调研](docs/research/existing-projects.md) — flai-mcp、fl-studio-mcp 详细分析
- [信号原理对比](docs/research/signal-mechanism.md) — SysEx vs 文件队列机制解析
- [方案对比](docs/research/comparison.md) — 功能覆盖、场景适用、痛点分析
- [场景分析](docs/requirements/use-cases.md) — 制作视角的场景覆盖 + progressive house 示例
- [产品路线图](planning/roadmap.md) — 分阶段开发计划

## 目录结构

```
flstudio-mcp-plus/
├── README.md                ← 项目总览（你在这里）
├── docs/                    ← 调研与文档
│   ├── research/            ← 竞品调研、技术调研
│   │   ├── existing-projects.md    ← 现有项目调研
│   │   ├── signal-mechanism.md     ← 信号原理分析
│   │   └── comparison.md           ← 方案对比
│   ├── requirements/        ← 需求文档
│   │   └── use-cases.md            ← 场景分析
│   └── architecture/        ← 架构设计（待填充）
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

- **语言：** Python 3.11+
- **MCP 框架：** FastMCP
- **MIDI：** mido（SysEx 通信）
- **Piano Roll：** flpianoroll 脚本 API
- **包管理：** uv

## 参考项目

- [kaupau/flai-mcp](https://github.com/kaupau/flai-mcp) — SysEx 全功能控制（MIT）
- [karl-andres/fl-studio-mcp](https://github.com/karl-andres/fl-studio-mcp) — Piano Roll 脚本
- [PyFLP](https://github.com/demberto/PyFLP) — .flp 文件解析库
- [FL Studio API Stubs](https://il-group.github.io/FL-Studio-API-Stubs/) — 官方 API 文档

## 状态

🚧 规划中 — Phase 0 调研阶段

---

*由 [AChou-YUN](https://github.com/AChou-YUN) 开发*
