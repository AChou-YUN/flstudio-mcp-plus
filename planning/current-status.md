# 当前处境与项目状态

> 更新日期：2026-06-18
> 阶段：Phase 0 调研收尾 → 准备进入需求确认

## 我们是谁

一个大三学生开发者，有 Java 基础，日常使用 FL Studio（Piano Roll 写音符为主），目标是做一个 AI 直接操控 FL Studio 的 MCP Server。

## 我们在哪

### 已完成的调研

| 维度 | 状态 | 结论 |
|------|------|------|
| 现有 MCP 项目 | ✅ | 两个可用项目：flai-mcp（全功能/Step Sequencer）、fl-studio-mcp（Piano Roll/功能窄） |
| 信号原理 | ✅ | SysEx 实时双向 vs 文件队列+触发，各有优劣 |
| AI 生态全景 | ✅ | 三层模型：素材生成（成熟）→ 问答辅助（成熟）→ Agent 操控（空白） |
| 场景覆盖 | ✅ | 鼓组/Mixer/工程管理可行，旋律/和弦可行但精度有差异，Playlist 排列/导出不可行 |
| 痛点分析 | ✅ | 最大卡点：Playlist clip 操作和导出无 API |

### 关键发现

1. **我们不是第一个，但竞争者很少。** MCP 生态里只有 2 个 FL Studio 项目，都还在 Alpha 阶段
2. **官方不会做这个。** Image-Line 的 AI 策略是"辅助灵感"，不是"Agent 操控 DAW"
3. **Suno 不是对手。** Suno 是层次 1（端到端生成），我们做层次 3（Agent 操控 DAW），赛道不同
4. **最大的技术风险是 FL Studio 的 API 限制。** Playlist 排列和导出是硬伤，可能需要绕路（PyFLP 操作 .flp 文件）

### 尚未确认的事

- [ ] flai-mcp 实际部署体验（手感如何？）
- [ ] fl-studio-mcp 实际部署体验（Piano Roll 写入精度？）
- [ ] PyFLP 能否绕过 Playlist API 限制
- [ ] 自动导出是否有技术路径
- [ ] mimo search API 可用（已验证 ✅）

## 下一步：聚焦推进模型

我们确定了这样的推进流程：

```
基础诉求 → 调研 → 需求与场景分析 → 技术确认 → 项目计划 → 项目实施
```

当前处于**调研收尾**阶段，即将进入**需求与场景分析**。

### 需要讨论的问题

1. **基础诉求明确：** 你到底要 AI 帮你做什么？（写鼓？写旋律？混音？全都要？）
2. **优先级排序：** 哪个场景对你最有价值？（省时间最多？最常做？）
3. **技术路线选择：** 先部署体验现有方案，还是直接开始写自己的？
4. **MVP 定义：** 第一个可用版本应该包含哪些功能？

## 仓库当前结构

```
flstudio-mcp-plus/
├── README.md                              ✅ 项目总览
├── docs/
│   ├── research/
│   │   ├── existing-projects.md           ✅ 现有 MCP 项目调研
│   │   ├── signal-mechanism.md            ✅ 信号原理对比
│   │   ├── comparison.md                  ✅ 方案对比
│   │   └── ai-ecosystem.md                ✅ AI 生态全景（新增）
│   ├── requirements/
│   │   └── use-cases.md                   ✅ 场景分析
│   └── architecture/                      待填充
├── planning/
│   ├── roadmap.md                         ✅ 路线图
│   ├── current-status.md                  ✅ 当前处境（本文）
│   └── milestones.md                      里程碑
├── src/                                   待填充
├── scripts/                               待填充
└── examples/                              待填充
```
