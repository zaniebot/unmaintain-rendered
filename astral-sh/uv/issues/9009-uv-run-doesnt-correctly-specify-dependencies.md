---
number: 9009
title: "`uv run` doesn’t correctly specify dependencies"
type: issue
state: closed
author: flying-sheep
labels: []
assignees: []
created_at: 2024-11-11T08:37:30Z
updated_at: 2024-11-11T08:38:45Z
url: https://github.com/astral-sh/uv/issues/9009
synced_at: 2026-01-10T01:24:35Z
---

# `uv run` doesn’t correctly specify dependencies

---

_Issue opened by @flying-sheep on 2024-11-11 08:37_

`foo.py`:

```py
# /// script
# dependencies = [
#   "tomllib; python_version < '3.11'",
# ]
# ///
```

```console
$ uv run foo.py
Reading inline script metadata from `foo.py`
  × No solution found when resolving script dependencies:
  ╰─▶ Because there are no versions of tomllib{python_full_version < '3.11'} and you require tomllib{python_full_version < '3.11'}, we can conclude that your requirements are unsatisfiable.
```

---

_Comment by @flying-sheep on 2024-11-11 08:38_

Oops, I was typing before awake. Of course `tomllib` isn‘t correct.

---

_Closed by @flying-sheep on 2024-11-11 08:38_

---
