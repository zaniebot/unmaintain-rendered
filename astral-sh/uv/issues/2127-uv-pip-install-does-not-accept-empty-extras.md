---
number: 2127
title: uv pip install does not accept empty extras
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-03-02T04:17:42Z
updated_at: 2024-03-02T20:36:30Z
url: https://github.com/astral-sh/uv/issues/2127
synced_at: 2026-01-10T01:23:12Z
---

# uv pip install does not accept empty extras

---

_Issue opened by @hauntsaninja on 2024-03-02 04:17_

@AlexWaygood ran into this in https://github.com/python/typeshed/pull/11517. I didn't see an issue from him, so here one is :-)
```
λ pip install 'pypyp[]'
Requirement already satisfied: pypyp in .../lib/python3.11/site-packages (1.1.0)
λ uv pip install 'pypyp[]'
error: Failed to parse `pypyp[]`
  Caused by: Expected an alphanumeric character starting the extra name, found ']'
pypyp[]
      ^
λ python
Python 3.11.5 (main, Nov  2 2023, 22:21:08) [Clang 15.0.0 (clang-1500.0.40.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import packaging.requirements
>>> packaging.requirements.Requirement("pypyp[]")
<Requirement('pypyp')>
```

---

_Label `bug` added by @charliermarsh on 2024-03-02 04:28_

---

_Label `good first issue` added by @charliermarsh on 2024-03-02 04:28_

---

_Comment by @charliermarsh on 2024-03-02 04:28_

Oh wow, didn't know this was allowed, thank you!

---

_Referenced in [astral-sh/uv#2128](../../astral-sh/uv/pulls/2128.md) on 2024-03-02 09:49_

---

_Comment by @charliermarsh on 2024-03-02 20:28_

Closed thanks to @SigureMo!

---

_Closed by @charliermarsh on 2024-03-02 20:36_

---
