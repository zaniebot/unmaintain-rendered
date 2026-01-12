```yaml
number: 10374
title: "E303: missing detection"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2024-03-13T00:11:07Z
updated_at: 2024-03-14T08:35:26Z
url: https://github.com/astral-sh/ruff/issues/10374
synced_at: 2026-01-12T15:54:50Z
```

# E303: missing detection

---

_@spaceone_

E303 not detected for:

```python
#



import foo


def bar() -> list[str]:
    pass
```

flake8 detects: `:5:1: E303 too many blank lines (3)`


---

_Comment by @hikaru-kajita on 2024-03-13 07:23_

I have tried all errors from `E301` to `E306` with `--preview` option, but none of them worked.

---

_Comment by @dhruvmanila on 2024-03-13 07:32_

Thanks for the report.

From the implementation side, I guess this must be a bug in the order in which the check is performed:

https://github.com/astral-sh/ruff/blob/dacec7377c436fc85e380f7cc69a007824fa08bc/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs#L699-L701

and the state is being updated:

https://github.com/astral-sh/ruff/blob/dacec7377c436fc85e380f7cc69a007824fa08bc/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs#L735-L736

/cc @hoel-bagard

---

_Label `bug` added by @dhruvmanila on 2024-03-13 07:32_

---

_Comment by @hoel-bagard on 2024-03-13 12:52_

I'll have a look at it, thanks for reporting the bug.

---

_Closed by @dhruvmanila on 2024-03-14 08:35_

---
