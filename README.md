# Claude HUD

> Claude Code 实时状态栏插件 - 显示上下文使用量、工具状态、Agent 进度和套餐用量

![Star History](https://api.star-history.com/svg?repos=fxzer/claude-hud-zhipu&type=Date)

## ✨ 功能特性

- **实时上下文显示** - Token 使用量百分比、剩余容量
- **套餐用量监控** - 支持 Anthropic 和智谱 AI 的用量查询
- **工具状态追踪** - 显示运行中和已完成的工具
- **Agent 进度** - 子任务状态和进度
- **Todo 管理** - 任务进度可视化
- **Git 状态** - 分支、是否有未提交更改
- **配置统计** - CLAUDE.md、规则、MCP、钩子数量

## 📸 预览

```
[GLM-4.7 | Zhipu] │ claude-hud git:(main*)
Context 15% │ Quota token █░░░░░ 11% (03-21 14:08) │ mcp ██████ 100%
◐ Edit: zhipu-usage.ts | ✓ Read ×3
▸ Fix authentication bug (2/5)
2 CLAUDE.md | 19 rules | 5 MCPs | 4 hooks
```

## 🚀 安装

### 前置要求

- Claude Code v1.0.80+
- Node.js 18+ 或 Bun

### 方式一：本地安装（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/fxzer/claude-hud-zhipu.git ~/.claude/plugins/claude-hud
cd ~/.claude/plugins/claude-hud

# 2. 安装依赖并构建
npm ci && npm run build

# 3. 配置状态栏
# 编辑 ~/.claude/settings.json，添加以下内容：
```

```json
{
  "statusLine": {
    "type": "command",
    "command": "bash -c 'exec \"$(command -v bun 2>/dev/null || command -v node)\" \"$HOME/.claude/plugins/claude-hud/dist/index.js\"'"
  }
}
```

### 方式二：手动配置

如果自动设置失败，手动编辑 `~/.claude/settings.json`：

```json
{
  "statusLine": {
    "type": "command",
    "command": "bash -c 'plugin_dir=$HOME/.claude/plugins/claude-hud; exec \"$(command -v bun 2>/dev/null || command -v node)\" \"${plugin_dir}/dist/index.js\"'"
  }
}
```

### 重启 Claude Code

配置完成后，重启 Claude Code 即可看到状态栏。

## ⚙️ 配置

### 基础配置

编辑 `~/.claude/plugins/claude-hud/config.json`：

```json
{
  "display": {
    "showModel": true,
    "showContextBar": true,
    "showUsage": true,
    "showTools": false,
    "showAgents": false,
    "showTodos": false,
    "showProject": true,
    "showConfigCounts": true,
    "showDuration": true
  },
  "gitStatus": {
    "enabled": true,
    "showDirty": true,
    "showAheadBehind": false
  },
  "usageThreshold": 80,
  "pathLevels": 1
}
```

### 智谱 AI 用量配置

智谱 AI 用量查询需要设置环境变量：

```bash
# 在 ~/.zshrc 或 ~/.bashrc 中添加
export ANTHROPIC_AUTH_TOKEN="your_zhipu_api_key"
export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/anthropic"
export ANTHROPIC_MODEL="glm-4.7"
```

用量显示说明：
- **Token 额度** - 智谱 API 的 Token 使用配额
- **MCP 时间** - 智谱 MCP 服务的时间限额

### Anthropic 用量配置

Anthropic 用量通过 OAuth 自动获取，无需额外配置。使用自定义 API Key 时会显示 "API" 标签。

## 🛠️ 开发

```bash
# 克隆仓库
git clone https://github.com/fxzer/claude-hud-zhipu.git
cd claude-hud-zhipu

# 安装依赖并构建
npm ci && npm run build

# 运行测试
npm test

# 监听模式开发
npm run dev
```

### 测试状态栏

```bash
echo '{"model":{"id":"glm-4.7","display_name":"GLM-4.7"},"context_window":{"context_window_size":128000,"current_usage":{}},"transcript_path":"/tmp/test.jsonl"}' | node dist/index.js
```

## 📦 发布到 npm

```bash
# 登录 npm
npm login

# 更新版本
npm version patch

# 发布到 npm
npm publish
```

发布后，其他用户可以通过以下方式安装：

```bash
# 安装到本地插件目录
npm install fxzer/claude-hud-zhipu --save ~/.claude/plugins/claude-hud
cd ~/.claude/plugins/claude-hud
npm run build
```

## 🔧 高级配置

### 颜色主题

```json
{
  "colors": {
    "critical": "red",
    "warning": "yellow",
    "good": "green"
  }
}
```

### 阈值设置

```json
{
  "usageThreshold": 80,
  "environmentThreshold": 80
}
```

## 🔍 故障排查

### 状态栏不显示

1. 检查 `~/.claude/settings.json` 中的 `statusLine` 配置
2. 确认 `dist/index.js` 文件存在
3. 重启 Claude Code

### 智谱用量不显示

1. 检查环境变量 `ANTHROPIC_AUTH_TOKEN` 是否设置
2. 确认模型 ID 以 `glm-` 开头
3. 查看缓存文件 `~/.claude/plugins/claude-hud/.zhipu-usage-cache.json`

### 调试模式

```bash
DEBUG=claude-hud:* echo '{"model":{"id":"glm-4.7"}}' | node dist/index.js
```

## 📄 许可证

MIT — 详见 [LICENSE](LICENSE)

## 🙏 致谢

基于 [fxzer/claude-hud](https://github.com/fxzer/claude-hud) 开发
