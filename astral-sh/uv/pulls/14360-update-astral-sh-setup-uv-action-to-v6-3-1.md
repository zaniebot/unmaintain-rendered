```yaml
number: 14360
title: Update astral-sh/setup-uv action to v6.3.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-6.x
created_at: 2025-06-30T01:48:28Z
updated_at: 2025-06-30T02:17:49Z
url: https://github.com/astral-sh/uv/pull/14360
synced_at: 2026-01-12T16:11:11Z
```

# Update astral-sh/setup-uv action to v6.3.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | minor | `v6.1.0` -> `v6.3.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

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

#### Changes

Release v6.2.0 contained a bug that slipped through the automated test. The action tried to look for the default version-manifest.json in the root of the repostory using this action instead of relative to the action itself.

#### 🐛 Bug fixes

- Look for version-manifest.json relative to action path [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;456](https://redirect.github.com/astral-sh/setup-uv/issues/456))

### [`v6.2.0`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v6.2.0): 🌈  New input manifest-file

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v6.1.0...v6.2.0)

#### Changes

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

#### 🚀 Enhancements

- Add input manifest-file [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;454](https://redirect.github.com/astral-sh/setup-uv/issues/454))
- Add warning about shadowed uv binaries to `activate-environment` [@&#8203;zanieb](https://redirect.github.com/zanieb) ([#&#8203;439](https://redirect.github.com/astral-sh/setup-uv/issues/439))

#### 🧰 Maintenance

- chore: update known versions for 0.7.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;444](https://redirect.github.com/astral-sh/setup-uv/issues/444))
- Set expected cache dir drive to C: on windows [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;451](https://redirect.github.com/astral-sh/setup-uv/issues/451))
- chore: update known versions for 0.7.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;442](https://redirect.github.com/astral-sh/setup-uv/issues/442))
- chore: update known versions for 0.7.10 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;440](https://redirect.github.com/astral-sh/setup-uv/issues/440))
- chore: update known versions for 0.7.9 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;437](https://redirect.github.com/astral-sh/setup-uv/issues/437))
- Check that all jobs are in all-tests-passed.needs [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;432](https://redirect.github.com/astral-sh/setup-uv/issues/432))
- chore: update known versions for 0.7.8 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;428](https://redirect.github.com/astral-sh/setup-uv/issues/428))

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC42Mi4xIiwidXBkYXRlZEluVmVyIjoiNDAuNjIuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-06-30 01:48_

---

_Merged by @charliermarsh on 2025-06-30 02:17_

---

_Closed by @charliermarsh on 2025-06-30 02:17_

---

_Branch deleted on 2025-06-30 02:17_

---
