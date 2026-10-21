# CodexClaw

> 离线私有化 AI Agent —— **QQ 是入口，Codex 是手脚，本地模型是大脑。**

在手机上给 QQ 发一句话，让电脑真的去干活：读文件、改代码、跑命令、操作桌面。
大脑跑在本机，不依赖云端模型。

## 三个角色

| 角色 | 是什么 | 干什么 |
|------|--------|--------|
| **CodexClaw** | Rust 编排层 | QQ 接入、多会话、审批流、定时任务、记忆与技能注入 |
| **OpenAI Codex** | App-Server 执行层 | shell / 文件编辑 / computer-use / 沙箱与审批 |
| **llama.cpp** | 本地推理 | 通过 OpenAI 兼容端点提供大脑，全离线 |

CodexClaw 不重造执行能力，只负责「谁在什么时候让 Codex 干什么」。
换大脑只改一个 `base_url`，执行层与编排层不动。

## 两种用法

| 入口 | 说明 |
|------|------|
| **QQ 机器人** | 手机发消息，电脑执行。需自备 QQ 开放平台 AppID / AppSecret |
| **`codex-claw chat`** | 终端对话框，不起 QQ 网关；与机器人共用同一套记忆、技能、会话 |

## 产品结构（离线便携包）

```
codexclaw_portable/
├─ start-portable.bat    一键启动：自检 → 起大脑 → 等就绪 → 起 CodexClaw
├─ chat.bat              本地对话窗口（不启 QQ）
├─ _portable-env.bat     共用环境：HOME / PATH / 状态目录
├─ codex-claw.exe        编排层主程序
├─ codex-cli/            执行层（OpenAI Codex CLI，内含原生 codex.exe）
├─ llamacpp/             大脑：llama-server + Gemma-4-E4B gguf（约 4.6GB）
├─ config/
│  ├─ codexclaw.toml     CodexClaw 主配置
│  └─ codex-config.toml  codex 的 provider 配置（指向本地 8080）
├─ skills/               能力模块示例：sys-health / downloads-report / computer-use
├─ install-skill.bat     离线加装技能
├─ uninstall-skill.bat   离线卸载技能
├─ codexclaw-src/        上游文档子集（README / getting-started / 配置样例）
├─ DEPLOY.md             离线部署指南（路径改写、离线排错、便携模式）
├─ LICENSE               MIT
└─ THIRD-PARTY-NOTICES.md 第三方组件与模型权重许可说明
```

状态目录：默认在 `~/.codex-claw/`（`data/` 存记忆与会话，`.codex/` 是 codex home）；
`CC_PORTABLE_DATA=1` 时改为包内 `data/`，整目录可随 U 盘迁移。

## 三条设计取向

- **离线优先** — 大脑指向本地端点，执行在本机，只有 QQ 收发需要网络。
- **能力可加装** — 技能 = 一个文件夹（`SKILL.md` + 可选脚本），`install-skill.bat` 离线装入，即插即用。
- **状态可迁移** — 记忆、会话、技能都是文件，拷走就能换台机器接着聊。

## 环境要求

- Windows x64（脚本为 `.bat`，二进制为 win32-x64）
- 离线包约 **5.0GB**，其中模型 gguf 约 4.6GB
- 已在 `codex-cli 0.137.0` + `Gemma-4-E4B-It Q4_K_M` 上验证本地链路

## 当前状态

**说明先行，文件暂未上传。** 本仓库目前只放文档占位，源码与离线包待整理后再发布。

已实测：本地自检 6/6、离线对话、记忆读写、技能安装/卸载/发现、便携模式状态隔离。
QQ 入口需配置真实凭据后自测。

## 许可

本项目 MIT。随包分发的第三方组件（OpenAI Codex CLI、llama.cpp、Gemma 模型权重）
遵循各自上游许可 —— **模型权重不是 MIT**，商用或再分发前请阅读其条款。
