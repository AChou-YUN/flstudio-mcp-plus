# 场景分析：AI 辅助音乐制作

> 从制作者视角梳理 MCP 能覆盖的场景，以 progressive house 为例

## 创作构思阶段

| 场景 | 可行性 | 推荐方案 | 说明 |
|------|--------|---------|------|
| AI 生成和弦进行 | ✅ | fl-studio-mcp | "写一个 A 小调 4 小节 1645 进行" |
| AI 生成鼓组 pattern | ✅ | flai-mcp | "写一个 four-on-the-floor kick" |
| AI 生成旋律/Bassline | ✅ | fl-studio-mcp | Piano Roll 精度更高 |
| AI 生成琶音 | ✅ | fl-studio-mcp | "每拍 4 个 16 分音符从根音向上" |
| AI 生成编曲结构 | ⚠️ 部分 | flai-mcp | 能加标记，但无法排列 clip |
| AI 生成多轨编排 | ❌ | — | Playlist clip 操作不存在 |

## 音色设计阶段

| 场景 | 可行性 | 推荐方案 | 说明 |
|------|--------|---------|------|
| AI 调整合成器参数 | ✅ | flai-mcp | "cutoff 60%, resonance 25%" |
| AI 切换预设 | ✅ | flai-mcp | "循环切换 FLEX 预设" |
| AI 搜索音色 | ❌ | — | AI 无法"听"音色 |
| AI 音色匹配建议 | ⚠️ | 需外部 | 可文字建议，无法直接操作 |

## 混音阶段

| 场景 | 可行性 | 推荐方案 | 说明 |
|------|--------|---------|------|
| AI 调节音量平衡 | ✅ | flai-mcp | "Kick 降 3dB，Hi-hat 硬声像" |
| AI 静音/独奏检查 | ✅ | flai-mcp | "独奏第 3 轨" |
| AI 加 EQ/压缩 | ❌ | — | 第三方插件 UI 无法脚本化 |
| AI 做自动化 | ❌ | — | Automation Clip 不在 API 范围 |
| AI 总线路由 | ✅ | flai-mcp | Channel Rack Mixer 路由可控制 |

## 工程管理阶段

| 场景 | 可行性 | 推荐方案 | 说明 |
|------|--------|---------|------|
| AI 重命名/改色 Pattern | ✅ | flai-mcp | "鼓 pattern 涂红色" |
| AI 克隆 Pattern | ✅ | flai-mcp | "复制 pattern 3 到 7" |
| AI Mixer 轨道命名 | ✅ | flai-mcp | "第 1 轨命名 Kick" |
| AI 导出音频 | ❌ | — | FL API 不支持 |

## 实际使用示例：Progressive House 4 小节

### 第一步：鼓组（flai-mcp）

```
在 Step Sequencer 上写 progressive house 鼓组，4 小节，128 BPM：
- Kick：每小节第 1、3 拍（重拍）
- Clap：每小节第 2、4 拍的正拍
- Closed Hi-hat：16 分音符交替
- Open Hi-hat：每小节最后一拍的第 4 个 16 分音符
```

### 第二步：和弦 Pad（fl-studio-mcp）

```
在 Piano Roll 的 FLEX Pad 通道写和弦进行：
- 调性：A 小调
- 拍号：4/4
- 小节数：4 小节
- 进行：Am - F - Dm - E（1-6-4-5）
- 每个和弦占 1 小节，全音符，velocity 80
```

### 第三步：琶音（fl-studio-mcp）

```
在 Piano Roll 的 Serum 通道写琶音：
- 跟随和弦进行
- 每拍 4 个 16 分音符，从根音向上
- 音域：C4-C6
- velocity 渐强：60→80→100→120
```

### 第四步：Bass（fl-studio-mcp）

```
在 Piano Roll 的 3xOsc 写 bass：
- 跟随和弦根音
- 每小节第 1、3 拍弹根音，八度低音
- 8 分音符，velocity 100
```

### 第五步：工程整理（flai-mcp）

```
- Pattern 1 改名 "Pad"，颜色蓝色
- Pattern 2 改名 "Arp"，颜色绿色
- Pattern 3 改名 "Drums"，颜色红色
- Pattern 4 改名 "Bass"，颜色黄色
- 设置 BPM 128
```

## 理想场景 vs 现实

| 用户期望 | 现状 | 卡点 |
|---------|------|------|
| "4/4拍 8小节 A调 1645琶音" | ⚠️ 勉强 | Piano Roll 写入可行，精度受限 |
| "底鼓在重拍13，军鼓在每小节最后一拍" | ✅ | Step Sequencer 天然支持 |
| "直接导出分轨" | ❌ | FL API 不支持导出 |
| "听效果自动调整" | ❌ | AI 无法"听" |

## 核心价值定位

```
Suno：给你一个烤好的蛋糕，不满意重新生成
flstudio-mcp-plus：AI 备料切菜，你来炒菜 — 速度比从零快，控制权在你
```

AI 辅助 ≠ AI 替代。目标是**加速创作流程**，不是取代制作者的审美判断。
