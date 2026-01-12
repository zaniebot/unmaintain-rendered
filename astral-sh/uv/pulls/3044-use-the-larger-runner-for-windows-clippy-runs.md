```yaml
number: 3044
title: Use the larger runner for Windows clippy runs
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/windows-clippy
created_at: 2024-04-15T21:08:56Z
updated_at: 2024-04-16T07:55:50Z
url: https://github.com/astral-sh/uv/pull/3044
synced_at: 2026-01-12T16:05:23Z
```

# Use the larger runner for Windows clippy runs

---

_@zanieb_

Running clippy can apparently be longer than the tests in https://github.com/astral-sh/uv/pull/3040

---

_Label `internal` added by @zanieb on 2024-04-15 21:08_

---

_Marked ready for review by @zanieb on 2024-04-15 21:12_

---

_@charliermarsh approved on 2024-04-16 00:34_

---

_Merged by @zanieb on 2024-04-16 02:16_

---

_Closed by @zanieb on 2024-04-16 02:16_

---

_Branch deleted on 2024-04-16 02:16_

---

_Comment by @konstin on 2024-04-16 07:55_

Another option is to cross-run clippy on a linux machine:

```
cargo xwin clippy --target x86_64-pc-windows-msvc
```

---
