```yaml
number: 9944
title: Improve display of ranges when pre-releases are not allowed
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/prerelease-ranges
created_at: 2024-12-16T21:35:48Z
updated_at: 2024-12-17T17:05:16Z
url: https://github.com/astral-sh/uv/pull/9944
synced_at: 2026-01-10T12:00:01Z
```

# Improve display of ranges when pre-releases are not allowed

---

_Pull request opened by @zanieb on 2024-12-16 21:35_

Closes https://github.com/astral-sh/uv/issues/9891

There are two changes here

1. We now exclude pre-releases (if they are not allowed) from the available versions set when simplifying ranges, this means the simplified range reflects the _allowed_ available versions — which is what we want. We no longer segment ranges into arbitrary looking segments..
2. We improve on #9885, expanding the scope to avoid regressions where we would now otherwise enumerate a bunch of versions

---

_@zanieb reviewed on 2024-12-16 22:09_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:489 on 2024-12-16 22:09_

This change extends #9885 to apply to https://github.com/astral-sh/uv/pull/9944/files#diff-4decbdad65ae6c2e77b162950bc1a15907dacaff952fcee61559704bf370fb06R2172 which would now otherwise enumerate all the anyio versions

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:3383 on 2024-12-16 22:11_

Honestly no clue what's up here?

---

_@zanieb reviewed on 2024-12-16 22:11_

---

_@zanieb reviewed on 2024-12-16 22:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:3383 on 2024-12-16 22:13_

I think I need to gate this logic to avoid applying if any of the ranges include pre-releases? Yuck

---

_@zanieb reviewed on 2024-12-16 22:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:3383 on 2024-12-16 22:17_

Fixed.

---

_@zanieb reviewed on 2024-12-16 22:17_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:1244 on 2024-12-16 22:17_

Needed for https://github.com/astral-sh/uv/pull/9944#discussion_r1887646848

---

_Marked ready for review by @zanieb on 2024-12-16 22:17_

---

_Comment by @zanieb on 2024-12-16 22:22_

Related #751 - but that needs a reproduction before I can close it.

---

_Review requested from @charliermarsh by @zanieb on 2024-12-16 23:57_

---

_Label `error messages` added by @zanieb on 2024-12-17 14:55_

---

_Review requested from @konstin by @zanieb on 2024-12-17 15:56_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:488 on 2024-12-17 16:17_

```suggestion
                    // also no need to enumerate the available versions.
```

---

_@konstin approved on 2024-12-17 16:18_

good idea

---

_Merged by @zanieb on 2024-12-17 17:05_

---

_Closed by @zanieb on 2024-12-17 17:05_

---

_Branch deleted on 2024-12-17 17:05_

---
