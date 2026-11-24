---
name: guardian-claim-recovery
description: Recover, transfer, or administratively abandon Guardian claims after a harness handoff, and choose the team policy when the coordination database is unavailable.
---

# Recover claims and transfer ownership

Canonical source (mirrored to the public skills repo, #105/#131). Keep this file and `guardian-context/SKILL.md` and `guardian-resume/SKILL.md` in sync when the protocol changes.

## 0. Install GuardianContext first

These skills are text only. Before using them, install the GuardianContext package and register the skills globally (see `guardian skills status`; it prints the exact commands). Running `npx skills` alone does not install the client or the backend.

## 1. Resume a claim (retomada)

Never infer your actor identity from `git config user.email`, branch name, worktree, or existing claims.

1. List current claims for the declared actor label (read-only):

   ```sh
   guardian claims resume --actor <DECLARED_ACTOR> --detail
   ```

2. If you are continuing the same actor's active claim in a restarted session or worktree, explicitly assume or renew it with the exact generation shown by `resume`:

   ```sh
   guardian claims assume <CLAIM_ID> --actor <DECLARED_ACTOR> --generation <N>
   ```

3. If the claim belongs to a different actor, do not assume it and do not invent a new actor. Continue through the transfer protocol in section 2. Never use `--no-verify` and never release someone else's claim yourself.

## 2. Transfer a claim when the actor changed (#133)

The claim is owned by an actor label you can no longer use (harness handoff, environment restart, or compaction that forgot the label). The Guardian issues and validates a one-time challenge; you only transport it. The human is the only source of the "yes".

1. Ask the Guardian for a challenge, using the claim id shown by `guardian claims resume`:

   ```sh
   guardian claim-transfer <CLAIM_ID> --to <NEW_ACTOR> --reason "<why>" --request --format json
   ```

2. **Use your harness's question tool** and show the `question` field verbatim — it contains the challenge (prefixed `gc1.`) and asks the human for the email of the person authorizing. Wait for the answer. Do not fabricate the answer, do not reuse an old challenge, and do not read the authorizer from `git config`.

3. With the human's answer, confirm (no TTY needed):

   ```sh
   guardian claim-transfer --confirm <challenge> --harness <HARNESS_ID> \
     --authorized-by <AUTHORIZER_EMAIL> --format json
   ```

4. Continue the work under the new actor. The `transferida` event in the trail records the authorizer email, the channel (`pergunta_harness`), the declared harness and the challenge hash.

Refusals are stable and mean "request a new challenge": `missing_authorizer`, `invalid_authorizer`, `not_found`, `expired`, `used`, `stale_generation`, `scope_changed`, `actor_changed`, `remote_transfer_not_supported`. A claim managed by the coordination server cannot be transferred with `--confirm`; that handoff is still unsupported and must be declared.

The Guardian does not cryptographically prove a human answered: the round trip is enforced by the challenge and the trail is the audit record. Never claim stronger guarantees.

## 3. Abandon a claim administratively (`guardian claims abandon`)

Use this only when a claim is stuck and the repository owner authorizes its administrative closure. It is a local act: claims managed by the coordination server are refused (`remote_transfer_not_supported` / "só local").

```sh
guardian claims abandon <CLAIM_ID> \
  --authorized-by <AUTHORIZER_EMAIL> --reason "<why>"
```

- `--authorized-by` is the declared email of the human authorizing (unauthenticated metadata, never the actor).
- `--reason` is required and recorded in the claim mirror.
- The original owner is read from `find_active` and used to release the claim; `authorized_by` is audit metadata only.
- A single `liberado` event is recorded with outcome `abandonado`; the mirror receives the audit fields.
- Remote-managed claims cannot be abandoned this way; declare the handoff instead.

## 4. When the coordination database is unavailable

`claims.on_unavailable` defaults to `block`: with the database unreachable, `enforce`/`strict` block `commit` and `verify --scope staged`. This protects exclusivity for live claims.

The team may document `claims.on_unavailable: warn` in `guardian.json` as an alternative when the database does not respond. In that mode the check continues with the known claims and reports `claims_unverified`; known conflicts and credential checks still apply. `observe` and diagnostics warn that the read was incomplete. This is documented, not implemented as a new feature — set it with `guardian claim-policy set-unavailable warn --equipe`.

Local write-protection tools use a separate preference, `claims_on_unavailable` (default `warn`), applied only when claim protection is active; it never overrides the team policy. Neither preference changes the default `block`.

Do not change `GUARDIAN_COORDINATION_ENABLED` or disable the timer to work around an unavailable database.
