# OpenClaw Telegram Exec Approval Skill

Reusable OpenClaw skill for fixing Telegram interactive exec approvals without turning the debugging process into folklore.

## Why this exists

Telegram approval UX can fail in a few especially annoying ways:

- approval messages show the wrong id
- buttons are missing or bound to the wrong approval target
- duplicate approval messages appear after restart
- local validation works, but the change is too messy to upstream cleanly

This repository packages that workflow into one focused OpenClaw skill so the fix is reusable instead of trapped in one machine's chat history.

## What you get

- `telegram-exec-approval-ui/` - the skill source
- `telegram-exec-approval-ui/references/upstream-plan.md` - how to translate local runtime patching into a source-level fix
- `telegram-exec-approval-ui/references/pr-draft.md` - PR framing and validation notes
- `dist/telegram-exec-approval-ui.skill` - packaged artifact ready to install or share

## Install

Use either path:

1. Import `dist/telegram-exec-approval-ui.skill` into an OpenClaw environment.
2. Copy `telegram-exec-approval-ui/` into your skills directory if you want the editable source.

## When to use the skill

Use it when OpenClaw on Telegram needs any of the following:

- diagnose broken exec approval flows
- repair button-driven approval routing
- distinguish full `approvalId` from short display-only ids
- suppress duplicate native approval messages after restart
- prepare a cleaner upstream patch or PR for the Telegram approval flow

## Skill outcome

The skill walks an agent through a practical sequence:

1. separate the approval surfaces
2. verify the native Telegram approval path
3. patch the smallest wrong user-visible surface first
4. dedupe noisy or repeated approval messages
5. reduce local experiments into a reusable or upstreamable change

## Repository layout

```text
openclaw-telegram-exec-approval-skill/
├── LICENSE
├── README.md
├── telegram-exec-approval-ui/
│   ├── SKILL.md
│   └── references/
│       ├── pr-draft.md
│       └── upstream-plan.md
└── dist/
    └── telegram-exec-approval-ui.skill
```

## Release hygiene

- keep the repository description aligned with the skill trigger language
- regenerate `dist/telegram-exec-approval-ui.skill` after each material skill change
- keep the repo narrow and practical; no unrelated debug junk

## Repository

- GitHub: `https://github.com/ruanrrn/openclaw-telegram-exec-approval-skill`
- License: MIT
