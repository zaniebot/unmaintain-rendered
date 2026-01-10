```yaml
number: 811
title: Update astral-sh/setup-uv action to v6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-07-11T07:31:11Z
updated_at: 2025-07-11T07:44:24Z
url: https://github.com/astral-sh/ty/pull/811
synced_at: 2026-01-10T02:34:10Z
```

# Update astral-sh/setup-uv action to v6

---

_Pull request opened by @renovate on 2025-07-11 07:31_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | major | `v5.4.2` -> `v6.3.1` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v6.3.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.3.1): 🌈 Do not warn when version not in manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.3.0...v6.3.1)

##### Changes

This is a hotfix to change the warning messages that a version could not be found in the local manifest-file to info level.

A `setup-uv` release contains a version-manifest.json file with infos in all available `uv` releases. When a new `uv` version is released this is not contained in this file until the file gets updated and a new `setup-uv` release is made.
We will overhaul this process in the future but for now the spamming of warnings is removed.

##### 🐛 Bug fixes

- Do not warn when version not in manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;462](https://redirect.github.com/astral-sh/setup-uv/issues/462))

##### 🧰 Maintenance

- chore: update known versions for 0.7.14 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;459](https://redirect.github.com/astral-sh/setup-uv/issues/459))
- Revert "Set expected cache dir drive to C: on windows ([#&#8203;451](https://redirect.github.com/astral-sh/setup-uv/issues/451))" [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;460](https://redirect.github.com/astral-sh/setup-uv/issues/460))

### [`v6.3.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.3.0): 🌈 Use latest version from manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.2.1...v6.3.0)

#### Changes

If a manifest-file is supplied the default value of the version input (latest) will get the latest version available in the manifest. That might not be the actual latest version available in the official uv repo.

#### 🚀 Enhancements

- Use latest version from manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;458](https://redirect.github.com/astral-sh/setup-uv/issues/458))

### [`v6.2.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.2.1): 🌈 Fix &quot;No such file or directory version-manifest.json&quot;

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.2.0...v6.2.1)

##### Changes

Release v6.2.0 contained a bug that slipped through the automated test. The action tried to look for the default version-manifest.json in the root of the repostory using this action instead of relative to the action itself.

##### 🐛 Bug fixes

- Look for version-manifest.json relative to action path [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;456](https://redirect.github.com/astral-sh/setup-uv/issues/456))

### [`v6.2.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.2.0): 🌈  New input manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.1.0...v6.2.0)

##### Changes

This release adds a new input `manifest-file`.

The `manifest-file` input allows you to specify a JSON manifest that lists available uv versions,
architectures, and their download URLs. By default, this action uses the manifest file contained
in this repository, which is automatically updated with each release of uv.

The manifest file contains an array of objects, each describing a version,
architecture, platform, and the corresponding download URL.

You can supply a custom manifest file URL to define additional versions,
architectures, or different download URLs.
This is useful if you maintain your own uv builds or want to override the default sources.

For example:

```json
[
  {
    "version": "0.7.12-alpha.1",
    "artifactName": "uv-x86_64-unknown-linux-gnu.tar.gz",
    "arch": "x86_64",
    "platform": "unknown-linux-gnu",
    "downloadUrl": "https://release.pyx.dev/0.7.12-alpha.1/uv-x86_64-unknown-linux-gnu.tar.gz"
  },
  ...
]
```

```yaml
- name: Use a custom manifest file
  uses: astral-sh/setup-uv@v6
  with:
    manifest-file: "https://example.com/my-custom-manifest.json"
```

> \[!WARNING]\
> If you have previously used `server-url` to use your self hosted uv binaries use this new way instead.
> `server-url` is deprecated and will be removed in a future release

##### 🚀 Enhancements

- Add input manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;454](https://redirect.github.com/astral-sh/setup-uv/issues/454))
- Add warning about shadowed uv binaries to `activate-environment` [@&#8203;zanieb](https://redirect.github.com/zanieb) ([#&#8203;439](https://redirect.github.com/astral-sh/setup-uv/issues/439))

##### 🧰 Maintenance

- chore: update known versions for 0.7.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;444](https://redirect.github.com/astral-sh/setup-uv/issues/444))
- Set expected cache dir drive to C: on windows [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;451](https://redirect.github.com/astral-sh/setup-uv/issues/451))
- chore: update known versions for 0.7.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;442](https://redirect.github.com/astral-sh/setup-uv/issues/442))
- chore: update known versions for 0.7.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;440](https://redirect.github.com/astral-sh/setup-uv/issues/440))
- chore: update known versions for 0.7.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;437](https://redirect.github.com/astral-sh/setup-uv/issues/437))
- Check that all jobs are in all-tests-passed.needs [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;432](https://redirect.github.com/astral-sh/setup-uv/issues/432))
- chore: update known versions for 0.7.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;428](https://redirect.github.com/astral-sh/setup-uv/issues/428))

### [`v6.1.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.1.0): 🌈

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.0.1...v6.1.0)

##### Changes

This release adds the input `server-url` which defaults to `https://github.com`. You can set this to a custom url to control where this action downloads the uv release from. This is useful for users of gitea and comparable solutions.

[@&#8203;sebadevo](https://redirect.github.com/sebadevo) pointed out that we don't invalidate the cache when the `prune-cache` input is changed. This leads to unnessecarily big caches. The input is now used to compute the cache key, properly invalidating the cache when it is changed.

> \[!NOTE]\
> For most users this release will invalidate the cache once.
> You will see the known warning [no-github-actions-cache-found-for-key](https://redirect.github.com/astral-sh/setup-uv?tab=readme-ov-file#why-do-i-see-warnings-like-no-github-actions-cache-found-for-key)
> This is expected and will only appear once.

##### 🐛 Bug fixes

- Purge cache in cache key [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;423](https://redirect.github.com/astral-sh/setup-uv/issues/423))

##### 🚀 Enhancements

- feat: support custom github url [@&#8203;Zoupers](https://redirect.github.com/Zoupers) ([#&#8203;414](https://redirect.github.com/astral-sh/setup-uv/issues/414))

##### 🧰 Maintenance

- chore: update known versions for 0.7.7 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;422](https://redirect.github.com/astral-sh/setup-uv/issues/422))
- chore: update known versions for 0.7.6 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;415](https://redirect.github.com/astral-sh/setup-uv/issues/415))
- chore: update known versions for 0.7.5 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;412](https://redirect.github.com/astral-sh/setup-uv/issues/412))
- chore: update known versions for 0.7.4 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;410](https://redirect.github.com/astral-sh/setup-uv/issues/410))
- chore: update known versions for 0.7.3 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;405](https://redirect.github.com/astral-sh/setup-uv/issues/405))
- Fix path to known-checksums.ts [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;404](https://redirect.github.com/astral-sh/setup-uv/issues/404))
- Fix update-known-versions workflow argument [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;401](https://redirect.github.com/astral-sh/setup-uv/issues/401))
- Fix update-known-versions workflow [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;400](https://redirect.github.com/astral-sh/setup-uv/issues/400))
- Create version-manifest.json on uv release [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;399](https://redirect.github.com/astral-sh/setup-uv/issues/399))
- Run infrastructure workflows on arm runners [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;396](https://redirect.github.com/astral-sh/setup-uv/issues/396))
- chore: update known checksums for 0.7.2 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;395](https://redirect.github.com/astral-sh/setup-uv/issues/395))
- chore: update known checksums for 0.7.0 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;390](https://redirect.github.com/astral-sh/setup-uv/issues/390))

##### 📚 Documentation

- Add section to README explaining if packages are installed by setup-uv [@&#8203;pirate](https://redirect.github.com/pirate) ([#&#8203;398](https://redirect.github.com/astral-sh/setup-uv/issues/398))

##### ⬆️ Dependency updates

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;424](https://redirect.github.com/astral-sh/setup-uv/issues/424))
- Bump typescript from 5.8.2 to 5.8.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;393](https://redirect.github.com/astral-sh/setup-uv/issues/393))

### [`v6.0.1`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.0.1): 🌈 Fix default cache dependency glob

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6...v6.0.1)

#### Changes

The new default in v6 used illegal patterns and therefore didn't match requirements files. This is now fixed.

#### 🐛 Bug fixes

- Fix default cache dependency glob [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;388](https://redirect.github.com/astral-sh/setup-uv/issues/388))

#### 🧰 Maintenance

- chore: update known checksums for 0.6.17 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;384](https://redirect.github.com/astral-sh/setup-uv/issues/384))

#### ⬆️ Dependency updates

- Bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;389](https://redirect.github.com/astral-sh/setup-uv/issues/389))

### [`v6.0.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.0.0): 🌈 activate-environment and working-directory

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v5.4.2...v6)

##### Changes

This version contains some breaking changes which have been gathering up for a while. Lets dive into them:

- [Activate environment](#activate-environment)
- [Working Directory](#working-directory)
- [Default `cache-dependency-glob`](#default-cache-dependency-glob)
- [Use default cache dir on self hosted runners](#use-default-cache-dir-on-self-hosted-runners)

##### Activate environment

In previous versions using the input `python-version` automatically activated a venv at the repository root.
This led to some unwanted side-effects, was sometimes unexpected and not flexible enough.

The venv activation is now explicitly controlled with the new input `activate-environment` (false by default):

```yaml
- name: Install the latest version of uv and activate the environment
  uses: astral-sh/setup-uv@v6
  with:
    activate-environment: true
- run: uv pip install pip
```

The venv gets created by the [`uv venv`](https://docs.astral.sh/uv/pip/environments/) command so the python version is controlled by the `python-version` input or the files `pyproject.toml`, `uv.toml`, `.python-version` in the `working-directory`.

##### Working Directory

The new input `working-directory` controls where we look for `pyproject.toml`, `uv.toml` and `.python-version` files
which are used to determine the version of uv and python to install.

It can also be used to control where the venv gets created.

```yaml
- name: Install uv based on the config files in the working-directory
  uses: astral-sh/setup-uv@v6
  with:
    working-directory: my/subproject/dir
```

> \[!CAUTION]
>
> The inputs `pyproject-file` and `uv-file` have been removed.

##### Default `cache-dependency-glob`

[@&#8203;ssbarnea](https://redirect.github.com/ssbarnea) found out that the default `cache-dependency-glob` was not suitable for a lot of users.

The old default

```yaml
cache-dependency-glob: |
  **/requirements*.txt
  **/uv.lock
```

is changed and should cover over 99.5% of use cases:

```yaml
cache-dependency-glob: |
  **/*(requirements|constraints)*.(txt|in)
  **/pyproject.toml
  **/uv.lock
```

> \[!NOTE]
>
> This shouldn't be a breaking change. The only thing you may notice is that your caches get invalidated once.

##### Use default cache dir on self hosted runners

The directory where uv stores its cache was always set to a directory in `RUNNER_TEMP`. For self-hosted runners this made no sense as this gets cleaned after every run and led to slower runs than necessary.

On self-hosted runners `UV_CACHE_DIR` is no longer set and the [default cache directory](https://docs.astral.sh/uv/concepts/cache/#cache-directory) is used instead.

##### 🚨 Breaking changes

- Change default cache-dependency-glob [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;352](https://redirect.github.com/astral-sh/setup-uv/issues/352))
- No default UV\_CACHE\_DIR on selfhosted runners [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;380](https://redirect.github.com/astral-sh/setup-uv/issues/380))
- new inputs activate-environment and working-directory [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;381](https://redirect.github.com/astral-sh/setup-uv/issues/381))

##### 🧰 Maintenance

- chore: update known checksums for 0.6.16 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;378](https://redirect.github.com/astral-sh/setup-uv/issues/378))
- chore: update known checksums for 0.6.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;377](https://redirect.github.com/astral-sh/setup-uv/issues/377))

##### 📚 Documentation

- bump to v6 in README [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;382](https://redirect.github.com/astral-sh/setup-uv/issues/382))
- log info on venv activation [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;375](https://redirect.github.com/astral-sh/setup-uv/issues/375))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4yMy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMjMuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-11 07:31_

---

_Merged by @MichaReiser on 2025-07-11 07:44_

---

_Closed by @MichaReiser on 2025-07-11 07:44_

---

_Branch deleted on 2025-07-11 07:44_

---
