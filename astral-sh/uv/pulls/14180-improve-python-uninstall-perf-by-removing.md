```yaml
number: 14180
title: "Improve Python uninstall perf by removing unnecessary call to `installations.find_all()`"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/speed-up-uninstall
created_at: 2025-06-21T09:29:18Z
updated_at: 2025-06-23T13:51:46Z
url: https://github.com/astral-sh/uv/pull/14180
synced_at: 2026-01-10T11:10:43Z
```

# Improve Python uninstall perf by removing unnecessary call to `installations.find_all()`

---

_Pull request opened by @jtfmumm on 2025-06-21 09:29_

#13954 introduced an unnecessary slow-down to Python uninstall by calling `installations.find_all()` to discover remaining installations after an uninstall. Instead, we can filter all initial installations against those in `uninstalled`.

As part of this change, I've updated `uninstalled` from a `Vec` to an `IndexSet` in order to do efficient lookups in the filter. This required a change I call out below to how we were retrieving them for messaging.


---

_Label `performance` added by @jtfmumm on 2025-06-21 09:29_

---

_@jtfmumm reviewed on 2025-06-21 09:29_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/uninstall.rs`:282 on 2025-06-21 09:29_

The destructuring approach no longer works now that this is an `IndexSet` instead of a `Vec`.

---

_@charliermarsh reviewed on 2025-06-21 23:48_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/uninstall.rs`:203 on 2025-06-21 23:48_

Nit: I'd suggest `IndexSet::<PythonInstallationKey>::default()` to avoid duplicating the type.

---

_Label `internal` added by @jtfmumm on 2025-06-23 07:50_

---

_Label `performance` removed by @jtfmumm on 2025-06-23 07:50_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/python/uninstall.rs`:203 on 2025-06-23 12:10_

Updated

---

_@jtfmumm reviewed on 2025-06-23 12:10_

---

_@Gankra approved on 2025-06-23 13:43_

---

_Comment by @Gankra on 2025-06-23 13:44_

Nice catch!

---

_Merged by @jtfmumm on 2025-06-23 13:51_

---

_Closed by @jtfmumm on 2025-06-23 13:51_

---

_Branch deleted on 2025-06-23 13:51_

---
