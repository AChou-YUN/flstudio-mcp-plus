# 技术验证计划

> 阶段五产出 | 在自研之前先验证现有方案
> 更新日期：2026-06-18

## 验证策略

**核心原则：** 不重复造轮子。先验证已有方案能不能跑通，再决定是融合还是自研。

```
验证 flai-mcp → 验证 fl-studio-mcp → 根据结果决策
    ↓                    ↓
  都能跑通             部分跑通/都不行
    ↓                    ↓
  研究代码融合         分析原因，调整方案
```

## 当前环境

| 项目 | 状态 |
|------|------|
| 操作系统 | Windows |
| FL Studio | 2024 版（21.x，API 够用） |
| Python | 已安装（LTS 版本） |
| MCP 客户端 | Codex |
| 虚拟 MIDI | 待安装（loopMIDI） |

---

## 验证一：flai-mcp（全功能控制）

**目标：** 验证 MCP Server → SysEx → FL Studio 的完整链路

### 步骤

#### 1. 安装 loopMIDI
- 下载：<https://www.tobias-erichsen.de/software/loopmidi.html>
- 安装后创建两个端口：
  - `FLAI In`
  - `FLAI Out`
- 截图确认端口创建成功

#### 2. 克隆并安装 flai-mcp
```bash
git clone https://github.com/kaupau/flai-mcp.git
cd flai-mcp
python -m venv .venv
.venv\Scripts\activate
pip install -e .
```

#### 3. 安装 FL Studio Bridge 脚本
```bash
python scripts/install_fl_bridge.py
```
或手动拷贝 `fl_bridge/device_flai.py` 到：
```
%USERPROFILE%\Documents\Image-Line\FL Studio\Settings\Hardware\FLAI\device_flai.py
```

#### 4. 配置 FL Studio
- 打开 FL Studio
- Options → MIDI Settings
- Input：选择 `FLAI In`，Controller type 选 `FLAI`，Port 设 1，Enable
- Output：选择 `FLAI Out`，Port 设 1，Enable
- 打开 View → Script output，确认看到：`[FLAI] MCP Bridge initialized. Ready.`

#### 5. 配置 Codex 连接 MCP
在 Codex 的 MCP 配置中添加：
```json
{
  "mcpServers": {
    "flai": {
      "command": "path/to/flai-mcp/.venv/Scripts/flai-mcp.exe",
      "args": ["--midi-in", "FLAI In", "--midi-out", "FLAI Out"]
    }
  }
}
```

#### 6. 测试用例

| 测试 | 输入 | 预期结果 | 通过 |
|------|------|---------|------|
| T1 播放控制 | "播放" | FL Studio 开始播放 | |
| T2 停止控制 | "停止" | FL Studio 停止 | |
| T3 写鼓组 | "写一个 four-on-the-floor kick pattern" | Step Sequencer 出现底鼓 | |
| T4 Mixer 控制 | "把 Mixer Track 1 音量调到 -6dB" | 推子变化 | |
| T5 Pattern 管理 | "重命名 Pattern 1 为 Drums" | Pattern 名称改变 | |

### 验证结论（测试后填写）

```
日期：
结果：通过 / 部分通过 / 失败
通过的测试：
失败的测试：
遇到的问题：
结论：
```

---

## 验证二：fl-studio-mcp（Piano Roll 写入）

**目标：** 验证 Piano Roll 脚本能否被触发并写入音符

### 步骤

#### 1. 克隆并安装
```bash
git clone https://github.com/karl-andres/fl-studio-mcp.git
cd fl-studio-mcp
./install.sh
```

#### 2. 安装 Piano Roll 脚本
- 拷贝 `composewithllm.pyscript` 到：
```
%USERPROFILE%\Documents\Image-Line\FL Studio\Settings\Piano roll scripts\
```

#### 3. 配置 FL Studio
- 重启 FL Studio
- Options → MIDI Settings → 选择虚拟 MIDI 端口
- Controller type 选 `flstudiomcp`

#### 4. 测试用例

| 测试 | 输入 | 预期结果 | 通过 |
|------|------|---------|------|
| T1 基础音符 | "C 大调 do re mi" | Piano Roll 出现 C D E | |
| T2 指定调性 | "A 小调 do re mi sol la" | Piano Roll 出现 A B C E F | |
| T3 指定小节 | "4 小节" | 音符跨越 4 小节 | |
| T4 和弦 | "Am 和弦" | Piano Roll 出现 A C E | |

### 验证结论（测试后填写）

```
日期：
结果：通过 / 部分通过 / 失败
通过的测试：
失败的测试：
遇到的问题：
延迟感受：
结论：
```

---

## 决策矩阵

根据两个验证的结果，决定下一步：

| flai-mcp | fl-studio-mcp | 决策 |
|----------|---------------|------|
| ✅ 通过 | ✅ 通过 | 研究两者代码，做融合方案 |
| ✅ 通过 | ❌ 失败 | 基于 flai-mcp 扩展，用 Step Sequencer 写旋律 |
| ❌ 失败 | ✅ 通过 | 基于 fl-studio-mcp 扩展，补 Mixer 控制 |
| ❌ 失败 | ❌ 失败 | 分析失败原因，考虑自研或换方案 |

---

## 风险与应对

| 风险 | 概率 | 应对 |
|------|------|------|
| loopMIDI 安装失败 | 低 | 换 IAC Driver 或其他虚拟 MIDI 方案 |
| FL Studio Script 输出无响应 | 中 | 检查 MIDI 端口配置、脚本路径 |
| MCP 客户端连不上 Server | 中 | 检查 Python 路径、依赖版本 |
| 音符写入精度不够 | 中 | 记录具体问题，评估是否可接受 |
