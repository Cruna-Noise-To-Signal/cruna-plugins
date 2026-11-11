# Report: Apache-2.0 license and repository metadata (radiant-baguette)

Request #21. Spec: [spec.md](spec.md). Plan: [plan.md](plan.md). Hand checks: [manual-test.md](manual-test.md).

## What was done

**F1 - License files and manifests**
- `LICENSE` at the repo root, `cruna-plugin-eng/agora/LICENSE` and `cruna-plugin-ita/agora/LICENSE`: byte-identical, the official Apache License 2.0 text from `gh api licenses/apache-2.0`. The only change is the appendix example line, filled in as `Copyright 2026 Cruna`. It sits after `END OF TERMS AND CONDITIONS`, which GitHub's license detection ignores. No NOTICE file.
- Both Claude manifests (`.claude-plugin/plugin.json`) gained `"license": "Apache-2.0"`, `"homepage": "https://crunasignal.com"` and `"repository": "https://github.com/Cruna-Noise-To-Signal/cruna-plugins"`. The English manifest's one-line `author` object is now spread over several lines, with the same content.
- Agora 0.7.0 → 0.7.1 in all four manifests and the Claude catalog. The ChatGPT catalog lists no versions. The OpenAI manifests changed only their version.
- `bin/cruna-zip agora` now includes `LICENSE` without any change to the script, because it zips the whole plugin folder.

**F2 - READMEs**
- Root README: version table changed from 0.6.0 to 0.7.1, a `LICENSE` row in the repository map, and a new `## License` section that names Apache License 2.0, links `LICENSE`, says every plugin carries a copy, and says the license grants no rights to the Cruna name or logo.
- `cruna-plugin-eng/agora/README.md`: `## License` with "Apache License 2.0: see [LICENSE](LICENSE)."
- `cruna-plugin-ita/agora/README.md`: `## Licenza` with "Apache License 2.0: vedi [LICENSE](LICENSE)."
- `docs/agora/glossary-it.md`: new term License → Licenza (the license name and the `LICENSE` file stay in English).

**F3 - Repository settings: not applied (blocked)**
- `gh repo edit Cruna-Noise-To-Signal/cruna-plugins --description … --homepage … --add-topic …` failed with `HTTP 403: Resource not accessible by personal access token`. The agent's `GH_TOKEN` lacks the Administration write permission on the repo.
- Recorded as a manual action in the `pm-setup` issue #1 (key `radiant-baguette:repo-settings`), with the exact command to run. The mirror was not touched.

## Commits (feature/radiant-baguette)

1. `radiant-baguette F1: Apache-2.0 LICENSE at the root and in each plugin`
2. `radiant-baguette F1: Claude manifests declare license, homepage and repository; Agora 0.7.1`
3. `radiant-baguette F2: READMEs state the Apache-2.0 license; version table at 0.7.1`
4. `radiant-baguette M1: report and manual test`

## Testing

- Test first: an acceptance script was written and run before any change, and all 17 checks failed. It checks that the three LICENSE files exist, that the text is the official one, that the copies are byte-identical, the manifest fields, 0.7.1 in the manifests and the Claude catalog, LICENSE inside the zip, and the README notes. All 17 pass after the change. The script is a throwaway in the session scratchpad and is not committed, because the repo has no test suite.
- The portable checker (`check_plugins.py .`) reports `✔ Valid for Claude and OpenAI` after every slice. The new manifest fields don't upset it.
- GitHub license detection could not be confirmed yet. `GET repos/…/license?ref=feature/radiant-baguette` returns 404 for the pushed branch. This endpoint seems to answer only for the default branch, so the check belongs after the release to `main` (spec risk: the badge reads the default branch).

## Notes

- `agora.zip` and `agora-ita.zip` at the repo root are untracked and ignored by `.gitignore` (`*.zip`). They were left as they are.
- The Agora spec was not changed, because no behaviour changed (spec: Out of Scope).
- Still open: F3 needs an admin to run the recorded command, then `pm manual done --key radiant-baguette:repo-settings --by human`.
