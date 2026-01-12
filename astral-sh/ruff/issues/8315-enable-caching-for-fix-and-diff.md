```yaml
number: 8315
title: "Enable caching for `--fix` and `--diff`"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-28T21:29:36Z
updated_at: 2023-10-29T16:06:37Z
url: https://github.com/astral-sh/ruff/issues/8315
synced_at: 2026-01-12T15:54:48Z
```

# Enable caching for `--fix` and `--diff`

---

_@charliermarsh_

Right now, we completely disable caching when you run with `--fix` and `--diff`, the reason being that the caching exists at a higher level (we save `Diagnostics`), and those commands rely on side-effects that happen during the creation of `Diagnostics`.

It's highly inefficient to avoid caching there!


---

_Comment by @charliermarsh on 2023-10-28 21:31_

I think we can at least read from the cache for files with no errors, which will give us high lift without any refactoring. I'll do that...

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-28 22:34_

---

_Label `performance` added by @charliermarsh on 2023-10-28 22:34_

---

_Closed by @charliermarsh on 2023-10-29 16:06_

---
