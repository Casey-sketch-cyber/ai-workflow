# ai-workflow：多 AI 协作的共享规范

这个仓库存放所有 AI 工具（Codex、Cursor、Claude、Grok……）共用的工作流程。
每个项目各自是独立的 GitHub 仓库；本仓库只放"怎么干活"的规则，不放项目代码。

## 内容

```
skills/
├── project-onboarding/     # 把一个项目整理干净、补齐文档、上传 GitHub（每个项目做一次）
│   ├── SKILL.md
│   └── templates/          # .gitignore、AGENTS.md、docs/* 模板
└── agent-session/          # 日常接任务的标准流程（每次干活都用）
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

### 网页版 AI（不能执行命令的）

把对应 `SKILL.md` 的内容直接粘贴给它，再附上任务描述即可。

## 分支与合并约定

各项目允许 AI 直接提交 `main`；用分支时由 AI 自行合并，不需要等用户审核。验证没通过或需要用户做决定时，AI 要停下来。细则见 `skills/agent-session/SKILL.md` 和 `skills/project-onboarding/templates/AGENTS.md` 的工作规则第 2 条。

已经上线的项目，其 `AGENTS.md` 是上线时从模板复制的，不会自动更新；需要在各项目里把第 2 条改成模板里的新写法。

## 更新规范

想改规则时，直接改本仓库里的文件并推送。所有 AI 下次 `git pull` 后就会使用新规则。

## 注意

- 本仓库可以设为私有，但 AI 工具需要有权限读取它。用 Codex 云端版时，要在 Codex 里授权它访问这个仓库。
- 模板里的 `gitignore` 文件没有前面的点，是为了防止它在本仓库里生效；复制到项目里时要改名为 `.gitignore`（skill 里已写明）。
