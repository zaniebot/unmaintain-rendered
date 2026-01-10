```yaml
number: 4706
title: "Add conversion from lockfile `Distribution` to `Metadata`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/lock-metadata
created_at: 2024-07-01T20:31:03Z
updated_at: 2024-07-02T18:03:22Z
url: https://github.com/astral-sh/uv/pull/4706
synced_at: 2026-01-10T13:48:28Z
```

# Add conversion from lockfile `Distribution` to `Metadata`

---

_Pull request opened by @ibraheemdev on 2024-07-01 20:31_

## Summary

Splitting this out from https://github.com/astral-sh/uv/pull/4495 because it's also useful to reuse the `uv pip tree` code for `uv tree`.


---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-01 20:31_

---

_Label `internal` added by @ibraheemdev on 2024-07-01 21:16_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1892 on 2024-07-01 22:21_

Should we use `FxHashMap` for consistency?

---

_@charliermarsh reviewed on 2024-07-01 22:21_

---

_@charliermarsh approved on 2024-07-01 22:22_

---

_@charliermarsh reviewed on 2024-07-01 22:23_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:1892 on 2024-07-01 22:23_

Tempted to have this return an enum (requirement or extra) rather than mutating this input argument, maybe a bit clearer. Or even just doing this at the call site? I defer to you though.

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:1892 on 2024-07-02 17:56_

`into_requirement` is used in three different iterator chains, so I think things are cleaner this way.

---

_@ibraheemdev reviewed on 2024-07-02 17:56_

---

_Merged by @ibraheemdev on 2024-07-02 18:03_

---

_Closed by @ibraheemdev on 2024-07-02 18:03_

---

_Branch deleted on 2024-07-02 18:03_

---
