```yaml
number: 3189
title: "Add `--python` support to `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/uv-run-python
created_at: 2024-04-22T16:44:13Z
updated_at: 2024-04-24T16:03:51Z
url: https://github.com/astral-sh/uv/pull/3189
synced_at: 2026-01-10T14:37:54Z
```

# Add `--python` support to `uv run`

---

_Pull request opened by @zanieb on 2024-04-22 16:44_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-04-22 16:44_

---

_Renamed from "zb/uv run python" to "Add `--python` support to `uv run`" by @zanieb on 2024-04-22 16:46_

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:1668 on 2024-04-23 07:24_

Do we want `-p` too`

---

_@konstin approved on 2024-04-23 07:26_

---

_@zanieb reviewed on 2024-04-23 13:09_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1668 on 2024-04-23 13:09_

Probably? 

---

_@konstin reviewed on 2024-04-23 13:59_

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:1668 on 2024-04-23 13:59_

I want it :D

---

_Comment by @zanieb on 2024-04-24 15:41_

```
❯ cargo run -- run -p 3.11 --no-workspace -- python --version
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/uv run -p 3.11 --no-workspace -- python --version`
Resolved 0 packages in 5ms
Audited 0 packages in 0.38ms
Python 3.11.7
```

---

_Merged by @zanieb on 2024-04-24 16:03_

---

_Closed by @zanieb on 2024-04-24 16:03_

---

_Branch deleted on 2024-04-24 16:03_

---
