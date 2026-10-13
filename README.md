# 福昕 Office 助手 / Foxit Office Assistant (fuxin-office)

**福昕 Office 助手**（又称：福昕 Office 全家桶技能，英文：Foxit Office Assistant / Foxit Office Skill，技术标识：`fuxin-office`）—— 让 AI Agent（Claude / GPT / DeepSeek / Kimi / 智谱 / 通义 / WorkBuddy 等）通过本地 MCP 网关，直接读写用户本机的 Word / Excel / PowerPoint 文档。

> Foxit Office Assistant (aka Foxit Office Skill, `fuxin-office`) — enables AI agents to read and edit local documents (Word / Excel / PowerPoint) via the Foxit local MCP gateway (`FuxinAiService`).

- **Skill ID**: `org-sd52yaru/fuxin-office`
- **入口技能（唯一对外）**: `fuxin-office`（详见 [`fuxin-office/SKILL.md`](./fuxin-office/SKILL.md)）
- **运行环境**: Windows + 福昕 Office（需 Agent Toolkit 插件）+ 本地网关 `FuxinAiService`
- **版本**: 1.00.03.235

## What it does / 能力一览

| 能力域           | 场景示例                                   |
| ------------- | -------------------------------------- |
| Word 编排       | 写报告 / 术语统一排版 / 高亮批注 / 清单审查             |
| Excel 编排      | 抽数建新表 / 条件高亮 / 批量建图（柱/折/饼/面积）          |
| PowerPoint 编排 | 生成汇报 Deck / 整理演示 / 选区编辑                |
| 只读问答          | Word/Excel/PPT 内容问答、摘要、定位、数据核对（绝不修改文档） |
| 批次撤销          | 批次事务边界 + 一次 Ctrl+Z 整组撤销                |
| 企业自定义流程       | 组合场景工具封装专属管线（如合同评审）                    |

## How it works / 工作原理

```
AI Agent ──(Skill 编排层 fuxin-office)──> 本地 MCP 网关 FuxinAiService ──> 福昕 Office 文档
```

1. **5 层预检**（`reference/bridge.md`）：网关 → 产品注册 → 技能注册 → 后端可达性 → 活动文档，输出四档状态（未安装 / 未就绪 / 半就绪 / 就绪）。
2. **MCP 调用**：JSON-RPC 2.0，`protocolVersion 2024-11-05`，流程 `initialize` → `notifications/initialized`（透传 `Mcp-Session-Id`）→ `tools/call`。
3. **端口动态读取**：网关端口**非固定值**，从 `%ProgramData%\Foxit Software\Fuxin Office\MCP\MCPServerPort.ini` 的 `ListenPort` 字段读取，**禁止硬编码**。
4. **请求头要求**：`Accept` 必须同时包含 `application/json` 与 `text/event-stream`。

## Prerequisites / 前置条件

- Windows 10/11
- 已安装福昕 Office 并启用 **Agent Toolkit** 插件（网关载体，缺失时 `FuxinAiService` 不会启动）
- `FuxinAiService` 网关已启动、对应产品（Word / Excel / PowerPoint）已注册并打开活动文档

## Deployment / 部署

1. 将本套件解压到平台的技能加载目录（只需入口 `SKILL.md` 及同级 `reference/`，平台仅发现并暴露 `fuxin-office` 一个技能）。
2. 启动福昕 Office，确认网关 `FuxinAiService` 运行中。
3. Agent 按 `fuxin-office/SKILL.md` 的编排规约调用（先预检、后路由到 `reference/*.md` 对应能力）。

目录结构、输入输出 schema、示例用例见 [`fuxin-office/README.md`](./fuxin-office/README.md)、[`fuxin-office/schema/input_schema.json`](./fuxin-office/schema/input_schema.json)、[`fuxin-office/examples/sample_case.json`](./fuxin-office/examples/sample_case.json)。

## Links / 相关链接

- SkillHub: `@org-sd52yaru/fuxin-office`（<https://skillhub.cn/skills/fuxin-office>）
- 已上线平台：腾讯 SkillHub、ClawHub、阿里魔塔、豆包
- 授权：见 [`fuxin-office/LICENSE.md`](./fuxin-office/LICENSE.md) ｜ © 2026 Fuxin. All rights reserved.
