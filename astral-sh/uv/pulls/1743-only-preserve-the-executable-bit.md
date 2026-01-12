```yaml
number: 1743
title: Only preserve the executable bit
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/set-extract-permissions
created_at: 2024-02-20T09:31:31Z
updated_at: 2024-02-20T15:41:06Z
url: https://github.com/astral-sh/uv/pull/1743
synced_at: 2026-01-12T16:04:42Z
```

# Only preserve the executable bit

---

_@konstin_

A file in a zip can set arbitrary unix permissions, but we, like pip, want to preserve only the executable bit and otherwise use the OS defaults.

This should be faster for wheels with many files since we now avoid the blocking fs call to set the permissions in most cases.

Fixes #1740.

---

_Label `bug` added by @konstin on 2024-02-20 09:31_

---

_@konstin reviewed on 2024-02-20 09:32_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:80 on 2024-02-20 09:32_

Permission seems to lack an api to set read and write without manual bitflags math.

---

_@ismail reviewed on 2024-02-20 09:36_

---

_Review comment by @ismail on `crates/uv-extract/src/stream.rs`:80 on 2024-02-20 09:36_

This should take `umask` into account, see https://github.com/pypa/pip/blob/main/src/pip/_internal/operations/install/wheel.py#L652

---

_Renamed from "Set read and write permissions for extracted files" to "Only preserve the executable bit" by @konstin on 2024-02-20 10:13_

---

_@konstin reviewed on 2024-02-20 10:28_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:80 on 2024-02-20 10:28_

Thanks for the pip reference!

I rewrote it so that we're now never setting the permissions explicitly ourselves, but instead first create the file with the default options and then attach the executable bit if required.

---

_@charliermarsh approved on 2024-02-20 13:59_

---

_Merged by @konstin on 2024-02-20 15:41_

---

_Closed by @konstin on 2024-02-20 15:41_

---

_Branch deleted on 2024-02-20 15:41_

---
