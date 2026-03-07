# OpenClaw Telegram Exec Approval Skill

Reusable OpenClaw skill for diagnosing, implementing, and upstreaming Telegram interactive exec approvals.

## Repository

- Name: `openclaw-telegram-exec-approval-skill`
- Purpose: package one focused skill around the Telegram exec approval flow instead of burying the workflow in chat logs or one-off local patches
- Audience: OpenClaw users who need to fix broken Telegram approval UX, validate the runtime path locally, or prepare a clean upstream PR

## What is included

- `telegram-exec-approval-ui/` - skill source
- `telegram-exec-approval-ui/references/upstream-plan.md` - source-level patch mapping and cleanup plan
- `telegram-exec-approval-ui/references/pr-draft.md` - draft PR framing and validation notes
- `dist/telegram-exec-approval-ui.skill` - packaged artifact ready to import or share

## Skill scope

The skill covers the practical Telegram approval workflow:

- separate native `approvalId` from short display-only ids
- wire Telegram inline buttons to `/approve <full-id> ...`
- resolve the real Telegram target from the OpenClaw session record
- reduce duplicate approval surfaces after restarts
- keep the Telegram approval message minimal when detail already exists elsewhere
- turn local runtime experiments into a smaller upstreamable patch plan

## Install

Choose one path:

1. Import `dist/telegram-exec-approval-ui.skill` into the target OpenClaw environment.
2. Copy `telegram-exec-approval-ui/` into a skills directory when you want the editable source.

## Use

Trigger the skill when Telegram exec approvals are missing buttons, using the wrong id, duplicating after restart, or when the fix needs to be packaged for reuse or upstream review.

Start with:

1. `telegram-exec-approval-ui/SKILL.md`
2. `telegram-exec-approval-ui/references/upstream-plan.md`
3. `telegram-exec-approval-ui/references/pr-draft.md`

## Repository layout

```text
openclaw-telegram-exec-approval-skill/
├── README.md
├── telegram-exec-approval-ui/
│   ├── SKILL.md
│   └── references/
│       ├── pr-draft.md
│       └── upstream-plan.md
└── dist/
    └── telegram-exec-approval-ui.skill
```

## Publishing notes

- Keep the repository description aligned with the skill trigger language.
- Regenerate `dist/telegram-exec-approval-ui.skill` after each material skill change.
- Keep the repo focused on the skill and its packaged artifact; avoid dumping unrelated debugging scraps into the tree.
