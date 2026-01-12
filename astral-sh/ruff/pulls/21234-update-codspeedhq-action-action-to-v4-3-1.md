```yaml
number: 21234
title: Update CodSpeedHQ/action action to v4.3.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-4.x
created_at: 2025-11-03T02:24:43Z
updated_at: 2025-11-03T14:53:21Z
url: https://github.com/astral-sh/ruff/pull/21234
synced_at: 2026-01-12T15:57:19Z
```

# Update CodSpeedHQ/action action to v4.3.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v4.1.1` -> `v4.3.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v4.3.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.3.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.2.1...v4.3.1)

#### Release Notes

##### <!-- 0 -->🚀 Features

- Enable read-inline-info to support inlined frames by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;139](https://redirect.github.com/CodSpeedHQ/runner/pull/139)
- Allow shorthand for selecting the mode by [@&#8203;art049](https://redirect.github.com/art049)
- Improve results display when running locally by [@&#8203;art049](https://redirect.github.com/art049)
- Bump to valgrind-codspeed 3.25.1-3codspeed2 by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;137](https://redirect.github.com/CodSpeedHQ/runner/pull/137)
- Allow wider range of valgrind codspeed version usage by [@&#8203;art049](https://redirect.github.com/art049)
- Automatically open the auth URL by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;133](https://redirect.github.com/CodSpeedHQ/runner/pull/133)
- Proper interactive sudo support by [@&#8203;art049](https://redirect.github.com/art049)
- Dump debug info of loaded modules by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;128](https://redirect.github.com/CodSpeedHQ/runner/pull/128)

##### <!-- 1 -->🐛 Bug Fixes

- Dont immediately add load\_bias to symbol offset by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Sudo behavior when root or not available ([#&#8203;141](https://redirect.github.com/CodSpeedHQ/action/issues/141)) by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;141](https://redirect.github.com/CodSpeedHQ/runner/pull/141)

#### Install codspeed-runner 4.3.1

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.3.1/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.3.1

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.3.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.3.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.3.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.3.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.2.1...v4.3.1>

### [`v4.2.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.2.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.2.0...v4.2.1)

#### Release Notes

#### What's Changed

##### <!-- 0 -->🚀 Features

- Use a prime number as frequency to avoid synchronization with periodic tasks by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- Pin actions/cache by [@&#8203;edgarrmondragon](https://redirect.github.com/edgarrmondragon) in [#&#8203;143](https://redirect.github.com/CodSpeedHQ/action/issues/143)
- Fix action cache by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;144](https://redirect.github.com/CodSpeedHQ/action/issues/144)

##### <!-- 1 -->🐛 Bug Fixes

- Ensure perf is always found on the machine by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;132](https://redirect.github.com/CodSpeedHQ/runner/pull/132)
- Correctly check if perf is installed by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)

##### <!-- 7 -->⚙️ Internals

- Add a metadata file in the cache that lists the installed packages by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- chore: bump runner version to 4.2.1 by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;145](https://redirect.github.com/CodSpeedHQ/action/issues/145)

##### New Contributors

- [@&#8203;edgarrmondragon](https://redirect.github.com/edgarrmondragon) made their first contribution in [#&#8203;143](https://redirect.github.com/CodSpeedHQ/action/pull/143)

#### Install codspeed-runner 4.2.1

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.2.1/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.2.1

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.2.0...v4.2.1>

### [`v4.2.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.2.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.1.1...v4.2.0)

#### Changes

✨ This new release adds a way for the runner to save and restore instruments it installs to a cache directory. The [CodSpeed github action](https://redirect.github.com/CodSpeedHQ/action) automatically makes use of these feature to speedup runs in your CI!

#### Details

##### <!-- 0 -->🚀 Features

- chore: bump runner version to 4.2.0 by [@&#8203;github-actions](https://redirect.github.com/github-actions)\[bot] in [#&#8203;142](https://redirect.github.com/CodSpeedHQ/action/issues/142)
- feat: cache valgrind installation from runner by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;140](https://redirect.github.com/CodSpeedHQ/action/issues/140)
- Allow caching installed executor instruments on ubuntu/debian by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;129](https://redirect.github.com/CodSpeedHQ/runner/pull/129)
- Automatically compress archive if profile folder is above a certain threshold by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;130](https://redirect.github.com/CodSpeedHQ/runner/pull/129)

##### <!-- 1 -->🐛 Bug Fixes

- Bump git2 to latest to support sparse checkout by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;131](https://redirect.github.com/CodSpeedHQ/runner/pull/131)

##### <!-- 7 -->⚙️ Internals

- Make fifo command dump trace level by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;130](https://redirect.github.com/CodSpeedHQ/runner/pull/130)
- chore: bump runner version to 4.2.0 by [@&#8203;github-actions](https://redirect.github.com/github-actions)\[bot] in [#&#8203;142](https://redirect.github.com/CodSpeedHQ/action/issues/142)

#### Install codspeed-runner 4.2.0

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.2.0/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.2.0

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.2.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.1.1...v4.2.0>

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

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNTkuNCIsInVwZGF0ZWRJblZlciI6IjQxLjE1OS40IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-11-03 02:24_

---

_Merged by @MichaReiser on 2025-11-03 03:04_

---

_Closed by @MichaReiser on 2025-11-03 03:04_

---

_Branch deleted on 2025-11-03 03:04_

---

_Comment by @github-actions[bot] on 2025-11-03 04:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Branch restored on 2025-11-03 14:53_

---
