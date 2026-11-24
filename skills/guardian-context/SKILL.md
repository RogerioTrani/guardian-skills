---
name: guardian-context
description: Preserve the current project session before context compaction or when a user or periodic reminder asks to checkpoint the work. Harness-agnostic; works via a lightweight project checkpoint without storing raw conversation transcripts, or with an explicitly supplied export when requested.
---

# Preserve the current session

Canonical source (mirrored to the public skills repo, #105/#131). Keep this file and `guardian-resume/SKILL.md` and `guardian-claim-recovery/SKILL.md` in sync when the protocol changes.

## 0. Install GuardianContext first

These skills are text only. Before using them, install the GuardianContext package and register the skills globally (see `guardian skills status`; it prints the exact commands). Running `npx skills` alone does not install the client or the backend.

Use this skill when a periodic reminder (`UserPromptSubmit`) or the user asks to checkpoint the current work before context compaction or a session handoff.

## 1. Lightweight Harness-Agnostic Checkpoint (Default — No Transcript Stored)

Record a concise, verified project checkpoint before compacting the context window:

```sh
guardian session-context checkpoint \
  --summary "Goal, settled decisions, constraints, and verified test state" \
  --actor "<DECLARED_ACTOR>" \
  --issue "#<NUMBER>" \
  --claim "<CLAIM_ID>" \
  --next-step "Immediate next action after compaction"
```

- Distinguish verified facts (commits, passing tests, issue numbers, active claim IDs) from open hypotheses.
- Never include credentials, tokens, or unrelated personal material in `--summary`.
- After saving the checkpoint, you can safely let the harness compact the session (`PreCompact` / `SessionStart` will re-inject this checkpoint automatically).

## 2. Explicit Export Synthesis (Optional — Only When User Selects an Export File)

If the user explicitly supplies a conversation export file to archive in detail:

1. Use a valid conversation ID already supplied for this current project session directly, or import the user-selected file:

   ```sh
   guardian conversation import <selected-export.jsonl> --format jsonl
   ```

   For plain text or Markdown, use `--format text`. For a continuation, add `--previous <conversation-id>`.
2. Prepare unsummarized messages with `guardian conversation prepare <id> --pending-only --max-chars 12000`, synthesize only those complete messages into UTF-8 JSON (`summary` and `message_ids`), and record with `guardian conversation summarize <id> <batch-id> --file <summary.json>`.
3. Confirm with `guardian conversation show <id>`.
