# IMPLEMENTATION PLAN — Apache-2.0 license and repository metadata

License the repo and its plugins under Apache-2.0 and fill in the GitHub description, homepage and topics. What and why: [spec.md](spec.md).

## How to use this plan [do not change]

- follow the repo's `guidelines` skill if it has one, otherwise the conventions in its `AGENTS.md` / `CLAUDE.md`, for the code implementation
- **Thin slices, small commits**, one logical step each, on the `feature/<name-id>`
  branch off the integration branch; prefix each commit `<name-id> F<n>:` (feature) or `<name-id> M<n>:` (milestone).

### Impact at a glance (verified against the code)

New LICENSE files at the root and in `cruna-plugin-eng/agora/` and `cruna-plugin-ita/agora/`. Both Claude manifests gain three fields; all four manifests and the Claude catalog go to 0.7.1. Root README and both plugin READMEs gain a license note. No skill changes. Repo settings change through `gh repo edit`.

---

## Feature backlog

Guidelines: Hard dependencies noted; otherwise parallelizable.

---

### F1 - License files and manifests
**Capability:** Apache-2.0 LICENSE at the root and in each plugin; Claude manifests declare license, homepage, repository; version 0.7.1 (spec: Implementation Decisions).
**Acceptance:**
- Four byte-identical LICENSE files with the official Apache-2.0 text.
- Both Claude manifests have `license: Apache-2.0`, `homepage`, `repository`.
- Agora is 0.7.1 in both languages, both manifests and both catalogs where versions are listed.
- The portable checker passes; `bin/cruna-zip agora` includes LICENSE.
**Deps:** none
**Touchpoints:** none

### F2 - READMEs
**Capability:** License section in the root README; one-line license note in the English and Italian plugin READMEs; version table at 0.7.1.
**Acceptance:** each README names Apache-2.0 and links the LICENSE; the Italian note follows the glossary.
**Deps:** F1
**Touchpoints:** none

### F3 - Repository settings
**Capability:** description, homepage and topics on the public repo (spec: Implementation Decisions).
**Acceptance:** `gh repo view --json description,homepageUrl,repositoryTopics` shows the spec's values.
**Deps:** none
**Touchpoints:** GitHub repository settings of Cruna-Noise-To-Signal/cruna-plugins

---

## Milestones (each demoable)

- **M1-licensed:** F1 + F2 + F3; GitHub reports the license on `dev` and shows the new description, homepage and topics.

## Sequencing

F1 → F2; F3 independent.

## Risks / watch-items

- GitHub's license badge reads the default branch, so it appears only after release to `main`.
- The OpenAI manifest schema may later accept license fields; out of scope until documented.
