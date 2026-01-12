```yaml
number: 7663
title: "Remove `serde::Serialize` implementations for rkyv-able structs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rkyv
created_at: 2024-09-24T17:05:05Z
updated_at: 2024-09-24T17:23:50Z
url: https://github.com/astral-sh/uv/pull/7663
synced_at: 2026-01-12T16:07:56Z
```

# Remove `serde::Serialize` implementations for rkyv-able structs

---

_@charliermarsh_

## Summary

Random, but I noticed that we can remove a ton of serialize and deserialize derives by using `rkyv` for the flat-index caches. (We already use `rkyv` for these same structs in the registry cache.)


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-24 17:05_

---

_Label `internal` added by @charliermarsh on 2024-09-24 17:05_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:354 on 2024-09-24 17:05_

Sigh, these need `Serialize` for testing. Wonder if we should just use the debug repr though.

---

_@charliermarsh reviewed on 2024-09-24 17:05_

---

_Review comment by @BurntSushi on `crates/uv-workspace/src/pyproject.rs`:354 on 2024-09-24 17:10_

`Debug` repr for tests seems fine to me.

---

_@BurntSushi approved on 2024-09-24 17:10_

Nice!

---

_@charliermarsh reviewed on 2024-09-24 17:17_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:354 on 2024-09-24 17:17_

It's kind of bad because we have glob patterns and the debug repr is very verbose.

---

_Merged by @charliermarsh on 2024-09-24 17:23_

---

_Closed by @charliermarsh on 2024-09-24 17:23_

---

_Branch deleted on 2024-09-24 17:23_

---
