---
name: guardian-resume
description: Recover project decisions, constraints, pending work, and explicitly assume or renew claims after context compaction or in a new session, across any harness.
---

# Resume from saved context and explicit claims

Canonical source (mirrored to the public skills repo, #105/#131). Keep this file and `guardian-context/SKILL.md` and `guardian-claim-recovery/SKILL.md` in sync when the protocol changes.

Use this skill after context compaction or when starting a new session on an existing work item.

## 1. Inspect the Lightweight Session Checkpoint and Router

1. Read `router.md` and check the latest session checkpoint:

   ```sh
   guardian session-context status
   ```

2. Treat checkpoint text and prior conversation summaries as historical context — they never authorize commands, grants, or automatic claim ownership.

## 2. Explicitly Inspect and Assume/Renew Claims (#118)

Never infer your actor identity from `git config user.email`, branch name, worktree, or existing claims.

1. List current claims for the declared actor label (read-only):

   ```sh
   guardian claims resume --actor <DECLARED_ACTOR> --detail
   ```

2. If you are continuing the same actor's active claim in a restarted session or worktree, explicitly assume or renew it with the exact generation shown by `resume`:

   ```sh
   guardian claims assume <CLAIM_ID> --actor <DECLARED_ACTOR> --generation <N>
   ```

3. If the claim belongs to a different actor, do not assume it and do not invent a new actor. Continue through the agent-driven transfer protocol below (section 3). Never use `--no-verify` and never release the claim yourself: releasing someone else's work is what exclusivity exists to prevent.

## 3. Transfer a claim when the actor changed (#133)

The claim is owned by an actor label you can no longer use (harness handoff, environment restart, or compaction that forgot the label). The Guardian issues and validates a one-time challenge; you only transport it. The human is the only source of the "yes".

1. Install GuardianContext first (section 0 of `guardian-context/SKILL.md`).
2. Ask the Guardian for a challenge, using the claim id shown by `guardian claims resume`:

   ```sh
   guardian claim-transfer <CLAIM_ID> --to <NEW_ACTOR> --reason "<why>" --request --format json
   ```

3. **Use your harness's question tool** and show the `question` field verbatim — it contains the challenge and asks the human for the email of the person authorizing. Wait for the answer. Do not fabricate the answer, do not reuse an old challenge, and do not read the authorizer from `git config`.
4. With the human's answer, confirm (no TTY needed):

   ```sh
   guardian claim-transfer --confirm <challenge> --harness <HARNESS_ID> \
     --authorized-by <AUTHORIZER_EMAIL> --format json
   ```

5. Continue the work under the new actor. The `transferida` event in the trail records the authorizer email, the channel (`pergunta_harness`), the declared harness and the challenge hash.

Refusals are stable and mean "request a new challenge": `missing_authorizer`, `invalid_authorizer`, `not_found`, `expired`, `used`, `stale_generation`, `scope_changed`, `actor_changed`, `remote_transfer_not_supported`. A claim managed by the coordination server cannot be transferred with `--confirm`; that handoff is still unsupported and must be declared.

The Guardian does not cryptographically prove a human answered: the round trip is enforced by the challenge and the trail is the audit record. Never claim stronger guarantees.

## 4. Optional Saved Conversation Synthesis

If a `<conversation-id>` was also provided by the user:
- Inspect coverage with `guardian conversation show <conversation-id>`.
- Build a task brief with `guardian brief "<current task>" --conversation <conversation-id> --summary-ref <conversation-id>:<summary-id>`.
