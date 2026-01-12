```yaml
number: 16109
title: "Move `TRACING_DURATIONS_FILE` to `EnvironmentOptions`"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - internal
assignees: []
merged: true
base: main
head: move-TRACING_DURATIONS_FILE-to-EnvironmentOptions
created_at: 2025-10-03T02:57:10Z
updated_at: 2025-10-04T05:10:25Z
url: https://github.com/astral-sh/uv/pull/16109
synced_at: 2026-01-12T16:12:06Z
```

# Move `TRACING_DURATIONS_FILE` to `EnvironmentOptions`

---

_@TaKO8Ki_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #14720

Add `TRACING_DURATIONS_FILE` to EnvironmentOptions.

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2025-10-03 13:48_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:578 on 2025-10-03 13:48_

This should be an `Option<Path>`, right?

---

_@TaKO8Ki reviewed on 2025-10-03 14:09_

---

_Review comment by @TaKO8Ki on `crates/uv-settings/src/lib.rs`:578 on 2025-10-03 14:09_

I’m keeping it as a String here—similar to `python_downloads_json_url`—since I thought that would be preferable. But if that isn’t necessary, I’ll change it to PathBuf.

https://github.com/astral-sh/uv/blob/cce20938aea11adb1060578e1e451a8fe2897b06/crates/uv-settings/src/settings.rs#L1006

---

_@zanieb reviewed on 2025-10-03 15:38_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:578 on 2025-10-03 15:38_

The URL can either be a `Path` or a `Url`.

This one should always be a `Path`.

---

_@zanieb reviewed on 2025-10-03 15:39_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:608 on 2025-10-03 15:39_

We might want a dedicated `parse_path_environment_variable` — `PathBuf` can be parsed from an `OsString`, it doesn't need to be able to be converted to a unicode `String`

---

_@zanieb approved on 2025-10-04 05:09_

---

_Comment by @zanieb on 2025-10-04 05:09_

Thanks!

---

_Merged by @zanieb on 2025-10-04 05:09_

---

_Closed by @zanieb on 2025-10-04 05:09_

---

_Label `internal` added by @zanieb on 2025-10-04 05:10_

---

_Branch deleted on 2025-10-04 05:10_

---
