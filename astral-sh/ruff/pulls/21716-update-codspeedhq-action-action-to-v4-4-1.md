```yaml
number: 21716
title: Update CodSpeedHQ/action action to v4.4.1
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/codspeedhq-action-4.x
created_at: 2025-12-01T01:04:47Z
updated_at: 2025-12-01T06:58:58Z
url: https://github.com/astral-sh/ruff/pull/21716
synced_at: 2026-01-12T15:57:31Z
```

# Update CodSpeedHQ/action action to v4.4.1

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [CodSpeedHQ/action](https://redirect.github.com/CodSpeedHQ/action) | action | minor | `v4.3.4` -> `v4.4.1` |

---

> [!WARNING]
> Some dependencies could not be looked up. Check the Dependency Dashboard for more information.

---

### Release Notes

<details>
<summary>CodSpeedHQ/action (CodSpeedHQ/action)</summary>

### [`v4.4.1`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.4.1)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.4.0...v4.4.1)

#### Release Notes

##### <!-- 0 -->🚀 Features

- Display oidc as announcement by [@&#8203;fargito](https://redirect.github.com/fargito)
- Add --allow-empty run option by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;160](https://redirect.github.com/CodSpeedHQ/runner/pull/160)

##### <!-- 1 -->🐛 Bug Fixes

- Do not espace trailing newlines in logger by [@&#8203;fargito](https://redirect.github.com/fargito)
- Make multiline logs appear correctly in summary by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;162](https://redirect.github.com/CodSpeedHQ/runner/pull/162)
- Request OIDC token just before upload by [@&#8203;fargito](https://redirect.github.com/fargito)
- Update docs links to oidc by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;159](https://redirect.github.com/CodSpeedHQ/runner/pull/159)

##### <!-- 7 -->⚙️ Internals

- feat: make use of allow-empty in the ci of this action by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;158](https://redirect.github.com/CodSpeedHQ/action/pull/158)
- chore: bump runner version to 4.4.1 by [@&#8203;github-actions](https://redirect.github.com/github-actions)\[bot] in [#&#8203;159](https://redirect.github.com/CodSpeedHQ/action/pull/159)

#### Install codspeed-runner 4.4.1

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.4.1/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.4.1

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.1/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.1/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.4.0...v4.4.1>

### [`v4.4.0`](https://redirect.github.com/CodSpeedHQ/action/releases/tag/v4.4.0)

[Compare Source](https://redirect.github.com/CodSpeedHQ/action/compare/v4.3.4...v4.4.0)

#### Release Notes

##### <!-- 0 -->🚀 Features

- Add support for oidc token authentication by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;156](https://redirect.github.com/CodSpeedHQ/runner/pull/156)
  - You can now use OIDC to authenticate your CodSpeed workflow and remove `CODSPEED_TOKEN` from your repositories secret! Learn more in the dedicated [changelog entry](https://codspeed.io/changelog/2025-11-19-simpler-authentication-with-oidc)
- Accept simulation as runner mode by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;152](https://redirect.github.com/CodSpeedHQ/runner/pull/152)
- Add a comment explaining why we do not check for emptiness in valgrind teardown by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;157](https://redirect.github.com/CodSpeedHQ/runner/pull/157)
- Validate walltime results before uploading by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Import walltime\_results from monorepo by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)
- Accept both simulation and instrumentation as a mode input by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange) in [#&#8203;155](https://redirect.github.com/CodSpeedHQ/action/pull/155)

##### <!-- 1 -->🐛 Bug Fixes

- Dont start perf unless it's not already started by [@&#8203;not-matthias](https://redirect.github.com/not-matthias) in [#&#8203;158](https://redirect.github.com/CodSpeedHQ/runner/pull/158)
- Use a line buffer when reading stdout/stderr streams by [@&#8203;GuillaumeLagrange](https://redirect.github.com/GuillaumeLagrange)

##### <!-- 4 --> 📚 Documentation

- docs: recommend OpenID Connect instead of static token by [@&#8203;fargito](https://redirect.github.com/fargito) in [#&#8203;156](https://redirect.github.com/CodSpeedHQ/action/pull/156)

##### <!-- 7 -->⚙️ Internals

- chore: bump runner version to 4.4.0 by [@&#8203;github-actions](https://redirect.github.com/github-actions)\[bot] in [#&#8203;157](https://redirect.github.com/CodSpeedHQ/action/pull/157)

#### Install codspeed-runner 4.4.0

##### Install prebuilt binaries via shell script

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/CodSpeedHQ/runner/releases/download/v4.4.0/codspeed-runner-installer.sh | sh
```

#### Download codspeed-runner 4.4.0

| File                                                                                                                                                                 | Platform         | Checksum                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| [codspeed-runner-aarch64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz) | ARM64 MUSL Linux | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.0/codspeed-runner-aarch64-unknown-linux-musl.tar.gz.sha256) |
| [codspeed-runner-x86\_64-unknown-linux-musl.tar.gz](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz)  | x64 MUSL Linux   | [checksum](https://redirect.github.com/CodSpeedHQ/runner/releases/download/v4.4.0/codspeed-runner-x86_64-unknown-linux-musl.tar.gz.sha256)  |

**Full Runner Changelog**: <https://github.com/CodSpeedHQ/runner/blob/main/CHANGELOG.md>

**Full Changelog**: <https://github.com/CodSpeedHQ/action/compare/v4.3.4...v4.4.0>

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi4xOS45IiwidXBkYXRlZEluVmVyIjoiNDIuMTkuOSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-12-01 01:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 01:23_


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

_Merged by @MichaReiser on 2025-12-01 06:58_

---

_Closed by @MichaReiser on 2025-12-01 06:58_

---

_Branch deleted on 2025-12-01 06:58_

---
