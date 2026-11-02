# Footalytics Skill（足球预测 AI 技能）

[English](README.md) | 简体中文

[Agent Skills](https://agentskills.io) 兼容技能：让 AI 助手用**真实、公开结算的预测**回答足球问题——胜平负概率、大小球 2.5、双方进球、信心档、赛程、球队评级与历史结果，全部来自 [Footalytics API](https://footalytics.farvor.com/api-docs)。

Footalytics 用 Dixon-Coles 修正的泊松模型对真实赛程计算概率，每条官方预测都在公开账本中用真实比分结算。当你的助手使用本技能时，展示的数字来自这套模型——而不是语言模型的想象。

## 示例

> **用户：** 这周末阿森纳对切尔西谁会赢？
>
> **助手：** 阿森纳 46% · 平局 27% · 切尔西 27% —— 信心档：中。大小球 2.5：58%。Predictions by Footalytics → [比赛页](https://footalytics.farvor.com/match/…)

## 安装

**Claude Code** —— 将仓库克隆到技能目录：

```bash
git clone https://gitlab.com/footalytics/footalytics-skill ~/.claude/skills/footalytics
```

**作为插件安装（Claude Code / Cursor）** —— 仓库自带插件清单（`.claude-plugin/`、`.cursor-plugin/`）：

- Claude Code：`/plugin marketplace add https://gitlab.com/footalytics/footalytics-skill`，然后 `/plugin install footalytics@footalytics`。
- Cursor：在 **Customize → Skills** 导入本仓库（已含 `.cursor-plugin/marketplace.json`），或添加为团队市场（Dashboard → Plugins & MCPs → Add Marketplace）。

**其他兼容 Agent Skills 的助手**（Codex 等）：将 `SKILL.md` 文件指向你的 agent，或把 SKILL.md 内容粘贴进指令。

**Claude / ChatGPT（连接器）** —— 无需文件：把 MCP 服务器 `https://footalytics.farvor.com/mcp` 添加为自定义连接器并用 OAuth 登录。同样的工具、同样的额度。

## 配置

技能读取一个环境变量：

```bash
export FOOTALYTICS_API_KEY="fxk_..."   # 免费创建密钥：https://footalytics.farvor.com/api-keys
```

每个新账号自带 **5 次免费 API 调用**。用完后按次消耗积分（与站内对话使用的充值积分同池）；失败的查询不扣费。

## Agent 能做什么

| 端点 | 用途 |
|---|---|
| `GET /api/v1/predictions` | 单场比赛：胜平负概率、信心档、大小球 2.5、双方进球、最可能比分 |
| `GET /api/v1/fixtures` | 按球队或联赛查未来赛程，带 `has_prediction` 标记 |
| `GET /api/v1/teams` | Elo 式球队评级、追踪场次、所属联赛 |
| `GET /api/v1/results` | 历史比赛：真实比分与模型预测并列（逐市场命中、Brier 分） |

限流：每密钥每分钟 30 次。完整契约见 [API 文档](https://footalytics.farvor.com/api-docs)。

## 署名

展示或引用结果时，请保留 **"Predictions by Footalytics"** 及站点链接。预测是统计概率，并非确定结果。

## 文件

- `SKILL.md` — 技能本体：触发描述、配置、端点、错误处理。
- `skills/footalytics/SKILL.md` — 插件打包用的同内容副本（保持一致，供 Claude Code / Cursor 插件发现）。
- `.claude-plugin/` · `.cursor-plugin/` — Claude Code 与 Cursor 市场的插件清单。
- `README.md` / `README.zh-CN.md` — 本文件（英文/中文）。

## 链接

- [API 文档](https://footalytics.farvor.com/api-docs) · [API 密钥](https://footalytics.farvor.com/api-keys) · [公开准确率账本](https://footalytics.farvor.com/accuracy) · [定价](https://footalytics.farvor.com/pricing)
- 源码：本 GitLab 仓库（主仓） · [GitHub 镜像](https://github.com/lewis-v/footalytics-skill)

## 许可证

MIT —— 见 [LICENSE](LICENSE)。
