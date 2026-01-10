```yaml
number: 17367
title: "Support relative paths in `UV_PYTHON_BIN_DIR` and `UV_TOOL_BIN_DIR`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: claude/parse-path-override-variable-i5AT4
created_at: 2026-01-08T21:54:56Z
updated_at: 2026-01-09T13:23:21Z
url: https://github.com/astral-sh/uv/pull/17367
synced_at: 2026-01-10T05:49:14Z
```

# Support relative paths in `UV_PYTHON_BIN_DIR` and `UV_TOOL_BIN_DIR`

---

_Pull request opened by @zanieb on 2026-01-08 21:54_

Closes #17364

Extends #17365 

---

_Label `enhancement` added by @zanieb on 2026-01-08 21:55_

---

_Marked ready for review by @zanieb on 2026-01-08 22:13_

---

_Review comment by @benjyw on `crates/uv-dirs/src/lib.rs`:91 on 2026-01-09 01:04_

Why special-case an empty string? Couldn't that usefully mean "the cwd itself"? 

---

_@benjyw reviewed on 2026-01-09 01:05_

Thanks for the change! I have one thought:

---

_@zanieb reviewed on 2026-01-09 13:21_

---

_Review comment by @zanieb on `crates/uv-dirs/src/lib.rs`:91 on 2026-01-09 13:21_

It's for consistency with the rest of our variables. People want to the ability to "unset" a variable using an empty value. I think you should use `.` to target the CWD.

---

_Merged by @zanieb on 2026-01-09 13:21_

---

_Closed by @zanieb on 2026-01-09 13:21_

---

_Comment by @benjyw on 2026-01-09 13:23_

That makes sense. Thanks again for the quick turnaround!

---
