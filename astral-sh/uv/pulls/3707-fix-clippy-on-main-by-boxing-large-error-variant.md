```yaml
number: 3707
title: Fix clippy on main by boxing large error variant
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-windows-clippy-main
created_at: 2024-05-21T17:24:16Z
updated_at: 2024-05-21T17:56:24Z
url: https://github.com/astral-sh/uv/pull/3707
synced_at: 2026-01-10T14:32:20Z
```

# Fix clippy on main by boxing large error variant

---

_Pull request opened by @konstin on 2024-05-21 17:24_

I don't really understand why this only happens on windows clippy and not on linux too, but as usual, boxing the error variant fixes it.

Fixup for #3585


---

_Label `internal` added by @konstin on 2024-05-21 17:24_

---

_@zanieb approved on 2024-05-21 17:26_

---

_Comment by @ibraheemdev on 2024-05-21 17:54_

`PathBuf` is [a byte larger on Windows](https://github.com/rust-lang/rust/blob/master/library/std/src/sys_common/wtf8.rs#L145), which is why this wasn't triggered anywhere else.

---

_@ibraheemdev approved on 2024-05-21 17:54_

---

_Merged by @konstin on 2024-05-21 17:55_

---

_Closed by @konstin on 2024-05-21 17:55_

---

_Branch deleted on 2024-05-21 17:55_

---

_Comment by @charliermarsh on 2024-05-21 17:55_

Wow

---

_Comment by @charliermarsh on 2024-05-21 17:56_

@ibraheemdev is a fount of knowledge

---
