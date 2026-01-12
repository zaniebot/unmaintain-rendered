```yaml
number: 4703
title: "Replace `map_or(false, ..)` uses with `is_some_and` and `is_ok_and`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/is-some-and
created_at: 2024-07-01T19:00:06Z
updated_at: 2024-07-01T19:28:43Z
url: https://github.com/astral-sh/uv/pull/4703
synced_at: 2026-01-12T16:06:24Z
```

# Replace `map_or(false, ..)` uses with `is_some_and` and `is_ok_and`

---

_@ibraheemdev_

## Summary

Looks like there isn't a clippy lint for this yet.

---

_Label `internal` added by @ibraheemdev on 2024-07-01 19:17_

---

_@charliermarsh reviewed on 2024-07-01 19:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/list.rs`:67 on 2024-07-01 19:17_

Wait this one is `map_or(true)`

---

_@charliermarsh approved on 2024-07-01 19:17_

---

_@ibraheemdev reviewed on 2024-07-01 19:19_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/toolchain/list.rs`:67 on 2024-07-01 19:19_

Oops, fixed. Need `is_none_or` to stabilize to refactor those.

---

_@konstin approved on 2024-07-01 19:19_

---

_Merged by @ibraheemdev on 2024-07-01 19:28_

---

_Closed by @ibraheemdev on 2024-07-01 19:28_

---

_Branch deleted on 2024-07-01 19:28_

---
