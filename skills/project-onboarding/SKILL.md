---
name: project-onboarding
description: 把一个本地项目（包括 Codex 等 AI 工具生成的项目文件夹）整理干净、补齐多智能体协作文档，并上传为 GitHub 私有仓库。用户说"上线这个项目""把项目传到 GitHub""初始化多 agent 协作"时使用。
---

# 项目上线：整理 → 补文档 → 上传 GitHub

目标：让任何 AI 工具（Codex、Cursor、Claude、Grok……）以后都能只靠仓库里的文件接手这个项目。
本 skill 同目录下的 `templates/` 里有所有模板文件。

## 硬性规则（任何一步都不能违反）

- **绝不上传密钥**：API Key、密码、token、`.env`、证书私钥等。
- **绝不自己处理 GitHub 密码或 token**：需要登录或授权时停下来，让用户自己完成。
- **不删除用户的任何原始文件**：只能新增文件、修改 `.gitignore`。需要移动或删除时，先列清单问用户。
- **仓库默认私有（private）**，除非用户明确要求公开。
- 文档内容必须来自实际代码；看不出来的地方写"待补充"，不要编造。

## 第 0 步：确认信息

开始前向用户确认（用户已经说过的就不用再问）：
1. 项目的本地路径
2. GitHub 仓库名（默认用文件夹名，小写加连字符）
3. 是否私有（默认是）

## 第 1 步：盘点文件

列出项目目录结构（忽略 `node_modules`、`.venv` 这类依赖目录），把内容分成四类，并用表格汇报给用户：

| 类别 | 例子 | 处理 |
|---|---|---|
| 项目代码与资源 | src/、tests/、配置文件、README | 上传 |
| AI 工具私有文件 | `.codex/`、对话记录、agent 缓存、临时脚本输出 | 写进 .gitignore |
| 敏感信息 | `.env`、含 key 的配置、私钥 | 写进 .gitignore，另建 `.env.example` |
| 大文件 | 数据集、模型权重、视频（> 50MB） | 写进 .gitignore，或问用户是否用 Git LFS |

拿不准某个文件属于哪一类时，列出来问用户。

## 第 2 步：.gitignore

- 以 `templates/gitignore` 为基础（复制到项目根目录时改名为 `.gitignore`），按项目语言删掉无关部分，再加上第 1 步发现的私有文件、敏感文件和大文件。
- 已经有 `.gitignore` 时，合并进去，不要覆盖。

## 第 3 步：密钥扫描

在准备提交的文件里搜索疑似密钥，至少检查以下模式：
`api_key`、`apikey`、`secret`、`token`、`password`、`sk-`、`ghp_`、`AKIA`、`-----BEGIN`

- 发现硬编码的密钥：告诉用户具体位置，建议改成从环境变量读取，并新建 `.env.example`（只写变量名，不写值）。
- 用户确认处理完之前，**不要进入第 6 步**。

## 第 4 步：补齐协作文档

按 `templates/` 里的模板，在项目根目录创建以下文件（已存在就补充，不要覆盖）：

- `AGENTS.md`：所有 AI 的说明书，根据实际代码填写安装、运行、测试命令
- `docs/ARCHITECTURE.md`：目录结构、模块职责、数据流
- `docs/PROGRESS.md`：当前状态、待办、已知问题
- `docs/DECISIONS.md`：先留空表头，以后记录重要决策

再为使用自己规则文件的工具创建"指针文件"，内容都只有一句"请先阅读并遵守根目录的 AGENTS.md"：
- `CLAUDE.md`

## 第 5 步：初始化 Git 并提交

```bash
git init            # 已经是仓库就跳过
git add .
git status          # 再确认一遍：没有 .env、没有大文件、没有 AI 私有文件
git commit -m "chore: 初始化项目，补齐多智能体协作文档"
git branch -M main
```

把 `git status` 的结果给用户过目，再继续。

## 第 6 步：创建 GitHub 仓库并推送

按顺序尝试：

1. **已安装并登录了 GitHub CLI（`gh auth status` 正常）**：
   ```bash
   gh repo create <仓库名> --private --source=. --push
   ```
2. **没有 gh，或者 gh 没登录**：停下来，请用户自己操作：
   - 在 github.com 新建一个**空**仓库（不要勾选添加 README 或 .gitignore），设为 Private
   - 把仓库地址发给你
   - 然后执行：
   ```bash
   git remote add origin <用户给的地址>
   git push -u origin main
   ```
   如果推送时要求登录，让用户自己在弹出的窗口或终端里完成。
3. **你运行在云端沙盒、没有推送权限**：说明情况，告诉用户在 Cursor 的"源代码管理"面板点 "Publish to GitHub"，或者在 Codex 设置里连接 GitHub。

## 第 7 步：汇报

用简短的列表告诉用户：
- 仓库地址
- 被忽略的内容（按类别）
- 发现并处理的敏感信息
- 新建的文档
- 还需要用户处理的事项
