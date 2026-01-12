```yaml
number: 4224
title: "Fix `pip_compile::missing_venv` test"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/fixup-main
created_at: 2024-06-10T22:34:19Z
updated_at: 2024-06-10T22:39:26Z
url: https://github.com/astral-sh/uv/pull/4224
synced_at: 2026-01-12T16:06:06Z
```

# Fix `pip_compile::missing_venv` test

---

_@zanieb_

A merge kerfuffle from #4222  and #4218 

Now we fail because we genuinely can't find any interpreters since tests contexts are isolated by default. I'll improve the error message and maybe add another test case once `main` is fixed.

---

_Label `internal` added by @zanieb on 2024-06-10 22:34_

---

_Merged by @zanieb on 2024-06-10 22:39_

---

_Closed by @zanieb on 2024-06-10 22:39_

---

_Branch deleted on 2024-06-10 22:39_

---
