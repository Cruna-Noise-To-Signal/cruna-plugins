# Manual test: Apache-2.0 license and repository metadata (radiant-baguette)

Hand checks for request #21. Run them from a clone of `Cruna-Noise-To-Signal/cruna-plugins` on branch `feature/radiant-baguette` (or `dev` after the merge).

## 1. LICENSE files

1. `cmp LICENSE cruna-plugin-eng/agora/LICENSE && cmp LICENSE cruna-plugin-ita/agora/LICENSE && echo identical`
   Expected: `identical`.
2. `diff <(gh api licenses/apache-2.0 --jq .body) LICENSE`
   Expected: exactly one difference, `Copyright [yyyy] [name of copyright owner]` → `Copyright 2026 Cruna`.
3. `ls NOTICE` → Expected: no such file.

## 2. Manifests and catalogs

1. `cat cruna-plugin-eng/agora/.claude-plugin/plugin.json cruna-plugin-ita/agora/.claude-plugin/plugin.json`
   Expected: each has `"version": "0.7.1"`, `"license": "Apache-2.0"`, `"homepage": "https://crunasignal.com"`, `"repository": "https://github.com/Cruna-Noise-To-Signal/cruna-plugins"`.
2. `grep version cruna-plugin-*/agora/plugin.json .claude-plugin/marketplace.json`
   Expected: `0.7.1` everywhere. The OpenAI `plugin.json` files have no license fields.
3. `python3 <cruna-core>/skills/portable-plugins/scripts/check_plugins.py .`
   Expected: `✔ Valid for Claude and OpenAI`.

## 3. Zip carries the license

1. `cd "$(mktemp -d)" && <repo>/bin/cruna-zip agora && unzip -l agora.zip | grep LICENSE`
   Expected: one `LICENSE` line at the zip root. Repeat with `agora-ita`.

## 4. Install from the marketplace (Claude Code)

1. `/plugin marketplace add Cruna-Noise-To-Signal/cruna-plugins` (or `/plugin marketplace update cruna-plugins`), on a build that contains this branch.
2. `/plugin install agora@cruna-plugins`, then open the plugin's details in `/plugin`.
   Expected: version 0.7.1. If the plugin manager shows license and homepage, they read Apache-2.0 and crunasignal.com. The installed plugin folder contains `LICENSE`.

## 5. READMEs

1. Open the root `README.md` on GitHub.
   Expected: the table shows 0.7.1 for both plugins, the repository map has a `LICENSE` row, and a `## License` section at the bottom links to `LICENSE` and mentions the Cruna name and logo.
2. Open `cruna-plugin-eng/agora/README.md`. Expected: `## License`, "Apache License 2.0: see LICENSE", and the link opens the plugin's LICENSE.
3. Open `cruna-plugin-ita/agora/README.md`. Expected: `## Licenza`, "Apache License 2.0: vedi LICENSE", and the link works.

## 6. Repository settings (after the manual action)

The agent's token could not change repo settings (HTTP 403). First run the command recorded in issue #1, key `radiant-baguette:repo-settings`, with an admin account (or use Settings > About on GitHub).

1. `gh repo view Cruna-Noise-To-Signal/cruna-plugins --json description,homepageUrl,repositoryTopics`
   Expected: description `Cruna's plugins for Claude and ChatGPT, in English and Italian`, homepage `https://crunasignal.com`, topics `claude, claude-code, claude-plugins, chatgpt, codex, ai-agents, agent-skills, plugin-marketplace, google-drive, productivity`.
2. On the repo's GitHub page, the About box shows the same values.
3. Check that `cruna-plugins-mirror` settings are unchanged.
4. Tick the item: `pm manual done --key radiant-baguette:repo-settings --by human`.

## 7. GitHub license detection (after release to `main`)

1. `gh repo view Cruna-Noise-To-Signal/cruna-plugins --json licenseInfo`
   Expected: `{"licenseInfo":{"key":"apache-2.0","name":"Apache License 2.0",…}}`. The About box shows "Apache-2.0 license".
