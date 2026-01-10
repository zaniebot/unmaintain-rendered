```yaml
number: 14235
title: "UP015 false negatives for many redundant `open` modes"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2024-11-09T22:39:12Z
updated_at: 2024-11-11T03:48:31Z
url: https://github.com/astral-sh/ruff/issues/14235
synced_at: 2026-01-10T11:09:56Z
```

# UP015 false negatives for many redundant `open` modes

---

_Issue opened by @dscorbett on 2024-11-09 22:39_

[`redundant-open-modes` (UP015)](https://docs.astral.sh/ruff/rules/redundant-open-modes/) only recognizes seven redundant modes in Ruff 0.7.3:

https://github.com/astral-sh/ruff/blob/fbf140a665629ce31191e56918bec6a724a24617/crates/ruff_linter/src/rules/pyupgrade/rules/redundant_open_modes.rs#L112-L120

However, many other modes are redundant. `t` and `U` can always be omitted. `r` is redundant unless `b` or `+` is present. The characters can appear in any order; although the rule handles `wt`, for example, it misses `tw`.

More information:

* Python documentation: https://docs.python.org/3.10/library/functions.html#open
* CPython source: https://github.com/python/cpython/blob/v3.10.0/Modules/_io/_iomodule.c#L273
* Full lists of modes: https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/_typeshed/__init__.pyi#L178

---

_Label `bug` added by @dylwil3 on 2024-11-09 22:50_

---

_Label `rule` added by @dylwil3 on 2024-11-09 22:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-11 00:19_

---

_Closed by @charliermarsh on 2024-11-11 03:48_

---

_Closed by @charliermarsh on 2024-11-11 03:48_

---
