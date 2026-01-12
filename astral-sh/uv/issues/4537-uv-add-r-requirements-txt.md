```yaml
number: 4537
title: "`uv add -r requirements.txt`"
type: issue
state: closed
author: ibraheemdev
labels:
  - cli
  - preview
assignees: []
created_at: 2024-06-26T04:56:40Z
updated_at: 2024-08-16T21:57:47Z
url: https://github.com/astral-sh/uv/issues/4537
synced_at: 2026-01-12T15:58:50Z
```

# `uv add -r requirements.txt`

---

_@ibraheemdev_

Support a flag that would allow `uv add` to read requirements from a file, e.g. `uv add -r requirements.txt`. This would enable seamless migration from a `requirements.txt` to a `pyproject.toml`.

There's some issues here. We would probably have to use `--raw` to be useful, at least as fallback for git dependencies. We would also need to make sure not to overwrite dependencies with different markers, e.g. `foo>2; python_version > '3.7'` and `foo<2; python_version < '3.7'` should be added as expected. This is at least different from the current behavior of `uv add`, but arguably that should also be changed.

---

_Label `preview` added by @ibraheemdev on 2024-06-26 04:56_

---

_Label `cli` added by @charliermarsh on 2024-07-30 16:10_

---

_Closed by @charliermarsh on 2024-08-16 21:57_

---

_Closed by @charliermarsh on 2024-08-16 21:57_

---
