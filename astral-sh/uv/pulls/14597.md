```yaml
number: 14597
title: Update CodSpeedHQ/action action to v3.7.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-3.x
created_at: 2025-07-14T03:56:27Z
updated_at: 2025-07-14T13:59:10Z
url: https://github.com/astral-sh/uv/pull/14597
synced_at: 2026-01-10T06:53:02Z
```

# Update CodSpeedHQ/action action to v3.7.0

---

_Pull request opened by @renovate on 2025-07-14 03:56_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v3.5.0` -> `v3.7.0` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v3.7.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.7.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.6.1...v3.7.0)

#### What's Changed

##### <!-- 0 -->🚀 Features

- Add pre- and post-benchmark scripts by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add cli args for perf by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;94](https://redirect.github.com/CodSpeedHQ/runner/pull/94)

##### <!-- 1 -->🐛 Bug Fixes

- Forward environment to systemd-run cmd by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Only panic in upload for non-existing integration by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Multi-line commands in valgrind by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Symlink libpython doesn't work for statically linked python by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;89](https://redirect.github.com/CodSpeedHQ/runner/pull/89)
- Run perf with sudo; support systemd-run for non-perf walltime by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Use correct path for unwind info by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

##### <!-- 7 -->⚙️ Internals

- Add executor tests by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;95](https://redirect.github.com/CodSpeedHQ/runner/pull/95)
- Add log to detect invalid origin url by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Upgrade to edition 2024 by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add debug logs for proc maps by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;88](https://redirect.github.com/CodSpeedHQ/runner/pull/88)
- Enhance version resolution with 'latest' support and flexible formats by [@&#8203;art049](https://redirect.github.com/art049) in [https://github.com/CodSpeedHQ/action/pull/132](https://redirect.github.com/CodSpeedHQ/action/pull/132)

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.6.1...v3.7.0
**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

### [`v3.6.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v3.6.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v3.5.0...v3.6.1)

##### What's Changed

##### <!-- 0 -->🚀 Features

- Allow setting upload url via env var for convenience by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;85](https://redirect.github.com/CodSpeedHQ/runner/pull/85)
- Send unknown cpu\_brand when it is not recognized by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- Allow only running the benchmarks, and only uploading the results by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;81](https://redirect.github.com/CodSpeedHQ/runner/pull/81)
- Install perf on setup by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add perf integration for python by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add perf integration for rust by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add fifo ipc by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Use custom time formatting to be in line with the rest of CodSpeed by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;77](https://redirect.github.com/CodSpeedHQ/runner/pull/77)
- Output information about benches after a local run by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;76](https://redirect.github.com/CodSpeedHQ/runner/pull/76)
- Allow specifying oauth token through CLI by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;75](https://redirect.github.com/CodSpeedHQ/runner/pull/75)
- Add option to output structured json by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;74](https://redirect.github.com/CodSpeedHQ/runner/pull/74)
- Add flags to specify repository from CLI by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Improve error handling for valgrind by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;67](https://redirect.github.com/CodSpeedHQ/runner/pull/67)
- Handle local run failure by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;71](https://redirect.github.com/CodSpeedHQ/runner/pull/71)
- Run benchmark with systemd (for optional cpu isolation) by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;86](https://redirect.github.com/CodSpeedHQ/runner/pull/86)

##### <!-- 1 -->🐛 Bug Fixes

- Persist logs when running with skip\_upload by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;84](https://redirect.github.com/CodSpeedHQ/runner/pull/84)
- Valgrind crash for unresolved libpython by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;82](https://redirect.github.com/CodSpeedHQ/runner/pull/82)
- Support trailing slash in origin url by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;83](https://redirect.github.com/CodSpeedHQ/runner/pull/83)
- Use bash to ensure correct behavior across systems by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Fix test randomly failing due to other test run in parallel by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Check child status code after valgrind by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;72](https://redirect.github.com/CodSpeedHQ/runner/pull/72)
- Only show perf output at debug or trace level by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;87](https://redirect.github.com/CodSpeedHQ/runner/pull/87)

##### <!-- 7 -->⚙️ Internals

- Dont use regex in perf map harvest by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Switch to astral-sh/cargo-dist by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;80](https://redirect.github.com/CodSpeedHQ/runner/pull/80)

**Full Changelog**: https://github.com/CodSpeedHQ/action/compare/v3.5.0...v3.6.1
**Full Runner Changelog**: https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4yMy4yIiwidXBkYXRlZEluVmVyIjoiNDEuMjMuMiIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-07-14 03:56_

---

_Merged by @zanieb on 2025-07-14 13:59_

---

_Closed by @zanieb on 2025-07-14 13:59_

---

_Branch deleted on 2025-07-14 13:59_

---
