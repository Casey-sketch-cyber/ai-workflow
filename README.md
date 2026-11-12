# ai-workflow：多 AI 协作的共享规范

这个仓库存放所有 AI 工具（Codex、Cursor、Claude、Grok……）共用的工作流程。
每个项目各自是独立的 GitHub 仓库；本仓库只放"怎么干活"的规则，不放项目代码。

## 内容

```
skills/
├── project-onboarding/     # 把一个项目整理干净、补齐文档、上传 GitHub（每个项目做一次）
│   ├── SKILL.md
│   └── templates/          # .gitignore、AGENTS.md、docs/* 模板
├── agent-session/          # 日常接任务的标准流程（每次干活都用）
│   └── SKILL.md
└── pr-review/              # 审查者审查并合并 PR 的标准流程（由 Claude 使用）
    └── SKILL.md
```

## 怎么用：直接复制发给 AI

下面的指令已经填好了仓库地址，只需替换尖括号里的内容。

### 1. 把一个项目上线到 GitHub（每个项目一次）

```
请先获取我的协作规范：
如果本机还没有 ~/ai-workflow 目录，执行 git clone https://github.com/Casey-sketch-cyber/ai-workflow.git ~/ai-workflow；
如果已经有了，进入该目录执行 git pull。
然后阅读 ~/ai-workflow/skills/project-onboarding/SKILL.md，严格按照里面的步骤，
把项目 <项目路径> 整理并上传为 GitHub 私有仓库，仓库名用 <仓库名>。
模板文件在同目录的 templates/ 里。遇到需要我确认或需要登录的地方，停下来问我。全程用中文。
```

### 2. 在已上线的项目里做任务（日常）

```
请先更新 ~/ai-workflow（没有就 clone，有就 git pull），
阅读 ~/ai-workflow/skills/agent-session/SKILL.md 并按流程工作。
本次任务：<任务描述>
```

### 3. 让 Claude 审查并合并（审查者）

```
请先更新 ~/ai-workflow（没有就 clone，有就 git pull），
阅读 ~/ai-workflow/skills/pr-review/SKILL.md 并按流程执行：
审查 Casey-sketch-cyber 名下各仓库的未合并 PR 和分支，通过的合并，没通过的退回。
```

这段话可以随时发给 Claude 手动触发，也可以设成 Claude 的定时任务，让它每隔几小时自动跑一次。

### 网页版 AI（不能执行命令的）

把对应 `SKILL.md` 的内容直接粘贴给它，再附上任务描述即可。

## 分支与合并约定

- 干活的 AI（Codex、Cursor、Grok 等）：每个任务一个分支，做完推送并创建 PR。不自己合并，也不用等用户。
- 审查：Claude 按 `skills/pr-review/SKILL.md` 扫描各项目的未合并 PR 和分支，对照项目的 `AGENTS.md` 和任务验收标准审查。通过就合并；没通过就在 PR 里留言，并在该项目 `docs/PROGRESS.md` 的待办里登记。可以设成定时任务，也可以由用户手动触发。**定时任务没有建好之前，PR 不会自动被审查，需要用户对 Claude 说一句"审查一下"。**
- 用户：只处理审查任务明确提出需要用户决定的事。

细则见 `skills/agent-session/SKILL.md` 和 `skills/project-onboarding/templates/AGENTS.md` 的工作规则第 2 条。

## 更新规范

想改规则时，直接改本仓库里的文件并推送。所有 AI 下次 `git pull` 后就会使用新规则。

## 注意

- 本仓库可以设为私有，但 AI 工具需要有权限读取它。用 Codex 云端版时，要在 Codex 里授权它访问这个仓库。
- 模板里的 `gitignore` 文件没有前面的点，是为了防止它在本仓库里生效；复制到项目里时要改名为 `.gitignore`（skill 里已写明）。
