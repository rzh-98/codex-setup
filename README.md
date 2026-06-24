# codex-setup

#### 在不同设备上安装 Codex 以及常用 Agent Skills 的同步入口

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./LICENSE)

每次换电脑、重装环境、临时开一台新机器，都不想再手动找 skill、复制目录、检查路径。

这个仓库只做一件事：把我常用的一组 Agent Skills 按固定清单安装到当前设备上。  
它不是 skill 市场，也不是大而全合集，只是我自己的“开箱必装列表”。

---

## （可选）Clash-mihomo安装

![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)



## （可选）Codex CLI 安装流程

Codex CLI 支持 macOS、Windows 和 Linux。推荐优先使用官方 standalone installer；如果你已经有 Node.js/npm 环境，也可以用 npm 安装。

### 1. （可选）安装 Node.js 和 npm

如果选择 npm 安装方式，需要先安装 Node.js。npm 会随 Node.js 一起安装。

检查是否已安装：

```bash
node -v
npm -v
```

#### Windows 推荐：

```
winget install --id OpenJS.NodeJS.LTS
```

#### macOS / Linux 安装 Node.js 和 npm

npm 会随 Node.js 一起安装。推荐使用 `nvm`，可以避免全局 npm 包权限问题，也方便切换 Node 版本。

```bash
# 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.5/install.sh | bash

# 让当前 shell 立即加载 nvm
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# 安装最新 LTS Node.js，npm 会一起安装
nvm install --lts
nvm use --lts
nvm alias default 'lts/*'

# 验证
node -v
npm -v
```

### 2. 安装 Codex CLI

官方 standalone 安装：

```
# macOS / Linux
curl -fsSL https://chatgpt.com/codex/install.sh | sh
# Windows PowerShell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

使用 npm 安装：

```
npm install -g @openai/codex
```

### 3. 启动并登录

```
codex
```

首次运行会提示登录，可以使用 ChatGPT 账号，也可以使用 OpenAI API key。

### 4. 验证安装

```
codex --version
codex doctor
```

### 5. 常见问题

如果 Windows PowerShell 报错：

```
npm.ps1 cannot be loaded because running scripts is disabled on this system.
```

可以按需调整执行策略：

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

修改执行策略前建议先确认公司或设备安全策略。

来源：OpenAI 官方 [Codex Quickstart](https://developers.openai.com/codex/quickstart)、[Codex CLI](https://developers.openai.com/codex/cli)，以及 npm 官方 [Node.js and npm 安装说明](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm/)。

---

## 常用Skill列表

### 写作相关

| Skill                         | 用途                                    | 原链接                                                       |
| ----------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| `awesome-ai-research-writing` | 学术写作、论文润色、双语翻译            | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[Leey21/awesome-ai-research-writing](https://github.com/Leey21/awesome-ai-research-writing) |
| `nature-skills`               | 符合nature论文学术表达和科研绘图的Skill | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[Yuan1z0825/nature-skills](https://github.com/Yuan1z0825/nature-skills) |
| `humanizer`                   | 去除英文 AI 写作痕迹                    | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[blader/humanizer](https://github.com/blader/humanizer) |
| `humanizer-zh`                | 去除中文 AI 写作痕迹                    | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[blader/humanizer](https://github.com/blader/humanizer) |

### Agent 记忆相关

| Skill        | 用途                     | 原链接                                                       |
| ------------ | ------------------------ | ------------------------------------------------------------ |
| `neat-freak` | 会话结束后同步文档和记忆 | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[KKKKhazix/khazix-skills]([KKKKhazix/khazix-skills: 数字生命卡兹克开源的 AI Skills 合集](https://github.com/KKKKhazix/khazix-skills/#-neat-freak洁癖)) |

### 代码相关

| Skill            | 用途                                      | 原链接                                                       |
| ---------------- | ----------------------------------------- | ------------------------------------------------------------ |
| `Superpowers`    | 一套完整的软件开发方法论                  | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[obra/superpowers]([[obra/superpowers: An agentic skills framework & software development methodology that works.](https://github.com/obra/superpowers)) |
| `Superpowers-zh` | Superpowers完整汉化 + 4 个中国原创 skills | ![GitHub Repo](https://img.shields.io/badge/github-repo-blue?logo=github)[jnMetaCode/superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) |



## Codex相关问题解决方案

### 沙箱权限

[Codex CLI bwrap 沙箱报错 `bwrap: loopback: Failed RTM_NEWADDR` 的安全修复方案 - Taolaw - 博客园](https://www.cnblogs.com/Taolaw/p/20184477)

### SQL 日志导致的磁盘写入 BUG

Codex Prompt：

> 帮我检查 ~1.codex/logs_2.sglite 是否因 TRACE 日志持续高频写盘；如果中招，先备份，再用 SQLite trigger 拦截 logs 表 insert，并 checkpoint/truncate WAL，最后采样确认 MAX（id）和WAL 不再增长。