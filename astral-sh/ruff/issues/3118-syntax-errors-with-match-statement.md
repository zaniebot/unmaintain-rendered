```yaml
number: 3118
title: Syntax errors with match statement
type: issue
state: closed
author: rslinckx
labels:
  - bug
assignees: []
created_at: 2023-02-22T11:15:01Z
updated_at: 2023-02-22T16:17:16Z
url: https://github.com/astral-sh/ruff/issues/3118
synced_at: 2026-01-12T15:54:43Z
```

# Syntax errors with match statement

---

_@rslinckx_

Here are 2 failing syntaxes for match statements using the latest 0.0.251 release. They both run fine but it seems to use more "esoteric" syntax from the match spec.

```
match {"test": 1}:
    case {
        **rest,
    }:
        print(rest)

```

```
match {"label": "test"}:
    case {
        "label": str() | None as label,
    }:
        print(label)

```

---

_Label `bug` added by @charliermarsh on 2023-02-22 12:17_

---

_Comment by @charliermarsh on 2023-02-22 12:19_

Thank you! I’ll take a look at these today.

---

_Comment by @charliermarsh on 2023-02-22 15:02_

This will be fixed by https://github.com/RustPython/RustPython/pull/4551.

---

_Closed by @charliermarsh on 2023-02-22 16:17_

---
