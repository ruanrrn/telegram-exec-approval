# Telegram Exec Approval UI Skill

A reusable OpenClaw skill for implementing and upstreaming Telegram interactive exec approvals.

## What this repo contains

- `telegram-exec-approval-ui/` - the skill source
- `dist/telegram-exec-approval-ui.skill` - packaged skill artifact

## What the skill covers

This skill documents a practical workflow for fixing Telegram exec approvals in OpenClaw, including:

- distinguishing native `approvalId` from short `approvalSlug` surfaces
- adding a Telegram-native runtime approval handler
- sending inline approval buttons bound to the full approval id
- normalizing `telegram:<id>` targets before Bot API delivery
- suppressing duplicate native approval messages after hot restarts
- keeping the custom Telegram approval message minimal when another message already carries the details
- translating local `dist`-level validation into a cleaner upstream PR plan

## Repo layout

```text
telegram-exec-approval-ui-skill/
├── README.md
├── telegram-exec-approval-ui/
│   ├── SKILL.md
│   └── references/
│       ├── pr-draft.md
│       └── upstream-plan.md
└── dist/
    └── telegram-exec-approval-ui.skill
```

## Use the skill locally

If you want the packaged artifact:

1. Take `dist/telegram-exec-approval-ui.skill`
2. Import or distribute it wherever you manage OpenClaw skills

If you want the editable source:

1. Open `telegram-exec-approval-ui/SKILL.md`
2. Read `telegram-exec-approval-ui/references/upstream-plan.md`
3. Read `telegram-exec-approval-ui/references/pr-draft.md`

## Intended outcome

This repo is meant to help turn a locally validated Telegram approval UX fix into a cleaner upstream OpenClaw PR.

## Status

Work in progress. The skill and references are usable now, and the upstream PR plan is included in the repo.
