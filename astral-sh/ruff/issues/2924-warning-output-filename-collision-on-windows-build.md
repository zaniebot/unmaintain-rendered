```yaml
number: 2924
title: "warning: output filename collision on windows build"
type: issue
state: closed
author: messense
labels:
  - release
assignees: []
created_at: 2023-02-15T14:50:55Z
updated_at: 2023-06-08T17:13:44Z
url: https://github.com/astral-sh/ruff/issues/2924
synced_at: 2026-01-12T15:54:43Z
```

# warning: output filename collision on windows build

---

_@messense_

```
warning: output filename collision.
The bin target `ruff` in package `ruff_cli v0.0.246 (D:\a\ruff\ruff\crates\ruff_cli)` has the same output filename as the lib target `ruff` in package `ruff v0.0.246 (D:\a\ruff\ruff\crates\ruff)`.
Colliding filename is: D:\a\ruff\ruff\target\x86_64-pc-windows-msvc\release\deps\ruff.pdb
The targets should have unique names.
Consider changing their names to be unique or compiling them separately.
This may become a hard error in the future; see <https://github.com/rust-lang/cargo/issues/6313>.
```

https://github.com/charliermarsh/ruff/actions/runs/4159851837/jobs/7196287945#step:5:442

---

_Label `release` added by @charliermarsh on 2023-02-15 14:52_

---

_Comment by @charliermarsh on 2023-06-08 17:13_

This appears to no longer be happening.

---

_Closed by @charliermarsh on 2023-06-08 17:13_

---
