# 🗺️ 产品路线图

> 最后更新：2026-06-18

## Phase 0：调研与体验（当前）

**目标：** 了解现有方案，确定技术方向

- [x] 调研现有 FL Studio MCP 项目（flai-mcp、fl-studio-mcp）
- [x] 分析信号原理差异（SysEx vs 文件队列）
- [x] 梳理场景覆盖与痛点
- [ ] 部署 flai-mcp，体验全功能控制
- [ ] 部署 fl-studio-mcp，体验 Piano Roll 写入
- [ ] 记录使用痛点与需求

## Phase 1：基础搭建（MVP）

**目标：** 可用的 MCP Server，覆盖核心功能

- [ ] 搭建 MCP Server 基础框架（FastMCP）
- [ ] 实现 MIDI SysEx 通信桥接
- [ ] Transport 控制（播放/停止/录音/调速）
- [ ] Mixer 基础控制（音量/声像/静音/独奏）
- [ ] Channel Rack 控制
- [ ] Pattern 管理（列表/切换/命名）
- [ ] Windows loopMIDI 集成

## Phase 2：音符写入

**目标：** 能通过 AI 写入旋律、和弦、鼓组

- [ ] Step Sequencer 音符写入（鼓组）
- [ ] Piano Roll 脚本集成（旋律/和弦/Bass）
- [ ] 支持音高、力度、时值、微时序
- [ ] 中文音乐术语理解（调性、拍号、进行）

## Phase 3：差异化创新

**目标：** 突破现有方案限制

- [ ] 融合 SysEx 控制 + Piano Roll 写入
- [ ] 插件参数控制与预设切换
- [ ] 工程管理自动化（命名/颜色/克隆）
- [ ] 中文自然语言交互
- [ ] OpenClaw 集成

## Phase 4：突破性功能

**目标：** 实现现有方案做不到的事

- [ ] Playlist clip 排列（探索 PyFLP / .flp 文件操作）
- [ ] 自动导出分轨
- [ ] 风格模板系统（progressive house / future bass 等）
- [ ] 音频分析反馈（接入听觉模型）

## 参考项目

- [kaupau/flai-mcp](https://github.com/kaupau/flai-mcp) — SysEx 全功能控制（MIT）
- [karl-andres/fl-studio-mcp](https://github.com/karl-andres/fl-studio-mcp) — Piano Roll 脚本
- [PyFLP](https://github.com/demberto/PyFLP) — .flp 文件解析库
- [music-copilot](https://github.com/TommyX12/music-copilot) — subprocess IPC 方案
- [FL Studio API Stubs](https://il-group.github.io/FL-Studio-API-Stubs/) — 官方 API 文档
