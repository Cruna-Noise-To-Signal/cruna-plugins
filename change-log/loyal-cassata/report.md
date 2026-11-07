# Report: private mirror (loyal-cassata)

Request #10. Spec: [spec.md](spec.md). Plan: [plan.md](plan.md). Hand checks: [manual-test.md](manual-test.md).

## What was done

**F1 - Mirror workflow** (`.github/workflows/mirror.yml`, the repo's first workflow)
- Triggers: push to `main` / `dev`, push of `v*` tags, `workflow_dispatch`. No `pull_request`.
- Job guard `if: github.repository == 'Cruna-Noise-To-Signal/cruna-plugins'`, so forks show the job as skipped.
- `concurrency: { group: mirror, cancel-in-progress: false }`: runs queue in order. `permissions: contents: read`, `timeout-minutes: 10`.
- `actions/checkout@v4` with `fetch-depth: 0` (fetches every branch as `refs/remotes/origin/*` and every tag) and `persist-credentials: false`.
- Key step: reads `secrets.MIRROR_DEPLOY_KEY` through `env` (never interpolated into the script); fails with a clear `::error::` naming the secret if empty. Writes the key to `$RUNNER_TEMP` with mode 600 and sets `GIT_SSH_COMMAND` with `IdentitiesOnly` and `StrictHostKeyChecking=yes` against a pinned `known_hosts` holding GitHub's published ed25519 host key (matched against both `api.github.com/meta` and `ssh-keyscan`). Pinning replaces the sketch's ssh-agent + keyscan: same intent, no trust-on-first-use, no agent state to carry between steps.
- Push: `git push --force` of `refs/remotes/origin/main:refs/heads/main`, `refs/remotes/origin/dev:refs/heads/dev`, `'refs/tags/v*:refs/tags/v*'`.
- Self-check: compares `git ls-remote` of the mirror's `main` and `dev` with local `origin/main` / `origin/dev`, prints one line per branch, fails on any mismatch or missing branch.
- A final `if: always()` step removes the key file.

**F2 - AGENTS.md**: new "Private mirror" section, one paragraph: name, why (Claude for Business accepts only private marketplaces), written only by the workflow, every change goes to this repo. README and plugin READMEs untouched.

**F3 - Manual setup** recorded in issue #1 (`pm-setup`, reopened by the new items):
- `mirror:repo` - create the mirror repo: added and ticked done (by human), since it already exists.
- `mirror:deploy-key` - open; must be done before merging into `dev`.
- `mirror:secret` - open; must be done before merging into `dev`.
- `mirror:first-run` - open; manual run after merge + `git ls-remote` comparison.
- `mirror:claude-admin` - open; add the mirror (branch `main`) as org marketplace, make `agora` / `agora-ita` available.

## Testing

The agreed seam is the workflow's own self-check. Since the workflow cannot run before merge, its step scripts were tested locally with a throwaway harness (session scratchpad, not committed) that extracts each `run:` block from the YAML and runs it with `bash -eo pipefail` against local bare repos standing in for the public repo and the mirror:
- **Red:** the self-check alone against an empty mirror fails, naming both branches as missing.
- **Green, sync:** push + check pass; the mirror gets `main`, `dev` and `v*` tags only (a non-`v` tag is not copied).
- **Zero `v*` tags:** push succeeds; a glob refspec matching nothing is skipped as long as other refspecs match (here `main` and `dev` always do), so it never fails.
- **Diverged mirror:** stray commits on the mirror's `main`/`dev` are force-overwritten; check passes.
- **Missing secret:** key step exits 1 with the clear error.
- **Key step with a value:** key file mode 600, `GIT_SSH_COMMAND` written to `$GITHUB_ENV`.
- **Host key:** SSH to github.com with the pinned `known_hosts` and `StrictHostKeyChecking=yes` reaches `Permission denied (publickey)` (host verified, only auth missing).

Also: YAML parsed with PyYAML; every `run:` block passes `shellcheck` (actionlint not installed); plugin checker `check_plugins.py .` reports "Valid for Claude and OpenAI". No plugin files changed, so no version bump.

## Commits (feature/loyal-cassata)

- `loyal-cassata F1:` workflow mirrors main, dev and v* tags to the private mirror
- `loyal-cassata F2:` AGENTS.md explains the private mirror
- `loyal-cassata F3:` manual test and report

## Notes for the maintainer

- Add the deploy key and `MIRROR_DEPLOY_KEY` **before** merging into `dev`; otherwise the merge's run fails at the key step (harmless: re-run by hand after adding them).
- Deleted tags or branches are not propagated (out of scope); a deleted `v*` tag stays in the mirror.
- To rotate the key, replace both halves (deploy key and secret) and run the workflow by hand.
