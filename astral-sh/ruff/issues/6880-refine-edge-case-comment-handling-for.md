```yaml
number: 6880
title: "Refine edge-case comment handling for `PatternMatchClass`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-25T19:05:49Z
updated_at: 2023-09-02T08:23:02Z
url: https://github.com/astral-sh/ruff/issues/6880
synced_at: 2026-01-12T15:54:46Z
```

# Refine edge-case comment handling for `PatternMatchClass`

---

_@charliermarsh_

In this case, comments `a` and `b` are leading on `2`, but should be leading on a node for the entire `b=2` (this requires an AST change):

```python
    case A(
        b # b
        = # c
        2 # d
        # e
    ):
        pass
```

In this case, the comment should be an open parenthesis comment on the arguments, but is currently a leading comment on `x` due to our generic parenthesized comment handling (which thinks `x` is parenthesized, rather than in a call) (this requires an AST change):
```python
    case Point2D(  # end of line line
            x
            ):
        ...
```


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-25 19:05_

---

_Label `bug` added by @charliermarsh on 2023-08-25 19:05_

---

_Label `formatter` added by @charliermarsh on 2023-08-25 19:05_

---

_Closed by @charliermarsh on 2023-08-26 14:45_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-09-02 08:23_

---
