Request: #10 (loyal-cassata)

## Problem Statement

Cruna publishes its plugins (today `agora` and `agora-ita`) from this public repository so that clients, anyone who finds them useful and the Cruna team can all install them. The Cruna organization on Claude for Business cannot add a marketplace from a public repository; the admin asks for a private one and suggests forking. GitHub does not allow a private fork of a public repository, and making this repository private would end its purpose as Cruna's open-source shelf. So today the Cruna organization cannot install the plugins Cruna itself builds.

## Solution

A private repository in the same GitHub organization, `Cruna-Noise-To-Signal/cruna-plugins-mirror`, holds an exact, automatically maintained copy of this repository's `main` and `dev` branches and its `v*` release tags. Every push to `main`, `dev` or a `v*` tag in the public repository copies that state to the mirror within seconds. Nobody ever edits the mirror; all work stays in the public repository and the existing workflow (feature PR → `dev` → release to `main`) does not change.

The Cruna organization's Claude admin adds the mirror as its marketplace and installs from its `main`, so the organization gets only released plugin versions, exactly what clients get from the public repository. The plugins, their manifests and both marketplace catalogs are untouched: they are agnostic to where they are read from.

A visual walk-through of the workflow is in the proposal artifact: https://claude.ai/artifact/TPvtGcaUB4PRC8JvWG6MSx

## User Stories

1. As a Cruna team member using Claude Desktop in the Cruna organization, I want to install `agora` or `agora-ita` from the organization's plugin settings, so that I use the same plugins Cruna offers to clients.
2. As a Cruna team member who only uses the plugins, I want the organization to get only released versions, so that work in progress on `dev` never breaks my Agora.
3. As the Cruna Claude admin, I want to add a private GitHub repository as the organization marketplace, so that the Business account's restriction on public marketplaces is respected.
4. As the Cruna Claude admin, I want the marketplace name and plugin names from the mirror to be the same as from the public repository (`cruna-plugins`, `agora@cruna-plugins`, `agora-ita@cruna-plugins`), so that instructions written for clients also work for the team.
5. As a maintainer, I want to keep working only in the public repository, so that I never have to remember a second place to push.
6. As a maintainer, I want a release to `main` to reach the organization without any extra step, so that releasing stays one action.
7. As a maintainer, I want the mirror's `dev` to follow the public `dev`, so that the organization could test unreleased versions from the mirror later without new plumbing.
8. As a maintainer, I want release tags copied to the mirror, so that anyone can see which release the organization runs.
9. As a maintainer, I want the mirror to be overwritten to match the public repository on every sync, so that a history rewrite or a stray edit in the mirror never leaves the two diverging.
10. As a maintainer, I want feature branches, issues, PRs and the Project to stay in the public repository only, so that the mirror carries nothing but what installation needs.
11. As a maintainer, I want a manual "resync now" button, so that I can seed the mirror the first time and repair it after a failure.
12. As a maintainer, I want the sync to fail loudly when the mirror does not end up matching the public repository, so that a broken sync is never silent.
13. As a maintainer, I want the sync never to run on forks or on pull requests, so that the secret that writes to the mirror cannot leak and forks of this public repository do nothing.
14. As a maintainer, I want only the sync to be able to write to the mirror, so that nobody edits it by accident.
15. As a client or member of the public, I want the public repository, its README and the plugin READMEs to stay addressed to me with no internal detail, so that the mirror is invisible to me.
16. As a maintainer or agent working in this repository, I want `AGENTS.md` to explain the mirror in one paragraph, so that nobody tries to change the mirror directly or wonders why it exists.
17. As a maintainer, I want the one-time manual setup steps recorded in the `pm-setup` issue, so that they are on record and can be repeated if the mirror is ever recreated.
18. As a maintainer testing `dev`, I want to keep testing exactly as today (Claude Code reading `dev` from the public repository, Claude Desktop with a zipped upload), so that the mirror changes nothing about testing.
19. As a ChatGPT user, I want nothing to change for me, so that the mirror affects only the Claude organization.

## Implementation Decisions

- **Mirror repository:** `Cruna-Noise-To-Signal/cruna-plugins-mirror`, private, default branch `main`, description "Read-only mirror of cruna-plugins. Do not edit; changes go to the public repo." Organization members get read access only.
- **What is mirrored:** `refs/heads/main`, `refs/heads/dev` and every `refs/tags/v*`, with full history. Nothing else (no feature branches, issues, PRs, Project, Actions history or settings).
- **Direction: the public repository pushes.** A GitHub Actions workflow in the public repository runs on push to `main`, on push to `dev`, on push of a `v*` tag, and on manual dispatch. It checks out the full history and force-pushes the three ref sets to the mirror. A pull job inside the mirror was rejected: scheduled workflows run from the default branch, which each sync overwrites with the public `main`, so the job would delete itself, and it would lag on a timer.
- **Authentication:** a GitHub App owned by the organization, "Cruna plugins mirror", with only the Contents and Workflows (read and write) repository permissions (Workflows because GitHub refuses an App push of commits that touch `.github/workflows/` without it), installed on the mirror only. Its App ID is an Actions variable `MIRROR_APP_ID` and its private key an Actions secret `MIRROR_APP_PRIVATE_KEY` in the public repository; each run mints a one-hour token limited to the mirror and revokes it at the end. Deploy keys were the first choice, but the organization's policy disables them, and GitHub recommends Apps instead; a personal access token was rejected because it belongs to one person and expires. The workflow only triggers on `push` and manual dispatch (never `pull_request`), and a guard restricts it to the repository `Cruna-Noise-To-Signal/cruna-plugins`, so forks never run it and the secret is never exposed to contributed code.
- **Ordering:** the workflow runs one sync at a time (a single concurrency group, queued, not cancelled), so a quick `dev` push followed by a release lands in order.
- **Self-check:** after pushing, the workflow compares the mirror's `main` and `dev` commit ids with the public ones and fails the run if they differ. This is the feature's single test seam.
- **Force-push:** every sync overwrites the mirror's refs, so the mirror always equals the public repository.
- **Plugins stay agnostic:** no plugin, manifest or catalog changes. Both catalogs use relative plugin sources, which resolve inside whichever repository they are read from, and the marketplace name `cruna-plugins` comes from the catalog, so it is identical from either repository. Links in plugin READMEs and skills keep pointing at the public repository, which is correct: it is the public home of the docs. No plugin version bump, because no plugin changes.
- **Organization marketplace reads `main`.** The Cruna organization includes people who only use the plugins, so it must get released versions only. `dev` is mirrored but nobody in the organization reads it for now.
- **Documentation:** one paragraph in `AGENTS.md` (layout or a new "Private mirror" section) saying the mirror exists, why, that it is written only by the workflow, and that it must never be edited. The public README and the plugin READMEs do not mention it.
- **Manual setup** (recorded with `pm manual add` in the `pm-setup` issue): create the mirror repository; create the GitHub App and install it on the mirror only; add its App ID as the `MIRROR_APP_ID` variable and its private key as the `MIRROR_APP_PRIVATE_KEY` secret on the public repository; after the workflow reaches `dev`, run it once by hand to seed the mirror; in the Claude admin, add the mirror as the organization marketplace and make `agora` and `agora-ita` available.

## Testing Decisions

- A good test checks external behaviour only: after a sync, the mirror's `main`, `dev` and `v*` tags point at the same commits as the public repository's. How the copy happens is not tested.
- **The workflow's own self-check** is the automated test: it fails the run when the mirror's `main` or `dev` differ from the public ones.
- **Acceptance by hand** (written into `manual-test.md` by the implementation): seed the mirror with a manual run and compare refs with `git ls-remote` on both repositories; push a change to `dev` and see the mirror's `dev` follow; after the next release, see the mirror's `main` and the new tag follow; confirm a fork of the public repository does not run the workflow; add the mirror in the Claude admin and install `agora` from it.
- **Prior art:** none in this repository (there are no workflows yet); the plugin checker from `cruna-core:portable-plugins` must still pass, since nothing about the plugins changes.

## Out of Scope

- Pointing the organization marketplace at `dev`, or any change to how `dev` is tested (Claude Code against the public repository, zipped upload in Claude Desktop).
- Mirroring feature branches, issues, PRs, the Project or repository settings.
- Propagating deleted branches or deleted tags to the mirror.
- Any change to plugins, manifests, catalogs, plugin READMEs or the public README.
- Changes for ChatGPT or Codex: the ChatGPT catalog is copied along with everything else, but no ChatGPT workspace setup is part of this request.
- Mirrors of other Cruna repositories.

## Further Notes

- Whether the Claude admin can pin a marketplace to a branch other than the default was not checked; it does not matter while the organization reads `main`, the mirror's default branch.
- If the mirror is ever recreated, the recorded manual steps plus one manual run of the workflow rebuild it completely.
