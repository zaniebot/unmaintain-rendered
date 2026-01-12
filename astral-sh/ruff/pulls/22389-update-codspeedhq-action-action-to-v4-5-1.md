```yaml
number: 22389
title: Update CodSpeedHQ/action action to v4.5.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-4.x
created_at: 2026-01-05T07:53:45Z
updated_at: 2026-01-05T08:10:08Z
url: https://github.com/astral-sh/ruff/pull/22389
synced_at: 2026-01-12T15:57:48Z
```

# Update CodSpeedHQ/action action to v4.5.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v4.4.1` → `v4.5.1` | `v4.5.2` |

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v4.5.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.5.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.4.1...v4.5.1)

#### Release Notes

##### <!-- 0 -->🚀 Features

- Remove projects query from the exec polling by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;173](https://redirect.github.com/CodSpeedHQ/runner/pull/173)
- Fetch project from API when running outside of git repo by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add get or create project repository query by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Automatically install exec-harness for exec subcommand by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Auto install codspeed-memtrack during executor setup by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Serialize events serially to allow streamed decoding by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;172](https://redirect.github.com/CodSpeedHQ/runner/pull/172)
- Parse perf file for memmap events instead of relying on /proc/pid/maps by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Use the projects upload enpdoint in exec command by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add exec subcommand and refactor run subcommand by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add exec-harness binary by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add memory executor by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add memtrack crate by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add artifact types by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add shared fifo by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add new fifo commands by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Support simulation for free-threaded python by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia) in [#&#8203;167](https://redirect.github.com/CodSpeedHQ/runner/pull/167)

##### <!-- 1 -->🐛 Bug Fixes

- fix: specify package when using branch/ref by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;162](https://redirect.github.com/CodSpeedHQ/action/pull/162)
- fix: properly check for allow\_empty input by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;164](https://redirect.github.com/CodSpeedHQ/action/pull/164)
- Filter out arm debugging symbols by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;179](https://redirect.github.com/CodSpeedHQ/runner/pull/179)
- Filter out empty named symbols when building perf-map by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;176](https://redirect.github.com/CodSpeedHQ/runner/pull/176)
- Do not break support for no reason when changing integration hooks protocol version by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;175](https://redirect.github.com/CodSpeedHQ/runner/pull/175)
- Remove dirty retry on timeout and simply increase timeout for GQL client by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Stop hanging indefinitely if process fails to start in memory executor by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;171](https://redirect.github.com/CodSpeedHQ/runner/pull/171)
- Remove the password prompt from the run\_with\_sudo dialog by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Collect events in thread to avoid mutex overhead by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Convert events in thread to avoid blocking at the end by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Compress only when size exceeds threshold by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Forward environment in memory executor by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Fix plan test in CI by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;165](https://redirect.github.com/CodSpeedHQ/runner/pull/165)
- Prevent nextest from running valgrind and memcheck concurrently by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Stop ignoring samples by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Use correct name for unwind\_data trait declaration by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Stop filtering out zero sized symbol by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Request OIDC token after creating profile archive by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;170](https://redirect.github.com/CodSpeedHQ/runner/pull/170)
- Remove snapshots that were not part of lfs by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;166](https://redirect.github.com/CodSpeedHQ/runner/pull/166)
- Always print memory mapping logs by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

##### <!-- 2 -->🏗️ Refactor

- Create a dedicated execution\_context that holds runtime information by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Move executor and instruments modules out of `run` module by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)

##### <!-- 7 -->⚙️ Internals

- docs(examples): add go and cpp examples by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;161](https://redirect.github.com/CodSpeedHQ/action/pull/161)
- Ignore some tags in the changelog
- Bump protocol version by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;174](https://redirect.github.com/CodSpeedHQ/runner/pull/174)
- Add CONTRIBUTING.md by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add cargo-dist arguments for release by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Reset exec-harness and memtrack crate versions to 1.0.0 ahead of first release by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Switch to pr run mode plan only for pr by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Add test to ensure path is properly forwarded by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;169](https://redirect.github.com/CodSpeedHQ/runner/pull/169)
- Make the exec command work outside of git repos by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Do not publish memtrack to crates.io by [@&#8203;adriencaccia](https://redirect.github.com/adriencaccia)
- Dont run valgrind and memory tests at the same time by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;164](https://redirect.github.com/CodSpeedHQ/runner/pull/164)
- Add test-log to see output on failures by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)
- Add workspace dependencies by [@&#8203;not-matthias](https://redirect.github.com/not-matthias)

#### Install codspeed-runner 4.5.1

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.5.1/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.5.1

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.5.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.5.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.5.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.5.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.4.1...v4.5.1>

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi42OS4xIiwidXBkYXRlZEluVmVyIjoiNDIuNjkuMSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-05 07:53_

---

_Merged by @MichaReiser on 2026-01-05 08:10_

---

_Closed by @MichaReiser on 2026-01-05 08:10_

---

_Branch deleted on 2026-01-05 08:10_

---
