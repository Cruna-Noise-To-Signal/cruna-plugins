# Manual test: private mirror (loyal-cassata)

Hand checks for `.github/workflows/mirror.yml`. Public repo: `Cruna-Noise-To-Signal/cruna-plugins`. Mirror: `Cruna-Noise-To-Signal/cruna-plugins-mirror` (already created). Steps 1-2 must be done **before** merging the PR into `dev`, because the merge itself triggers the workflow.

## 1. Create the GitHub App (org owner)

1. https://github.com/organizations/Cruna-Noise-To-Signal/settings/apps > **New GitHub App**.
2. Name `Cruna plugins mirror`; Homepage URL `https://github.com/Cruna-Noise-To-Signal/cruna-plugins`.
3. Webhook: untick **Active**.
4. Repository permissions: **Contents: Read and write** (Metadata: Read-only is added automatically). Nothing else.
5. Where can this GitHub App be installed: **Only on this account**. Create.
6. On the App's page, note the **App ID**. Under **Private keys**, **Generate a private key**: a `.pem` file downloads.
7. Left menu > **Install App** > Cruna-Noise-To-Signal > **Only select repositories** > `cruna-plugins-mirror` > Install.

## 2. Add the variable and the secret (before merging)

On the public repo > Settings > Secrets and variables > Actions:
1. **Variables** tab > New repository variable: name `MIRROR_APP_ID`, value = the App ID.
2. **Secrets** tab > New repository secret: name `MIRROR_APP_PRIVATE_KEY`, value = full contents of the `.pem` file (including the `-----BEGIN` / `-----END` lines).
3. Delete the downloaded `.pem` file.
4. Tick `mirror:app` and `mirror:secret` in issue #1 (`pm manual done --key mirror:app --by human`, same for `mirror:secret`).

Expected: the App shows as installed on `cruna-plugins-mirror` only; the variable and the secret are listed in the repo's Actions settings.

## 3. Merge and verify the first run

1. Squash-merge the PR into `dev`. Actions > Mirror shows a run triggered by the push to `dev`.
2. Also run it once by hand: Actions > Mirror > Run workflow, branch `dev`.
3. Open each run. Expected: all steps green; the push step lists `main`, `dev` and the `v*` tags (first run: `[new branch]` / `[new tag]`), then prints `main: mirror at <sha>, matches` and `dev: mirror at <sha>, matches`.

If the first step fails with "MIRROR_APP_ID or the secret MIRROR_APP_PRIVATE_KEY is missing", step 2 was skipped. If the token step fails, check the App ID and the key; if the push fails with a 403 or "Repository not found", check the App is installed on `cruna-plugins-mirror` with Contents: Read and write.

## 4. Compare refs on both repos

```sh
git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*'
git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins-mirror.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*'
```

(The mirror is private: use an account with read access.) Expected: identical lines in both outputs. The mirror has no other branches.

One-liner diff (empty output means pass):

```sh
diff <(git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*') \
     <(git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins-mirror.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*')
```

Then tick `mirror:first-run` in issue #1.

## 5. A dev push follows

1. Merge any small PR into `dev` (or push a trivial commit to `dev`).
2. Expected: a Mirror run starts within seconds and goes green; repeating the step 4 diff gives empty output, and the mirror's `refs/heads/dev` equals the new public `dev` SHA.

## 6. A release follows, with its tag

1. Run the normal release (`dev` into `main`, tag `vYYYY.MM.DD`).
2. Expected: Mirror runs for the push to `main` and for the tag push; they run one after another and go green. With several pushes in quick succession GitHub may cancel a waiting run; that's fine, because the run that goes ahead pushes the latest state.
3. Repeat the step 4 diff: empty output; the new tag is on the mirror and points at the same commit.

## 7. Forks do not run it

1. Fork the public repo into a personal account (or use an existing fork) and push a commit to the fork's `dev`, or run Actions > Mirror > Run workflow there (enable Actions on the fork if GitHub asks).
2. Expected: the run shows the `mirror` job as **skipped** (grey), no step executes, and the mirror's refs do not change.
3. Delete the fork if it was made only for this test.

## 8. Install agora from the mirror in the Claude admin

1. Claude admin of the Cruna organization > plugin / marketplace settings > add a GitHub marketplace: `Cruna-Noise-To-Signal/cruna-plugins-mirror`, branch `main` (grant the Claude GitHub app access to the private repo if asked).
2. Expected: the marketplace appears as `cruna-plugins` listing `agora` and `agora-ita` at the same versions as `main` in the public repo.
3. Make `agora` and `agora-ita` available to members. As a member in Claude Desktop, install `agora`; expected: it installs and its skills appear (for example, ask "what can Agora do?" and the `agora:help` skill answers).
4. Tick `mirror:claude-admin` in issue #1.
