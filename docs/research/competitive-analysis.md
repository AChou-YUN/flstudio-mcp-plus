# 🏆 竞品分析 — FL Studio MCP 生态

> 调研日期：2026-06-15

## 主要竞品

### 1. calvinw/fl-studio-mcp

- **地址：** https://github.com/calvinw/fl-studio-mcp
- **功能范围：** 仅 Piano Roll
- **核心能力：** 和弦、旋律、bass line、音符修改
- **架构：** MCP Server → JSON 队列 → 快捷键触发 → Bridge Script
- **平台：** macOS（完整）/ Windows（部分）
- **优势：** 自动触发，零干预
- **劣势：** 功能单一，仅 Piano Roll

### 2. karl-andres/fl-studio-mcp

- **地址：** https://github.com/karl-andres/fl-studio-mcp
- **功能范围：** 全功能（Transport、Mixer、Channel、Plugin、Piano Roll）
- **核心能力：** 完整的 FL Studio 控制
- **架构：** MCP Server → MIDI + JSON → Controller Script
- **平台：** macOS（IAC Driver）/ Windows（loopMIDI）
- **优势：** 功能最全，架构合理
- **劣势：** 不能加载插件、不能创建 Pattern

### 3. szichedelic/fl-studio-mcp

- **地址：** https://github.com/szichedelic/fl-studio-mcp
- **功能：** 自然语言控制 FL Studio
- **特点：** Claude AI 集成

## 共同限制

| 限制 | 原因 | 可能的突破方向 |
|------|------|--------------|
| 不能加载插件 | 脚本 API 不支持 | 快捷键模拟/文件监控 |
| 不能创建 Pattern | 无公开 API | FL Studio 内部接口/hack |
| 需要 FL Studio 运行 | 无法绕过 | — |
| Windows 支持不完整 | MIDI 虚拟端口方案差异 | loopMIDI 成熟方案 |

## 我们的差异化机会

1. **功能整合** — 合并两个主流项目的优点
2. **突破限制** — 探索插件加载和 Pattern 创建
3. **中文支持** — 国内用户友好
4. **OpenClaw 集成** — 作为 OpenClaw MCP Server 使用

---

*此文件持续更新*
