```yaml
number: 1917
title: F841 fix results in statement without effect or syntax error for multiline assignments
type: issue
state: closed
author: sbrugman
labels:
  - bug
assignees: []
created_at: 2023-01-16T14:31:46Z
updated_at: 2023-01-16T19:27:42Z
url: https://github.com/astral-sh/ruff/issues/1917
synced_at: 2026-01-12T15:54:41Z
```

# F841 fix results in statement without effect or syntax error for multiline assignments

---

_@sbrugman_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

### Example for statement without effect

```
def fn(a, b):
    sample = a if a is not None \
        else b
    pass
```

Result:

```
def fn(a, b):
   a if a is not None \
        else b
    pass
```

Expected:
```
def fn(a, b):
    pass
```

### Example for invalid syntax

```
def fn(a, b):
    sample = (
        a
        if a is not None
        else b
    )
    pass
```

Result:

```
def fn(a, b):
    a
        if a is not None
        else b
    )
    pass
```

Expected:
```
def fn(a, b):
    pass
```

### To reproduce

`ruff example.py --select F841 --fix`

## Proposed solution
- Make fix take multiline assignments into account (https://github.com/charliermarsh/ruff/blob/main/src/rules/pyflakes/rules/unused_variable.rs#L53)

### Version

ruff 0.0.223

---

_Renamed from "F841 fix results in statement without effect or syntax error" to "F841 fix results in statement without effect or syntax error for multiline assignments" by @sbrugman on 2023-01-16 14:39_

---

_Comment by @charliermarsh on 2023-01-16 16:14_

Definitely looks like a bug (in the latter case — the former was us being conservative, but we could be more aggressive). Will take a look! Thanks for the clear issue.

---

_Label `bug` added by @charliermarsh on 2023-01-16 16:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-16 18:18_

---

_Closed by @charliermarsh on 2023-01-16 19:27_

---
