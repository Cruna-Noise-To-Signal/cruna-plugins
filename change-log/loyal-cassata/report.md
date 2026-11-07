# Report: private mirror (loyal-cassata)

Request #10. Spec: [spec.md](spec.md). Plan: [plan.md](plan.md). Hand checks: [manual-test.md](manual-test.md).

## What was done

**F1 - Mirror workflow** (`.github/workflows/mirror.yml`, the repo's first workflow)
- Triggers: push to `main` / `dev`, push of `v*` tags, `workflow_dispatch`. No `pull_request`.
- Job guard `if: github.repository == 'Cruna-Noise-To-Signal/cruna-plugins'`, so forks show the job as skipped.
- `concurrency: { group: mirror, cancel-in-progress: false }`: runs queue in order. `permissions: contents: read`, `timeout-minutes: 10`.
- `actions/checkout@v4` with `fetch-depth: 0` (fetches every branch as `refs/remotes/origin/*` and every tag) and `persist-credentials: false`.
- Auth (round 2): the org policy disables deploy keys, so the workflow authenticates as an org GitHub App ("Cruna plugins mirror", Contents read/write, installed on the mirror only). A first step fails with a clear `::error::` when the `MIRROR_APP_ID` variable or `MIRROR_APP_PRIVATE_KEY` secret is missing; `actions/create-github-app-token@v1` then mints a one-hour token limited to `cruna-plugins-mirror`, revoked when the job ends. Push and check go over HTTPS with that token. (Round 1 used an SSH deploy key with a pinned GitHub host key; that code is gone.)
- Push: `git push --force` of `refs/remotes/origin/main:refs/heads/main`, `refs/remotes/origin/dev:refs/heads/dev`, `'refs/tags/v*:refs/tags/v*'`.
- Self-check: compares `git ls-remote` of the mirror's `main` and `dev` with local `origin/main` / `origin/dev`, prints one line per branch, fails on any mismatch or missing branch.
- Push and self-check share one step, so the mirror URL is defined once.

**F2 - AGENTS.md**: new "Private mirror" section, one paragraph: name, why (Claude for Business accepts only private marketplaces), written only by the workflow, every change goes to this repo. README and plugin READMEs untouched.

**F3 - Manual setup** recorded in issue #1 (`pm-setup`, reopened by the new items):
- `mirror:repo` - create the mirror repo: added and ticked done (by human), since it already exists.
- `mirror:app` - open; create the GitHub App and install it on the mirror only; before merging into `dev`.
- `mirror:secret` - open; `MIRROR_APP_ID` variable and `MIRROR_APP_PRIVATE_KEY` secret; before merging into `dev`.
- `mirror:deploy-key` - dropped (org policy disables deploy keys).
- `mirror:first-run` - open; manual run after merge + `git ls-remote` comparison.
- `mirror:claude-admin` - open; add the mirror (branch `main`) as org marketplace, make `agora` / `agora-ita` available.

## Testing

The agreed seam is the workflow's own self-check. Since the workflow cannot run before merge, its step scripts were tested locally with a throwaway harness (session scratchpad, not committed) that extracts each `run:` block from the YAML and runs it with `bash -eo pipefail` against local bare repos standing in for the public repo and the mirror:
- **Red:** the self-check alone against an empty mirror fails, naming both branches as missing.
- **Green, sync:** push + check pass; the mirror gets `main`, `dev` and `v*` tags only (a non-`v` tag is not copied).
- **Zero `v*` tags:** push succeeds; a glob refspec matching nothing is skipped as long as other refspecs match (here `main` and `dev` always do), so it never fails.
- **Diverged mirror:** stray commits on the mirror's `main`/`dev` are force-overwritten; check passes.
- **Missing settings:** the first step exits 1 with the clear error (round 2).
- Round 1 also tested the SSH key step and pinned host key; both were replaced in round 2.

Also: YAML parsed with PyYAML; every `run:` block passes `shellcheck` (actionlint not installed); plugin checker `check_plugins.py .` reports "Valid for Claude and OpenAI". No plugin files changed, so no version bump.

## Commits (feature/loyal-cassata)

- `loyal-cassata F1:` workflow mirrors main, dev and v* tags to the private mirror
- `loyal-cassata F2:` AGENTS.md explains the private mirror
- `loyal-cassata F3:` manual test and report

## Notes for the maintainer

- Create and install the App and add `MIRROR_APP_ID` / `MIRROR_APP_PRIVATE_KEY` **before** merging into `dev`; otherwise the merge's run fails at the first step (harmless: re-run by hand after adding them).
- Deleted tags or branches are not propagated (out of scope); a deleted `v*` tag stays in the mirror.
- To rotate the App key, generate a new private key in the App settings, replace the secret, delete the old key, and run the workflow by hand.
