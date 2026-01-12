```yaml
number: 12227
title: Update CodSpeedHQ/action action to v3.5.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-3.x
created_at: 2025-03-17T03:37:15Z
updated_at: 2025-03-17T18:17:05Z
url: https://github.com/astral-sh/uv/pull/12227
synced_at: 2026-01-12T16:10:11Z
```

# Update CodSpeedHQ/action action to v3.5.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v3` -> `v3.5.0` |

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v3.5.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.5.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.4.0...v3.5.0)

#### What's Changed

##### <!-- 0 -->🚀 Features

-   Add mode command arg by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;69](https://redirect.github.com/CodSpeedHQ/runner/pull/69)
-   Add a setup command by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;61](https://redirect.github.com/CodSpeedHQ/runner/pull/61)
-   Allow usage on any x86 or arm os with a warning by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;66](https://redirect.github.com/CodSpeedHQ/runner/pull/66)

##### <!-- 1 -->🐛 Bug Fixes

-   Fix valgrind version checks by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;65](https://redirect.github.com/CodSpeedHQ/runner/pull/65)

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.4.0...v3.5.0
**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

### [`v3.4.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.4.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.3.1...v3.4.0)

#### What's Changed

##### <!-- 0 -->🚀 Features

-   feat: add `GH_MATRIX` and `GH_STRATEGY` env variables by [@&#8203;fargito](https://redirect.github.com/fargito) in [https://github.com/CodSpeedHQ/action/pull/123](https://redirect.github.com/CodSpeedHQ/action/pull/123)
-   Add run_part to upload metadata by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;57](https://redirect.github.com/CodSpeedHQ/runner/pull/57)

##### <!-- 1 -->🐛 Bug Fixes

-   Fix stderr error display by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;63](https://redirect.github.com/CodSpeedHQ/runner/pull/63)

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.3.1...v3.4.0
**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

### [`v3.3.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.3.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.3.0...v3.3.1)

#### What's Changed

##### <!-- 0 -->🚀 Features

-   Bail when performance report s3 upload does not work by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)

##### <!-- 1 -->🐛 Bug Fixes

-   Catch server error as well as client in upload error handling by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;64](https://redirect.github.com/CodSpeedHQ/runner/pull/64)

**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

### [`v3.3.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.3.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.2.0...v3.3.0)

#### What's Changed

##### <!-- 0 -->🚀 Features

-   Allow downgrades while installing valgrind-codspeed by [@&#8203;art049](https://redirect.github.com/art049)
-   Update sysinfo crate by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;62](https://redirect.github.com/CodSpeedHQ/runner/pull/62)

##### <!-- 1 -->🐛 Bug Fixes

-   Unify environments between the two modes by [@&#8203;art049](https://redirect.github.com/art049) in [#&#8203;59](https://redirect.github.com/CodSpeedHQ/runner/pull/59)

##### <!-- 7 -->⚙️ Internals

-   Bump valgrind-codspeed version to 3.24.0-0codspeed1 and change supported systems by [@&#8203;art049](https://redirect.github.com/art049)

**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

### [`v3.2.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.2.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.1.0...v3.2.0)

#### Release Notes

##### <!-- 0 -->🚀 Features

-   Implement gitlab ci provider by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;47](https://redirect.github.com/CodSpeedHQ/runner/pull/47)
-   Add repository provider to upload metadata by [@&#8203;fargito](https://redirect.github.com/fargito)
-   Use system distribution id instead of name by [@&#8203;fargito](https://redirect.github.com/fargito)

##### <!-- 2 -->🏗️ Refactor

-   Move sender out of ghdata by [@&#8203;fargito](https://redirect.github.com/fargito)
-   Rename provider to ci provider by [@&#8203;fargito](https://redirect.github.com/fargito)
-   Use string for runId by [@&#8203;fargito](https://redirect.github.com/fargito)
-   Improve string interpolation by [@&#8203;fargito](https://redirect.github.com/fargito)

##### <!-- 7 -->⚙️ Internals

-   Configure git-cliff for changelog generation by [@&#8203;art049](https://redirect.github.com/art049)
-   Add rust settings by [@&#8203;fargito](https://redirect.github.com/fargito)

**Associated runner release**: https://github.com/CodSpeedHQ/runner/releases/tag/v3.2.0
**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.1.0...v3.2.0

### [`v3.1.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.1.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.0.1...v3.1.0)

#### What's Changed

This new release includes compatibility with our new `wall time` instruments, which allow measurement of real execution timings. This will be very useful for macro benchmarks running on properly isolated machines. You can now use our **CodSpeed Macro** runners without changing anything in your existing CI workflows.

Find more info on our Walltime instrument [here](https://docs.codspeed.io/instruments/walltime/).

![image](https://redirect.github.com/user-attachments/assets/650d1123-5cd6-4309-84e0-d5dd6ced64f3)

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.0.1...v3.1.0

### [`v3.0.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.0.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3...v3.0.1)

#### What's Changed

-   chore: bump runner version to 3.0.1 by [@&#8203;github-actions](https://redirect.github.com/github-actions) in [https://github.com/CodSpeedHQ/action/pull/115](https://redirect.github.com/CodSpeedHQ/action/pull/115)
-   chore: support ubuntu 24.04

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.0.0...v3.0.1

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yMDAuMCIsInVwZGF0ZWRJblZlciI6IjM5LjIwMC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-03-17 03:37_

---

_Merged by @charliermarsh on 2025-03-17 18:17_

---

_Closed by @charliermarsh on 2025-03-17 18:17_

---

_Branch deleted on 2025-03-17 18:17_

---
