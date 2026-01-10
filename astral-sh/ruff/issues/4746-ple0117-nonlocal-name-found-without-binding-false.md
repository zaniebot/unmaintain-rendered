---
number: 4746
title: PLE0117 - nonlocal name found without binding - false possitive
type: issue
state: closed
author: rmorshea
labels:
  - bug
assignees: []
created_at: 2023-05-31T08:04:51Z
updated_at: 2023-06-06T18:49:38Z
url: https://github.com/astral-sh/ruff/issues/4746
synced_at: 2026-01-10T01:22:43Z
---

# PLE0117 - nonlocal name found without binding - false possitive

---

_Issue opened by @rmorshea on 2023-05-31 08:04_

- version - 0.0.270
- command `ruff .`
- no especially relevant settings

It seems that [`PLE0117`](https://beta.ruff.rs/docs/rules/nonlocal-without-binding/) erroneously considers `del` statements when determining if a variable as been assigned

```python
def f():
    x = 1

    def g():
        nonlocal x  # error: PLE0117
        x = 2

    del x
```

---

_Label `bug` added by @charliermarsh on 2023-05-31 15:13_

---

_Comment by @charliermarsh on 2023-05-31 15:13_

Agreed.

---

_Referenced in [astral-sh/ruff-lsp#128](../../astral-sh/ruff-lsp/issues/128.md) on 2023-06-05 05:13_

---

_Referenced in [astral-sh/ruff#4888](../../astral-sh/ruff/pulls/4888.md) on 2023-06-06 01:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-06 01:30_

---

_Closed by @charliermarsh on 2023-06-06 18:49_

---
