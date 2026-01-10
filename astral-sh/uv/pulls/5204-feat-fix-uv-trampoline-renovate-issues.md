```yaml
number: 5204
title: "feat: fix uv-trampoline renovate issues"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: trampoline-renovate
created_at: 2024-07-19T01:12:36Z
updated_at: 2024-07-25T05:19:14Z
url: https://github.com/astral-sh/uv/pull/5204
synced_at: 2026-01-10T13:37:23Z
```

# feat: fix uv-trampoline renovate issues

---

_Pull request opened by @samypr100 on 2024-07-19 01:12_

## Summary

1. Fixes errors from https://github.com/astral-sh/uv/pull/4878
2. More cleanup. Removed the need for `MaybeUninit` and `SizeOf` helpers. Renamed main entrypoint to expected default in windows `mainCRTStartup` to avoid re-declaring /ENTRY in build.rs.
3. Adds a small basic test harness that >>on windows<< will generate both types of launchers and run them. I've had been using this locally to test changes and edge cases, but it might be useful for others. It's based on core parts of install-wheel-rs.

## Test Plan

Tested locally on a couple of script/gui apps.

---

_Comment by @zanieb on 2024-07-19 01:37_

Thanks for owning this!

---

_Marked ready for review by @samypr100 on 2024-07-19 17:26_

---

_Review requested from @konstin by @zanieb on 2024-07-19 19:43_

---

_Assigned to @konstin by @zanieb on 2024-07-19 19:43_

---

_Review comment by @konstin on `crates/uv-trampoline/tests/harness.rs`:206 on 2024-07-22 12:33_

```suggestion
    // Generate Launcher Payload
```

---

_@konstin reviewed on 2024-07-22 12:33_

---

_Review comment by @konstin on `crates/uv-trampoline/tests/harness.rs`:191 on 2024-07-22 12:37_

Could you split this into two tests? One that runs the CLI test, and another that needs the user to close the window annotated with `#[ignore]`

---

_@konstin approved on 2024-07-22 12:37_

---

_@samypr100 reviewed on 2024-07-22 20:04_

---

_Review comment by @samypr100 on `crates/uv-trampoline/tests/harness.rs`:191 on 2024-07-22 20:04_

Done in 20df349646b6ac85fb9b7e01098f6018e4d67f76

---

_@konstin approved on 2024-07-23 08:12_

Thanks!

---

_Merged by @konstin on 2024-07-23 08:12_

---

_Closed by @konstin on 2024-07-23 08:12_

---

_Branch deleted on 2024-07-25 05:19_

---
