```yaml
number: 17291
title: Update astral-sh/setup-uv action to v7
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-7.x
created_at: 2026-01-02T16:07:53Z
updated_at: 2026-01-02T16:21:19Z
url: https://github.com/astral-sh/uv/pull/17291
synced_at: 2026-01-12T16:12:42Z
```

# Update astral-sh/setup-uv action to v7

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | major | `v6.8.0` → `v7.1.6` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v7.1.6`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.6): 🌈 add OS version to cache key to prevent binary incompatibility

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.5...v7.1.6)

##### Changes

This release will invalidate your cache existing keys!

The os version e.g. `ubuntu-22.04` is now part of the cache key. This prevents failing builds when a cache got populated with wheels built with different tools (e.g. glibc) than are present on the runner where the cache got restored.

##### 🐛 Bug fixes

- feat: add OS version to cache key to prevent binary incompatibility [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;716](https://redirect.github.com/astral-sh/setup-uv/issues/716))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.17 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;714](https://redirect.github.com/astral-sh/setup-uv/issues/714))

##### ⬆️ Dependency updates

- Bump actions/checkout from 5.0.0 to 6.0.1 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;712](https://redirect.github.com/astral-sh/setup-uv/issues/712))
- Bump actions/setup-node from 6.0.0 to 6.1.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;715](https://redirect.github.com/astral-sh/setup-uv/issues/715))

### [`v7.1.5`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.5): 🌈 allow setting `cache-local-path` without `enable-cache: true`

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.4...v7.1.5)

##### Changes

[#&#8203;612](https://redirect.github.com/astral-sh/setup-uv/pull/612) fixed a faulty behavior where this action set `UV_CACHE_DIR` even though `enable-cache` was `false`. It also fixed the cases were the cache dir is already configured in a settings file like `pyproject.toml` or `UV_CACHE_DIR` was already set.  Here the action shouldn't overwrite or set `UV_CACHE_DIR`.

These fixes introduced an unwanted behavior: You can still set `cache-local-path` but this action didn't do anything. This release fixes that.

You can now use `cache-local-path` to automatically set `UV_CACHE_DIR` even when `enable-cache` is `false` (or gets set to false by default e.g. on self-hosted runners)

```yaml
- name: This is now possible
  uses: astral-sh/setup-uv@v7
  with:
    enable-cache: false
    cache-local-path: "/path/to/cache"
```

##### 🐛 Bug fixes

- allow cache-local-path w/o enable-cache [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;707](https://redirect.github.com/astral-sh/setup-uv/issues/707))

##### 🧰 Maintenance

- set biome files.maxSize to 2MiB [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;708](https://redirect.github.com/astral-sh/setup-uv/issues/708))
- chore: update known checksums for 0.9.16 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;706](https://redirect.github.com/astral-sh/setup-uv/issues/706))
- chore: update known checksums for 0.9.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;704](https://redirect.github.com/astral-sh/setup-uv/issues/704))
- chore: use `npm ci --ignore-scripts` everywhere [@&#8203;woodruffw](https://redirect.github.com/woodruffw) ([#&#8203;699](https://redirect.github.com/astral-sh/setup-uv/issues/699))
- chore: update known checksums for 0.9.14 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;700](https://redirect.github.com/astral-sh/setup-uv/issues/700))
- chore: update known checksums for 0.9.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;694](https://redirect.github.com/astral-sh/setup-uv/issues/694))
- chore: update known checksums for 0.9.12 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;693](https://redirect.github.com/astral-sh/setup-uv/issues/693))
- chore: update known checksums for 0.9.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;688](https://redirect.github.com/astral-sh/setup-uv/issues/688))

##### ⬆️ Dependency updates

- Bump peter-evans/create-pull-request from 7.0.8 to 7.0.9 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;695](https://redirect.github.com/astral-sh/setup-uv/issues/695))
- bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;709](https://redirect.github.com/astral-sh/setup-uv/issues/709))
- Bump github/codeql-action from 4.30.9 to 4.31.6 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;698](https://redirect.github.com/astral-sh/setup-uv/issues/698))
- Bump zizmorcore/zizmor-action from 0.2.0 to 0.3.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;696](https://redirect.github.com/astral-sh/setup-uv/issues/696))
- Bump eifinger/actionlint-action from 1.9.2 to 1.9.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;690](https://redirect.github.com/astral-sh/setup-uv/issues/690))

### [`v7.1.4`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.4): 🌈 Fix libuv closing bug on Windows

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.3...v7.1.4)

##### Changes

This release fixes the bug `Assertion failed: !(handle->flags & UV_HANDLE_CLOSING)` on Windows runners

##### 🐛 Bug fixes

- Wait 50ms before exit to fix libuv bug [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;689](https://redirect.github.com/astral-sh/setup-uv/issues/689))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;681](https://redirect.github.com/astral-sh/setup-uv/issues/681))
- chore: update known checksums for 0.9.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;679](https://redirect.github.com/astral-sh/setup-uv/issues/679))

### [`v7.1.3`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.3): 🌈 Support act

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.2...v7.1.3)

##### Changes

This bug fix release adds support for <https://github.com/nektos/act>
It was previously broken because of a too new `undici` version and TS transpilation target.

Compatibility with act is now automatically tested.

##### 🐛 Bug fixes

- use old undici and ES2022 target for act support [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;678](https://redirect.github.com/astral-sh/setup-uv/issues/678))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;677](https://redirect.github.com/astral-sh/setup-uv/issues/677))
- chore: update known checksums for 0.9.7 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;671](https://redirect.github.com/astral-sh/setup-uv/issues/671))
- chore: update known checksums for 0.9.6 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;670](https://redirect.github.com/astral-sh/setup-uv/issues/670))

##### 📚 Documentation

- Correct description of `cache-dependency-glob` [@&#8203;allanlewis](https://redirect.github.com/allanlewis) ([#&#8203;676](https://redirect.github.com/astral-sh/setup-uv/issues/676))

### [`v7.1.2`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.2): 🌈 Speed up extraction on Windows

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.1...v7.1.2)

#### Changes

[@&#8203;lazka](https://redirect.github.com/lazka) fixed a bug that caused extracting uv to take up to 30s. Thank you!

#### 🐛 Bug fixes

- Use tar for extracting the uv zip file on Windows too [@&#8203;lazka](https://redirect.github.com/lazka) ([#&#8203;660](https://redirect.github.com/astral-sh/setup-uv/issues/660))

#### 🧰 Maintenance

- chore: update known checksums for 0.9.5 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;663](https://redirect.github.com/astral-sh/setup-uv/issues/663))

#### ⬆️ Dependency updates

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;664](https://redirect.github.com/astral-sh/setup-uv/issues/664))
- Bump github/codeql-action from 4.30.8 to 4.30.9 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;652](https://redirect.github.com/astral-sh/setup-uv/issues/652))

### [`v7.1.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.1): 🌈 Fix empty workdir detection and lowest resolution strategy

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.0...v7.1.1)

##### Changes

This release fixes a bug where the `working-directory` input was not used to detect an empty work dir. It also fixes the `lowest` resolution strategy resolving to latest when only a lower bound was specified.

Special thanks to [@&#8203;tpgillam](https://redirect.github.com/tpgillam) for the first contribution!

##### 🐛 Bug fixes

- Fix "lowest" resolution strategy with lower-bound only [@&#8203;tpgillam](https://redirect.github.com/tpgillam) ([#&#8203;649](https://redirect.github.com/astral-sh/setup-uv/issues/649))
- Use working-directory to detect empty workdir [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;645](https://redirect.github.com/astral-sh/setup-uv/issues/645))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.4 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;651](https://redirect.github.com/astral-sh/setup-uv/issues/651))
- chore: update known checksums for 0.9.3 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;644](https://redirect.github.com/astral-sh/setup-uv/issues/644))

##### 📚 Documentation

- Change version in docs to v7 [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;647](https://redirect.github.com/astral-sh/setup-uv/issues/647))

##### ⬆️ Dependency updates

- Bump github/codeql-action from 4.30.7 to 4.30.8 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;639](https://redirect.github.com/astral-sh/setup-uv/issues/639))
- Bump actions/setup-node from 5.0.0 to 6.0.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;641](https://redirect.github.com/astral-sh/setup-uv/issues/641))
- Bump eifinger/actionlint-action from 1.9.1 to 1.9.2 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;634](https://redirect.github.com/astral-sh/setup-uv/issues/634))
- Update lockfile with latest npm [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;636](https://redirect.github.com/astral-sh/setup-uv/issues/636))

### [`v7.1.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.0): 🌈 Support all the use cases

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.0.0...v7.1.0)

#### Changes

**Support all the use cases!!!**
... well, that we know of.

This release adds support for some use cases that most users don't encounter but are useful for e.g. people running Gitea.

The input `resolution-strategy` lets you use the lowest possible version of uv from a version range. Useful if you want to test your tool with different versions of uv.

If you use `activate-environment` the path to the activated venv is now also exposed under the output `venv`.

Downloaded python installations can now also be uploaded to the GitHub Actions cache backend. Useful if you are running in `act` and have configured your own backend and don't want to download python again, and again over a slow internet connection.

Finally the path to installed python interpreters is now added to the `PATH` on Windows.

#### 🚀 Enhancements

- Add resolution-strategy input to support oldest compatible version selection @&#8203;[copilot-swe-agent\[bot\]](https://redirect.github.com/apps/copilot-swe-agent) ([#&#8203;631](https://redirect.github.com/astral-sh/setup-uv/issues/631))
- Add value of UV\_PYTHON\_INSTALL\_DIR to path [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;628](https://redirect.github.com/astral-sh/setup-uv/issues/628))
- Set output venv when activate-environment is used [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;627](https://redirect.github.com/astral-sh/setup-uv/issues/627))
- Cache python installs [@&#8203;merlinz01](https://redirect.github.com/merlinz01) ([#&#8203;621](https://redirect.github.com/astral-sh/setup-uv/issues/621))

#### 🧰 Maintenance

- Add copilot-instructions.md [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;630](https://redirect.github.com/astral-sh/setup-uv/issues/630))
- chore: update known checksums for 0.9.2 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;626](https://redirect.github.com/astral-sh/setup-uv/issues/626))
- chore: update known checksums for 0.9.1 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;625](https://redirect.github.com/astral-sh/setup-uv/issues/625))
- Fall back to PR for updating known versions [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;623](https://redirect.github.com/astral-sh/setup-uv/issues/623))

#### 📚 Documentation

- Split up documentation [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;632](https://redirect.github.com/astral-sh/setup-uv/issues/632))

#### ⬆️ Dependency updates

- Bump deps [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;633](https://redirect.github.com/astral-sh/setup-uv/issues/633))
- Bump github/codeql-action from 3.30.6 to 4.30.7 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;614](https://redirect.github.com/astral-sh/setup-uv/issues/614))

### [`v7.0.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.0.0): 🌈 node24 and a lot of bugfixes

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.8.0...v7.0.0)

#### Changes

This release comes with a load of bug fixes and a speed up. Because of switching from node20 to node24 it is also a breaking change. If you are running on GitHub hosted runners this will just work, if you are using self-hosted runners make sure, that your runners are up to date. If you followed the normal installation instructions your self-hosted runner will keep itself updated.

This release also removes the deprecated input `server-url` which was used to download uv releases from a different server.
The [manifest-file](https://redirect.github.com/astral-sh/setup-uv?tab=readme-ov-file#manifest-file) input supersedes that functionality by adding a flexible way to define available versions and where they should be downloaded from.

##### Fixes

- The action now respects when the environment variable `UV_CACHE_DIR` is already set and does not overwrite it. It now also finds [cache-dir](https://docs.astral.sh/uv/reference/settings/#cache-dir) settings in config files if you set them.
- Some users encountered problems that [cache pruning](https://redirect.github.com/astral-sh/setup-uv?tab=readme-ov-file#disable-cache-pruning) took forever because they had some `uv` processes running in the background. Starting with uv version `0.8.24` this action uses `uv cache prune --ci --force` to ignore the running processes
- If you just want to install uv but not have it available in path, this action now respects `UV_NO_MODIFY_PATH`
- Some other actions also set the env var `UV_CACHE_DIR`. This action can now deal with that but as this could lead to unwanted behavior in some edgecases a warning is now displayed.

##### Improvements

If you are using minimum version specifiers for the version of uv to install for example

```toml
[tool.uv]
required-version = ">=0.8.17"
```

This action now detects that and directly uses the latest version. Previously it would download all available releases from the uv repo
to determine the highest matching candidate for the version specifier, which took much more time.

If you are using other specifiers like `0.8.x` this action still needs to download all available releases because the specifier defines an upper bound (not 0.9.0 or later) and "latest" would possibly not satisfy that.

#### 🚨 Breaking changes

- Use node24 instead of node20 [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;608](https://redirect.github.com/astral-sh/setup-uv/issues/608))
- Remove deprecated input server-url [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;607](https://redirect.github.com/astral-sh/setup-uv/issues/607))

#### 🐛 Bug fixes

- Respect UV\_CACHE\_DIR and cache-dir [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;612](https://redirect.github.com/astral-sh/setup-uv/issues/612))
- Use --force when pruning cache [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;611](https://redirect.github.com/astral-sh/setup-uv/issues/611))
- Respect UV\_NO\_MODIFY\_PATH [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;603](https://redirect.github.com/astral-sh/setup-uv/issues/603))
- Warn when `UV_CACHE_DIR` has changed [@&#8203;jamesbraza](https://redirect.github.com/jamesbraza) ([#&#8203;601](https://redirect.github.com/astral-sh/setup-uv/issues/601))

#### 🚀 Enhancements

- Shortcut to latest version for minimum version specifier [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;598](https://redirect.github.com/astral-sh/setup-uv/issues/598))

#### 🧰 Maintenance

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;613](https://redirect.github.com/astral-sh/setup-uv/issues/613))
- Fix test-uv-no-modify-path [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;604](https://redirect.github.com/astral-sh/setup-uv/issues/604))
- Don't assume all test passed if cancelled [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;599](https://redirect.github.com/astral-sh/setup-uv/issues/599))

#### ⬆️ Dependency updates

- Bump github/codeql-action from 3.30.5 to 3.30.6 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;605](https://redirect.github.com/astral-sh/setup-uv/issues/605))
- Bump github/codeql-action from 3.30.3 to 3.30.5 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;594](https://redirect.github.com/astral-sh/setup-uv/issues/594))
- Bump [@&#8203;renovatebot/pep440](https://redirect.github.com/renovatebot/pep440) from 4.2.0 to 4.2.1 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;581](https://redirect.github.com/astral-sh/setup-uv/issues/581))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-02 16:07_

---

_@woodruffw approved on 2026-01-02 16:09_

---

_Merged by @woodruffw on 2026-01-02 16:21_

---

_Closed by @woodruffw on 2026-01-02 16:21_

---

_Branch deleted on 2026-01-02 16:21_

---
