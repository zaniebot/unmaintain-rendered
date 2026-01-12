```yaml
number: 8593
title: "uv init: Implies `--package` when using `--build-backend`"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: init
created_at: 2024-10-26T14:07:58Z
updated_at: 2024-10-26T14:59:07Z
url: https://github.com/astral-sh/uv/pull/8593
synced_at: 2026-01-12T16:08:23Z
```

# uv init: Implies `--package` when using `--build-backend`

---

_@j178_

## Summary

Closes #8568


---

_@j178 reviewed on 2024-10-26 14:11_

---

_Review comment by @j178 on `crates/uv/src/settings.rs`:205 on 2024-10-26 14:11_

`--virtual` should imply `--no-package` instead.

---

_@j178 reviewed on 2024-10-26 14:16_

---

_Review comment by @j178 on `crates/uv/src/settings.rs`:205 on 2024-10-26 14:16_

gonna fix in a separate PR

---

_@zanieb approved on 2024-10-26 14:50_

---

_Merged by @zanieb on 2024-10-26 14:51_

---

_Closed by @zanieb on 2024-10-26 14:51_

---

_Label `bug` added by @zanieb on 2024-10-26 14:51_

---

_Branch deleted on 2024-10-26 14:59_

---
