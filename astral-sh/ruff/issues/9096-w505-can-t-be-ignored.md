```yaml
number: 9096
title: "`W505` can't be ignored"
type: issue
state: closed
author: tylerlaprade
labels: []
assignees: []
created_at: 2023-12-11T17:23:18Z
updated_at: 2023-12-11T17:30:41Z
url: https://github.com/astral-sh/ruff/issues/9096
synced_at: 2026-01-10T11:09:51Z
```

# `W505` can't be ignored

---

_Issue opened by @tylerlaprade on 2023-12-11 17:23_

Hi, unless I messed up the syntax, it seems that `W505` violations can't be ignored with `noqa` when `max-doc-length` is set.

<img width="943" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/b3f951ac-a206-4f44-a1b5-c5241e4f4049">

---

_Comment by @charliermarsh on 2023-12-11 17:30_

Is that within a multiline string? If so, you need to put the `# noqa` after the end of the multiline string, rather than _within_ it.

---

_Comment by @tylerlaprade on 2023-12-11 17:30_

Ah, looks like it has to be at the end of the docstring

EDIT: jinx!

---

_Closed by @tylerlaprade on 2023-12-11 17:30_

---
