# Telegram Exec Approval

[English](README.md) | 简体中文

![Telegram Exec Approval banner](assets/social-preview.svg)

![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-1F2937?style=flat-square)
![Focus-Telegram Approval UI](https://img.shields.io/badge/Focus-Telegram%20Approval%20UI-10B981?style=flat-square&labelColor=1F2937)
![Works-Standalone](https://img.shields.io/badge/Works-Standalone-F9FAFB?style=flat-square&labelColor=374151)
![Artifact-.skill Included](https://img.shields.io/badge/Artifact-.skill%20Included-D1FAE5?style=flat-square&labelColor=374151)
![README-Bilingual](https://img.shields.io/badge/README-Bilingual-F9FAFB?style=flat-square&labelColor=065F46)
![License-MIT](https://img.shields.io/badge/License-MIT-F9FAFB?style=flat-square&labelColor=1F2937)

一个专门收拾 Telegram 交互式 exec 审批问题的 OpenClaw skill：把按钮、ID、去重和上游交付这条线做清楚，别再靠口口相传修锅。

## Quick pitch

修好坏掉的 Telegram 审批按钮、错误 ID、重复消息，以及上游交付路径。
先让原生审批链路正常，再把修复整理成能复用的东西。

## Why this exists

Telegram 审批 UX 最烦人的地方，不是单点 bug，而是一串破事会一起出现：

- 审批消息显示的是错的 ID
- 按钮缺失，或者点了之后走到错的审批目标
- 重启后重复发审批消息
- 本地明明修好了，但最后没人把修复整理成可复用包或可提交 PR 的变更

这种组合拳最容易把问题变成“只有老员工才知道怎么修”的部落传说。

`telegram-exec-approval` 就是拿来结束这种烂传统的。

它把这条工作流打包成一个明确的技能：

- 先分清不同的审批展示面
- 端到端验证 Telegram 原生审批路径
- 修正错误 ID、缺失按钮或误导性按钮行为
- 压住重复审批消息
- 把本地修复收敛成可复用或可上游化的改动

## Works independently

`telegram-exec-approval` 可以单独使用。

即使你没上别的 Telegram 调试或 continuity skill，它本身也已经能改善这些问题：

- 审批 ID 正确性
- inline button 路由
- 重复审批抑制
- Telegram 原生路径验证纪律
- 可复用打包与上游交付

相关仓可能有帮助，但不是这个仓成立的前提。

## What the skill teaches

这个 skill 会要求代理：

- 在补丁之前先拆清审批展示面
- 把完整 `approvalId` 当成唯一权威，而不是相信展示用短 ID
- 先让 Telegram 原生审批路径端到端可用
- 优先修最小的错误用户可见面，而不是一上来乱扩散
- 当重复消息出现时，对同一个 `approvalId` 做短时间窗口去重
- 如果另一条消息已经带了详细正文，就把 Telegram 按钮消息保持得尽量简短
- 把本地 runtime 修复收敛成最小的可复用或可上游提交改动

## When to use it

适合这些场景：

- 排查 Telegram exec 审批流为什么坏了
- 修复按钮驱动的审批路由
- 区分完整 `approvalId` 和展示用短 ID
- 压制重启后的重复原生审批消息
- 准备更干净的上游 patch 或 PR

## Example behavior

### Example 1: 可见 ID 不对

Telegram 审批消息里显示的是短 ID，但 `/approve` 只认更长的内部 ID。

靠谱的代理应该：

1. 先确认到底是哪一个展示面在显示错误 ID
2. 把完整 `approvalId` 当成权威值
3. 只补最小但仍然泄漏错误 ID 的那一层可见面
4. 用新的审批请求重新验证

### Example 2: 按钮有了，但路由是歪的

Telegram 里出现了按钮，但点下去并不能解析到正确审批。

靠谱的代理应该：

1. 先证明普通 Telegram inline button 本身能工作
2. 记录并比对实际发出的 `approvalId`
3. 确认按钮消息和 assistant 侧 pending reply 指向的是同一个完整 ID
4. 先修 Telegram handler，再去碰更广的显示层

### Example 3: 本地修好了，但上游方案一团糟

本地 runtime patch 已经能修复问题，但源码级改动仍然没整理清楚。

靠谱的代理应该：

1. 去掉临时调试垃圾
2. 把有效修复收敛成最小、完整、可解释的改动
3. 使用仓内 reference 来组织 PR 说明和上游映射
4. 把结果打包好，确保不只在这台机器上能复用

## Related skills

这些是相关项，不是依赖项：

- `restart-continuity`：当审批 UX bug 跟重启行为缠在一起时很有用 —— <https://github.com/ruanrrn/restart-continuity>
- `multi-task-continuity`：当审批修复是更长多任务流程的一部分时很有用 —— <https://github.com/ruanrrn/multi-task-continuity>

如果核心问题就是 Telegram exec 审批，用这个仓单独上就够了。

## Social preview

建议 social preview 资源：`assets/social-preview.svg`

建议一句话文案：

> Fix broken Telegram approval buttons, IDs, duplicates, and upstream handoff.

GitHub 备注：

- 当前公开的 `gh` CLI 和 GraphQL `UpdateRepositoryInput` 都没有可写的 custom social preview 字段。
- 如果要把这张图真正设成仓库 social preview，只能去 GitHub 仓库设置页手动上传 `assets/social-preview.svg`。

## What you get

- `telegram-exec-approval-ui/` - skill 源码
- `telegram-exec-approval-ui/references/upstream-plan.md` - 把本地 runtime patch 收敛成源码级修复的路径说明
- `telegram-exec-approval-ui/references/pr-draft.md` - PR 说明框架和验证备注
- `dist/telegram-exec-approval-ui.skill` - 可直接安装或分享的打包产物

## Install

两种方式都可以：

1. 直接导入 `dist/telegram-exec-approval-ui.skill`
2. 把 `telegram-exec-approval-ui/` 复制到你的 skills 目录，按源码方式使用

## Repository layout

```text
telegram-exec-approval/
├── LICENSE
├── README.md
├── README.zh-CN.md
├── CONTRIBUTING.md
├── assets/
│   └── social-preview.svg
├── telegram-exec-approval-ui/
│   ├── SKILL.md
│   └── references/
│       ├── pr-draft.md
│       └── upstream-plan.md
└── dist/
    └── telegram-exec-approval-ui.skill
```

## Contributing

见 `CONTRIBUTING.md`，里面写了贡献范围、PR 预期，以及怎样避免把这个仓做成 Telegram 万能杂物间。

## Release hygiene

- 仓库描述要和 skill 的触发语义保持一致
- 每次 skill 有实质改动后，都要重新生成 `dist/telegram-exec-approval-ui.skill`
- 保持仓库克制、可复用，不要混进无关调试垃圾
- 保证可复用说明和上游交付说明始终跟实际修复路径一致

## Repository

- GitHub: `https://github.com/ruanrrn/telegram-exec-approval`
- License: MIT
