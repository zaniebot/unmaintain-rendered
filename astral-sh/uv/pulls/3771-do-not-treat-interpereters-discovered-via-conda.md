```yaml
number: 3771
title: "Do not treat interpereters discovered via `CONDA_PREFIX` as system interpreters"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/conda-prefix
created_at: 2024-05-22T20:33:31Z
updated_at: 2024-05-22T21:27:34Z
url: https://github.com/astral-sh/uv/pull/3771
synced_at: 2026-01-12T16:05:50Z
```

# Do not treat interpereters discovered via `CONDA_PREFIX` as system interpreters

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/3769

---

_Label `bug` added by @zanieb on 2024-05-22 20:33_

---

_Renamed from "zb/conda prefix" to "Do not treat interpereters discovered via `CONDA_PREFIX` as system interpreters" by @zanieb on 2024-05-22 20:34_

---

_Marked ready for review by @zanieb on 2024-05-22 20:48_

---

_Comment by @zanieb on 2024-05-22 21:24_

This is passing an integration test at https://github.com/astral-sh/uv/pull/3773 and I'd like to get the fix out now.

I've opted not to allow `VIRTUAL_ENV` to point to a system Python installation anymore as described in #3765 so I think this is the right level of change.

---

_Merged by @zanieb on 2024-05-22 21:27_

---

_Closed by @zanieb on 2024-05-22 21:27_

---

_Branch deleted on 2024-05-22 21:27_

---
