```yaml
number: 20711
title: Update CodSpeedHQ/action action to v4.1.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-4.x
created_at: 2025-10-06T02:08:42Z
updated_at: 2025-10-06T06:00:13Z
url: https://github.com/astral-sh/ruff/pull/20711
synced_at: 2026-01-10T17:34:34Z
```

# Update CodSpeedHQ/action action to v4.1.0

---

_Pull request opened by @renovate on 2025-10-06 02:08_

> [!NOTE]
> Mend has cancelled [the proposed renaming](https://redirect.github.com/renovatebot/renovate/discussions/37842) of the Renovate GitHub app being renamed to `mend[bot]`.
> 
> This notice will be removed on 2025-10-07.

<hr>

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v4.0.1` -> `v4.1.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v4.1.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.1.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.0.1...v4.1.0)

##### Changes

✨ This new release improves the accuracy of walltime profiling with perf and reduces the upload times.

##### Details

##### <!-- 0 -->🚀 Features

- Add timestamp to unwind data by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;120](https://redirect.github.com/CodSpeedHQ/runner/pull/120)
- Add unwind data v2 format with base\_svma by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add perf v2 support by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;119](https://redirect.github.com/CodSpeedHQ/runner/pull/119)
- Add runner-shared crate by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add content encoding to upload metadata by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- Do not compress profile archive for walltime runs by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- Detect stack size at runtime by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;103](https://redirect.github.com/CodSpeedHQ/runner/pull/103)
- Add unwind data tests by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Run python with perf jit dump by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

##### <!-- 1 -->🐛 Bug Fixes

- Use shared elf\_helper for unwind and symbol information by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Forward go runner exit status by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;115](https://redirect.github.com/CodSpeedHQ/runner/pull/115)
- Ignore statically linked python by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Codspeed debug check by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Create perf map for jitdump by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

##### <!-- 2 -->🏗️ Refactor

- Store upload metadata latest version in a const by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;117](https://redirect.github.com/CodSpeedHQ/runner/pull/117)
- Refactor profile-archive by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)

##### <!-- 7 -->⚙️ Internals

- Cargo clippy lints by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Only enable debug logs GH action is debugged by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;118](https://redirect.github.com/CodSpeedHQ/runner/pull/118)
- Fix the release commit message by [@&#8203;art049](https://redirect.github.com/art049)
- Make runner-shared not publishable by [@&#8203;art049](https://redirect.github.com/art049)
- Add debug log for /proc/<pid>/maps by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

##### Install codspeed-runner 4.1.0

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.1.0/codspeed-runner-installer.sh | sh
```

##### Download codspeed-runner 4.1.0

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.1.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.1.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.1.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.1.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xMzEuOSIsInVwZGF0ZWRJblZlciI6IjQxLjEzMS45IiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-10-06 02:08_

---

_Comment by @github-actions[bot] on 2025-10-06 02:22_

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

_Merged by @MichaReiser on 2025-10-06 06:00_

---

_Closed by @MichaReiser on 2025-10-06 06:00_

---

_Branch deleted on 2025-10-06 06:00_

---
