```yaml
number: 241
title: "`pip-compile` should error if specified extra is not found in any sources"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-10-30T19:51:45Z
updated_at: 2023-10-31T19:17:37Z
url: https://github.com/astral-sh/uv/issues/241
synced_at: 2026-01-12T15:58:22Z
```

# `pip-compile` should error if specified extra is not found in any sources

---

_@zanieb_

Per discussion in https://github.com/astral-sh/puffin/pull/239 — `pip-compile --extra <name>` should error if the name is not present in _any_ of the source files i.e. the name can be missing from one but not all. `requirements.txt` formatted files provide an empty set of extra names. `pyproject.toml` files provide extra names in the `optional-dependencies` keys.

Note this behavior differs from `pip-compile` upstream which does not error or warn in this case.

---

_Closed by @zanieb on 2023-10-31 19:17_

---
