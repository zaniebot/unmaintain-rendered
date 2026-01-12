```yaml
number: 2551
title: "\"Stray quotes\" fixup can lead to invalid markers"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2024-03-19T19:22:00Z
updated_at: 2024-04-23T18:07:53Z
url: https://github.com/astral-sh/uv/issues/2551
synced_at: 2026-01-12T15:58:38Z
```

# "Stray quotes" fixup can lead to invalid markers

---

_@charliermarsh_

The "stray quotes" fixup will remove quotes from markers, e.g., `numpy (>=1.19) ; python_version >= "3.7"` gets rewritten as `numpy (>=1.19) ; python_version >= 3.7`.


---

_Label `bug` added by @charliermarsh on 2024-03-19 19:22_

---

_Label `good first issue` added by @charliermarsh on 2024-03-25 14:33_

---

_Label `help wanted` added by @charliermarsh on 2024-03-25 14:33_

---

_Comment by @charliermarsh on 2024-03-25 14:33_

This is all in `crates/pypi-types/src/lenient_requirement.rs`.

---

_Comment by @fatelei on 2024-03-25 16:03_

i want to have a try

---

_Comment by @zanieb on 2024-03-25 16:48_

Just let us know here or in a draft pull request if you need any help!

---

_Closed by @charliermarsh on 2024-04-23 18:07_

---
