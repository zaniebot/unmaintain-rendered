```yaml
number: 1340
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
created_at: 2025-10-13T00:42:52Z
updated_at: 2025-10-13T06:22:21Z
url: https://github.com/astral-sh/ty/pull/1340
synced_at: 2026-01-10T02:34:10Z
```

# Update astral-sh/setup-uv action to v7

---

_Pull request opened by @renovate on 2025-10-13 00:42_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | major | `v6.8.0` -> `v7.1.0` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

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

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ty).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNDMuMSIsInVwZGF0ZWRJblZlciI6IjQxLjE0My4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-10-13 00:42_

---

_Merged by @MichaReiser on 2025-10-13 06:22_

---

_Closed by @MichaReiser on 2025-10-13 06:22_

---

_Branch deleted on 2025-10-13 06:22_

---
