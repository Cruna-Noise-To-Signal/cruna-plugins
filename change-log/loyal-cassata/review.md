# Review — loyal-cassata

Round 1, base 6bae5b4. Result: **PASS** on both axes.

## Specs

VERDICT: PASS. All three features match the spec; issue #1 holds `mirror:repo` (done) and `mirror:deploy-key`, `mirror:secret`, `mirror:first-run`, `mirror:claude-admin` (open), with the key and secret entries saying to finish them before merging into `dev`.

- (a) Missing/partial: the spec wants AGENTS.md to say the mirror "must never be edited"; the paragraph implies it ("read-only copy", "overwrites whatever is there, so every change goes to this repo") without saying it outright. Not blocking.
- Verified: triggers (push `main`/`dev`, `v*` tags, manual dispatch, no `pull_request`); fork guard; single concurrency group with `cancel-in-progress: false`; key only from `MIRROR_DEPLOY_KEY` with a clear error when empty; pinned GitHub host key; force-push of `main`, `dev`, `v*` tags; self-check failing on a `main`/`dev` mismatch; `manual-test.md` covers every "Acceptance by hand" item; no plugin, catalog or README changes, so no version bump.
- (b) Scope creep, harmless: the `mirror:repo` entry adds "issues and wiki off"; the workflow adds `timeout-minutes: 10` and a key-file cleanup step.
- (c) Possibly wrong, not blocking:
  - GitHub keeps only one pending run per concurrency group, so with three quick pushes the middle pending run is cancelled. No state is lost (the run that goes ahead pushes the latest refs), but "never cancelled" in spec and manual-test step 6 isn't literally true.
  - Self-check covers `main`/`dev` only, as the spec says; tags are checked by hand in manual-test step 4.
  - The pinned host key breaks loudly if GitHub rotates it; one-line fix.

## Standards

VERDICT: PASS. No `guidelines` skill; judged against AGENTS.md, `cruna-core:writing-for-agents` and GitHub Actions security practice.

- Conformance: no violations. No plugin change, so no bump; checker passed; change-log layout matches `.github/pm.yml`. The AGENTS.md section is one place for the mirror, records the non-derivable facts (why, never edit), states them positively and names the workflow.
- Actions security: `permissions: contents: read`; no `pull_request`; repository guard; `persist-credentials: false`; secret scoped to one step via `env`; host key pinned with `StrictHostKeyChecking=yes`; key file mode 600 in `$RUNNER_TEMP`, removed by an `always()` step; timeout; clear error on missing secret.
- Advisories (none blocking):
  1. `actions/checkout@v4` pinned by tag, not commit SHA; low risk here (read-only token, secret only in a later step).
  2. Host-key rotation recovery ("update the pinned key from api.github.com/meta") should be noted in the report or pm-setup item.
  3. Possible Speculative Generality: step ids `key`, `push`, `check` are never referenced.
  4. Possible Duplicated Code: `main`/`dev` appear in both the push refspecs and the check loop; the org/repo name in the guard and in `MIRROR`. Borderline for an 83-line file.
  5. Deleted tags/branches aren't propagated: out of scope per spec, noted in report.
- No other baseline smells; each commit is one logical change.
