```yaml
number: 5032
title: "pip compile: exclude --upgrade-package from the header"
type: pull_request
state: merged
author: skshetry
labels:
  - bug
assignees: []
merged: true
base: main
head: exclude-upgrade-package-header
created_at: 2024-07-13T03:43:44Z
updated_at: 2024-07-13T04:55:02Z
url: https://github.com/astral-sh/uv/pull/5032
synced_at: 2026-01-12T16:06:35Z
```

# pip compile: exclude --upgrade-package from the header

---

_@skshetry_

## Summary

Fixes #5031.

## Test Plan

Existing snapshot tests should cover it.



---

_Marked ready for review by @skshetry on 2024-07-13 04:10_

---

_@zanieb approved on 2024-07-13 04:24_

Thanks for contributing!

---

_Merged by @zanieb on 2024-07-13 04:24_

---

_Closed by @zanieb on 2024-07-13 04:24_

---

_Label `bug` added by @zanieb on 2024-07-13 04:24_

---

_Branch deleted on 2024-07-13 04:28_

---

_@skshetry reviewed on 2024-07-13 04:43_

---

_Review comment by @skshetry on `crates/uv/src/commands/pip/compile.rs`:551 on 2024-07-13 04:43_

Looks like this does not work with `--upgrade-package=` or `-P=`. I should have looked more carefully at `--find-links` above.

Will create a fix.

---

_@zanieb reviewed on 2024-07-13 04:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:551 on 2024-07-13 04:55_

Thanks!

---
