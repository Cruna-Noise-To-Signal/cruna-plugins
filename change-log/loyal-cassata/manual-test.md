# Manual test: private mirror (loyal-cassata)

Hand checks for `.github/workflows/mirror.yml`. Public repo: `Cruna-Noise-To-Signal/cruna-plugins`. Mirror: `Cruna-Noise-To-Signal/cruna-plugins-mirror` (already created). Steps 1-2 must be done **before** merging the PR into `dev`, because the merge itself triggers the workflow.

## 1. Generate the deploy key pair

In a scratch folder outside any repo:

```sh
ssh-keygen -t ed25519 -N '' -C cruna-plugins-mirror-sync -f mirror_deploy_key
```

This writes `mirror_deploy_key` (private) and `mirror_deploy_key.pub` (public).

## 2. Add the deploy key and the secret (before merging)

1. Mirror repo > Settings > Deploy keys > Add deploy key. Title `cruna-plugins sync`, key = contents of `mirror_deploy_key.pub`, tick **Allow write access**. Save.
2. Public repo > Settings > Secrets and variables > Actions > New repository secret. Name `MIRROR_DEPLOY_KEY`, value = full contents of `mirror_deploy_key` (including the `-----BEGIN` / `-----END` lines). Save.
3. Delete both local files: `rm mirror_deploy_key mirror_deploy_key.pub`.
4. Tick `mirror:deploy-key` and `mirror:secret` in issue #1 (`pm manual done --key mirror:deploy-key --by human`, same for `mirror:secret`).

Expected: the deploy key is listed with "Read/write"; the secret is listed under Repository secrets.

## 3. Merge and verify the first run

1. Squash-merge the PR into `dev`. Actions > Mirror shows a run triggered by the push to `dev`.
2. Also run it once by hand: Actions > Mirror > Run workflow, branch `dev`.
3. Open each run. Expected: all steps green; the push step lists `main`, `dev` and the `v*` tags (first run: `[new branch]` / `[new tag]`); the check step prints `main: mirror at <sha>, matches` and `dev: mirror at <sha>, matches`.

If the key step fails with "Secret MIRROR_DEPLOY_KEY is missing", step 2.2 was skipped. If the push fails with `Permission denied (publickey)` or "denied to deploy key", check the key pair and the write-access tick.

## 4. Compare refs on both repos

```sh
git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*'
git ls-remote git@github.com:Cruna-Noise-To-Signal/cruna-plugins-mirror.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*'
```

(The mirror is private: use an account with read access.) Expected: identical lines in both outputs. The mirror has no other branches.

One-liner diff (empty output means pass):

```sh
diff <(git ls-remote https://github.com/Cruna-Noise-To-Signal/cruna-plugins.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*') \
     <(git ls-remote git@github.com:Cruna-Noise-To-Signal/cruna-plugins-mirror.git 'refs/heads/main' 'refs/heads/dev' 'refs/tags/v*')
```

Then tick `mirror:first-run` in issue #1.

## 5. A dev push follows

1. Merge any small PR into `dev` (or push a trivial commit to `dev`).
2. Expected: a Mirror run starts within seconds and goes green; repeating the step 4 diff gives empty output, and the mirror's `refs/heads/dev` equals the new public `dev` SHA.

## 6. A release follows, with its tag

1. Run the normal release (`dev` into `main`, tag `vYYYY.MM.DD`).
2. Expected: Mirror runs for the push to `main` and for the tag push; they run one after another (never cancelled) and both go green.
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
