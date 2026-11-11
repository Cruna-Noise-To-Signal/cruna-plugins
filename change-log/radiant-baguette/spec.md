Request: #21 (radiant-baguette)

## Problem Statement

cruna-plugins is a public repository with no license, which legally means "all rights reserved": nobody may use, copy or adapt the plugins, even though the README invites everyone to install them. The repository also has no description, homepage or topics, so on GitHub it looks unfinished and is hard to find.

## Solution

The repository and every plugin it ships are licensed under Apache-2.0, chosen over MIT for its explicit patent grant and because it grants no right to the Cruna name. The license text travels with each plugin, so a plugin installed from the marketplace or uploaded as a zip still carries it. The GitHub repository gets a one-line description, Cruna's website as homepage, and topics that describe what it ships.

## User Stories

1. As a person installing Agora, I want to know under which terms I may use it, so that I can use it at work without legal doubt.
2. As a developer, I want a standard, recognised license, so that I can fork or adapt a plugin knowing what I must keep.
3. As a company legal reviewer, I want GitHub to detect the license automatically, so that my compliance tooling classifies the repo correctly.
4. As a user who received a plugin as a zip, I want the license inside the zip, so that the terms travel with the copy I have.
5. As a user browsing the marketplace, I want each plugin manifest to declare its license, so that the plugin manager can show it.
6. As Cruna, I want the license to grant no rights to the Cruna name or logo, so that forks cannot pass themselves off as Cruna.
7. As Cruna, I want an explicit patent grant and termination clause, so that contributors and users are protected from patent claims.
8. As a visitor on GitHub, I want a short description and a homepage link, so that I understand what the repo is and who makes it.
9. As someone searching GitHub, I want topics such as claude plugins and chatgpt, so that I can find the repo.
10. As an Italian-speaking user, I want the Italian plugin README to state the license too, so that both language versions say the same thing.
11. As a README reader, I want a License section, so that I find the terms without opening the LICENSE file.

## Implementation Decisions

- License: Apache License 2.0, the unmodified official text, copyright "2026 Cruna". No NOTICE file: Apache-2.0 makes it optional, and there are no third-party notices to carry.
- One LICENSE at the repository root (GitHub's license detection reads it) and an identical copy at the root of each plugin folder (English and Italian), because the zip and the marketplace install copy only the plugin folder.
- Claude plugin manifests gain `license` (`Apache-2.0`, SPDX id), `homepage` (https://crunasignal.com) and `repository` (the public repo URL), all documented Claude plugin manifest fields. The OpenAI manifest stays as it is apart from the version, because its schema documents no such fields and the portable checker does not cover them.
- Agora goes from 0.7.0 to 0.7.1 in all four manifests and the Claude catalog: the plugin files change, so they get a version bump.
- The root README gets a License section; each plugin README gets a one-line license note, in English and Italian.
- Repository settings, set with `gh repo edit` on the public repo only (the private mirror is not touched):
  - description: "Cruna's plugins for Claude and ChatGPT, in English and Italian"
  - homepage: https://crunasignal.com
  - topics: claude, claude-code, claude-plugins, chatgpt, codex, ai-agents, agent-skills, plugin-marketplace, google-drive, productivity

## Testing Decisions

- The portable checker (`check_plugins.py`) passes on the repo.
- `gh repo view --json licenseInfo` reports Apache-2.0 once the LICENSE reaches the default branch (after release); before that, `gh api repos/.../license?ref=dev` reports it.
- `gh repo view --json description,homepageUrl,repositoryTopics` shows the new values.
- `bin/cruna-zip agora` produces a zip that contains LICENSE.
- The four LICENSE files are byte-identical.

## Out of Scope

- Licensing the private repositories of the organisation or the mirror.
- A CONTRIBUTING guide, code of conduct or contributor license agreement.
- Per-file license headers in skill files.
- Changes to the Agora spec: the license changes no Agora behaviour.

## Further Notes

The README's plugin version table already lags the manifests (it shows 0.6.0); it is updated to 0.7.1 while touching the README.
