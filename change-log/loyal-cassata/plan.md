# IMPLEMENTATION PLAN — Private mirror of cruna-plugins

Keep a private, automatically synced copy of this repository so the Cruna Claude organization can install its own plugins. What and why: [spec.md](spec.md).

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

The repository has no `.github/workflows/` yet; this adds the first workflow. Both catalogs (`.claude-plugin/marketplace.json`, `.agents/plugins/marketplace.json`) use relative `./cruna-plugin-*` sources and need no change. No plugin files change, so no version bump. `AGENTS.md` gains one paragraph. Everything else is manual setup outside the repository (GitHub org, repo secret, Claude admin).

---

## Feature backlog

Guidelines: Hard dependencies noted; otherwise parallelizable.

---

### F1 - Mirror workflow
**Capability:** The sync workflow in the public repository: triggers, fork guard, ordering, force-push of `main`, `dev` and `v*` tags, and the self-check (spec: Implementation Decisions, Testing Decisions).
**Acceptance:**
- Triggers only on push to `main`, push to `dev`, push of a `v*` tag and manual dispatch; no `pull_request` trigger.
- Skips when the repository is not `Cruna-Noise-To-Signal/cruna-plugins`.
- One sync at a time, queued, never cancelled.
- Authenticates as the mirror GitHub App (`MIRROR_APP_ID` variable, `MIRROR_APP_PRIVATE_KEY` secret) with a token limited to the mirror; pushes over HTTPS. (Changed from a deploy key in review round 2: the org policy disables deploy keys.)
- Force-pushes `main`, `dev` and all `v*` tags to `Cruna-Noise-To-Signal/cruna-plugins-mirror`.
- Fails the run when the mirror's `main` or `dev` commit differs from the public one after the push.
- Fails with a clear message when the secret is missing.
**Deps:** none to write; needs F3 done to run.
**Touchpoints:** GitHub Actions on the public repository; the mirror repository.

---

### F2 - AGENTS.md paragraph
**Capability:** One paragraph in `AGENTS.md` on the mirror (spec: Documentation decision).
**Acceptance:**
- Says the mirror exists, its name, why (Claude for Business accepts only private marketplaces), that only the workflow writes to it, and that it must never be edited.
- Follows `cruna-core:writing-for-agents`.
- Public README and plugin READMEs are unchanged.
**Deps:** none.
**Touchpoints:** none.

---

### F3 - Manual setup recorded
**Capability:** The one-time manual steps from the spec recorded in the `pm-setup` issue with `pm manual add`, one key each (spec: Manual setup).
**Acceptance:**
- Five entries: create mirror repo; GitHub App created and installed on the mirror only; `MIRROR_APP_ID` variable and `MIRROR_APP_PRIVATE_KEY` secret on the public repo; first manual run after the workflow reaches `dev`; mirror added as org marketplace in the Claude admin.
- Each entry is self-contained (exact repo name, settings, secret name).
- `manual-test.md` in the change-log folder lists the acceptance checks from spec Testing Decisions.
**Deps:** none.
**Touchpoints:** the `pm-setup` issue (#1).

---

## Milestones (each demoable)

- **M1-ready-to-merge:** F1 + F2 + F3 — workflow, docs and recorded manual steps on the feature branch; the plugin checker passes; the workflow file is reviewed for the fork guard and triggers.
- **M2-mirror-live:** after the squash merge to `dev` and the human's manual steps — a manual run seeds the mirror, the self-check passes, `git ls-remote` shows matching refs, and the Claude admin installs `agora` from the mirror.

## Sequencing

F1, F2 and F3 are independent. M2 needs M1 merged into `dev` (workflow present on the branch that triggers it) and the manual steps from F3 done by a human; the mirror's `main` fills on the next release, or on the first manual run, which copies both branches.

## Risks / watch-items

- The workflow cannot run on the feature branch (it triggers only on `main`/`dev`), so the first real run is after the merge to `dev`; review the file carefully before merging.
- Before the first release after the merge, the mirror's `main` is only as current as the public `main`, which is fine: the first manual run copies it.
- Deleted tags or branches are not propagated (out of scope); a deleted `v*` tag stays in the mirror.
- A workflow in the public repository is visible to everyone; it contains no secret, only the mirror's name.
- The App can write only to the mirror; if its private key is ever exposed, generate a new one in the App settings, replace the secret, and delete the old key.
