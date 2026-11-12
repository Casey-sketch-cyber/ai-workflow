---
name: agent-session
description: 在一个已上线（有 AGENTS.md）的项目里开始和结束一次 AI 工作会话的标准流程：同步代码、读交接记录、做任务、验证、写交接、提交并合并到 main。任何 AI 在项目里接新任务时使用。
---

# 日常协作：一次工作会话的标准流程

适用于已经按 `project-onboarding` 上线、根目录有 `AGENTS.md` 的项目。
如果项目还没有 `AGENTS.md`，先停下，提醒用户先运行 `project-onboarding`。

## 开始

1. **同步最新代码**
   ```bash
   git checkout main
   git pull
   ```
   有未提交的改动时，先停下问用户怎么处理，不要擅自丢弃或覆盖。

2. **读上下文**（按顺序）：`AGENTS.md` → `docs/PROGRESS.md` → `docs/ARCHITECTURE.md` → `docs/DECISIONS.md`

3. **确认任务**：用一两句话复述你理解的任务和打算改动的范围。如果任务和 `DECISIONS.md` 里的决定冲突，先问用户。

4. **选择在哪里改**
   - 小改动、只有你一个 AI 在干活：可以直接在 `main` 上改。
   - 改动较大，或可能有其他 AI 同时在改：开分支。
     ```bash
     git checkout -b <工具名>/<任务简述>
     ```
     工具名用你自己的名字：`codex`、`cursor`、`claude`、`grok` 等。

## 进行中

- 只改与任务相关的文件。
- 小步提交，每次提交说明写清楚做了什么。
- 发现任务比预想的大，或者需要改动约定之外的模块，先停下问用户。

## 结束

1. **验证**：运行 `AGENTS.md` 里的测试和检查命令，确认通过。没法通过时，在交接记录里如实写明。
2. **写交接**：在 `docs/PROGRESS.md` 的"工作记录"最上方追加一条：
   ```markdown
   ### YYYY-MM-DD · <工具名> · <分支名或 main>
   - **完成**：
   - **未完成**：
   - **注意 / 坑**：
   ```
   同时更新"当前状态""待办""已知问题"。
3. **记决策**：做了重要的设计取舍，就在 `docs/DECISIONS.md` 加一行。
4. **提交并推送**
   ```bash
   git add .
   git status        # 确认没有密钥、大文件、AI 缓存
   git commit -m "<类型>: <说明>"
   ```
   - 直接在 `main` 上改的：先 `git pull --rebase`，再 `git push`。
   - 用分支的：`git push -u origin <分支名>`，然后进入下一步。
5. **自行合并到 `main`**（用分支时）。验证通过就合并，不需要等用户审核：
   - 能创建 PR：`gh pr create --fill`，然后立即合并（`gh pr merge --merge`）。
   - 不能创建 PR：
     ```bash
     git checkout main
     git pull
     git merge <分支名>
     git push
     ```
   合并前先同步最新的 `main`。有冲突且自己解决不了时，停下问用户，不要丢弃别人的改动。不要强制推送 `main`。
6. **仍然要停下等用户的情况**：验证没通过；任务需要用户做决定（例如项目文档里写明的决策关口）；改动超出了约定的范围。这些情况下不要合并，把情况告诉用户。

## 汇报

结束时用中文简短告诉用户：改动是否已在 `main` 上（或停在哪个分支、为什么没合并）、做了什么、测试结果、需要用户决定或处理的事。
