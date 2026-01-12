```yaml
number: 14325
title: "Make warnings about masked `[tool.uv]` fields more precise"
type: pull_request
state: merged
author: Gankra
labels:
  - error messages
assignees: []
merged: true
base: main
head: gankra/warn-precise
created_at: 2025-06-27T19:01:04Z
updated_at: 2025-07-20T22:54:52Z
url: https://github.com/astral-sh/uv/pull/14325
synced_at: 2026-01-12T16:11:09Z
```

# Make warnings about masked `[tool.uv]` fields more precise

---

_@Gankra_

This is the second half of #14308

---

_Label `error messages` added by @Gankra on 2025-06-27 19:01_

---

_@Gankra reviewed on 2025-06-27 19:03_

---

_Review comment by @Gankra on `crates/uv/tests/it/show_settings.rs`:3885 on 2025-06-27 19:03_

(The crux of this test is just that stderr is empty)

---

_@Gankra reviewed on 2025-06-27 19:06_

---

_Review comment by @Gankra on `crates/uv/tests/it/show_settings.rs`:3661 on 2025-06-27 19:06_

As a bonus I made the diagnostic print out the offending fields

---

_@zanieb approved on 2025-07-11 16:59_

---

_Merged by @charliermarsh on 2025-07-20 22:54_

---

_Closed by @charliermarsh on 2025-07-20 22:54_

---

_Branch deleted on 2025-07-20 22:54_

---
