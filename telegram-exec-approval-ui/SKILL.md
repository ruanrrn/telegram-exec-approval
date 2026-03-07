---
name: telegram-exec-approval-ui
description: Add or repair Telegram interactive exec approvals in OpenClaw. Use when Telegram should show approval buttons for exec requests, approval messages use the wrong id, duplicate approval messages appear after restarts, or the feature needs to be organized into a reusable patch/PR workflow.
---

# Telegram Exec Approval UI

Implement Telegram-native exec approvals in two phases: prove the behavior locally, then prepare the smallest source-level change for an upstream PR.

## Workflow

1. Split the approval surfaces before changing code.
2. Make the native Telegram approval path work end-to-end.
3. Remove duplicate or misleading approval surfaces.
4. Package the learned workflow so it can be reused or proposed upstream.

## Step 1: Split the approval surfaces

Identify which approval UI the user is seeing.

- OpenClaw native approval uses the full `approvalId` and is the one `/approve <id> allow-once|allow-always|deny` must resolve.
- A short 8-character id usually comes from an `approvalSlug` display surface.
- Never assume the first visible approval message is the correct one.

Check these separately:

- Assistant-facing pending reply text
- Telegram-native button message
- Any extra gateway/harness approval message

## Step 2: Make the native Telegram approval path work

Prefer a Telegram runtime-side handler over text-only forwarding.

Core requirements:

- Subscribe to `exec.approval.requested`
- Resolve the Telegram chat target from the session record
- Normalize `telegram:<id>` into a real Bot API chat id
- Send Telegram inline buttons that call `/approve <full-approval-id> ...`

Useful local validation:

- Verify ordinary Telegram inline buttons work first
- Log native approval events and the exact `approvalId`
- Confirm the Telegram button message and the pending reply reference the same full UUID

## Step 3: Fix the common failure modes

### Wrong id shown

Patch any remaining display surface that uses `approvalSlug` instead of `approvalId`.

Common local patch targets in this workspace have included:

- `dist/reply-DhtejUNZ.js`
- `dist/pi-embedded-CtM2Mrrj.js`
- `dist/pi-embedded-DgYXShcG.js`
- `dist/plugin-sdk/reply-DFFRlayb.js`
- `dist/subagent-registry-CkqrXKq4.js`

When testing locally, patch the smallest visible surface first, then retest with a fresh approval.

### Duplicate approval messages after restart

Treat the Telegram approval handler as a singleton per account and suppress duplicate `approvalId` processing for a short time window.

### Too much repeated text

If the gateway already sends the detailed approval message, keep the custom Telegram button message minimal. A short action label plus buttons is enough.

## Step 4: Prepare the PR-minded version

Once the local behavior works, reduce the fix into the smallest coherent upstream change.

Prefer these principles:

- Keep one authoritative `approvalId`
- Avoid separate Telegram-only ids
- Keep the runtime-specific Telegram button sender small
- Do not duplicate approval details across multiple Telegram messages unless necessary
- Preserve `/approve <id> ...` as the fallback path

For upstream prep:

- Record which local dist patches were necessary
- Map those back to the logical source areas they represent
- Separate behavior fixes from debug logging before proposing a PR
- Read `references/upstream-plan.md` to collapse local dist experiments into a clean source-level patch plan
- Read `references/pr-draft.md` when preparing the PR title, summary, and validation notes

## Validation checklist

Do not call the feature done until all are true:

- Telegram approval messages show buttons
- Tapping a button resolves the correct full `approvalId`
- The assistant-facing pending reply also shows the full `approvalId`
- Duplicate native approval messages are suppressed
- The custom Telegram button message does not repeat unnecessary approval details
- After issuing an approval request, the assistant waits for the user approval action to complete before sending another routine chat reply, so execution can continue cleanly on approval-driven surfaces

## Packaging rule

If the feature is likely to be reused, package this skill and keep the task state in `TODO.md` and `memory/active-task.md` aligned while the patch is still in flight.
