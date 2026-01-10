```yaml
number: 7347
title: "CLI: Invocation without arguments should check the current directory"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:01:52Z
updated_at: 2023-11-21T17:34:23Z
url: https://github.com/astral-sh/ruff/issues/7347
synced_at: 2026-01-10T11:09:49Z
```

# CLI: Invocation without arguments should check the current directory

---

_Issue opened by @zanieb on 2023-09-13 16:01_

Currently `ruff check` requires an input path. Instead, it should default to checking the current directory.

---

_Label `cli` added by @zanieb on 2023-09-13 16:01_

---

_Renamed from "Check: Invocation without arguments should check the current directory" to "CLI: Invocation without arguments should check the current directory" by @zanieb on 2023-09-13 16:04_

---

_Comment by @MichaReiser on 2023-09-13 16:04_

To make sure we're aligned. Does that mean it checks the current directory respecting the `exclude` and `include` configurations? Meaning, it wouldn't check anything if `exclude = ["**"]`

---

_Comment by @charliermarsh on 2023-09-13 16:07_

(I would assume so -- in other words, it would have the same semantics as `ruff check .`?)

---

_Comment by @bluetech on 2023-09-14 06:43_

Various tools have a configuration for which directories to use when invoked with no arguments.
Mypy has [`files`](https://mypy.readthedocs.io/en/stable/config_file.html#confval-files), `modules`, `packages`.
Pytest has [`testpaths`](https://docs.pytest.org/en/7.4.x/reference/reference.html#confval-testpaths).
I find it useful.

---

_Comment by @zanieb on 2023-09-14 20:40_

Thanks for the resources! We should respect the `include` and `exclude` configuration options (and similar).

---

_Comment by @dhruvmanila on 2023-09-15 03:31_

Related: #3970 

---

_Closed by @zanieb on 2023-11-21 17:34_

---
