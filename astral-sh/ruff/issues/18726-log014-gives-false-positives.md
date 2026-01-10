```yaml
number: 18726
title: LOG014 Gives false positives
type: issue
state: closed
author: hmvp
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-06-17T15:48:33Z
updated_at: 2025-06-20T18:43:09Z
url: https://github.com/astral-sh/ruff/issues/18726
synced_at: 2026-01-10T11:09:58Z
```

# LOG014 Gives false positives

---

_Issue opened by @hmvp on 2025-06-17 15:48_

### Summary

LOG014 should report `exc_info=True` but also reports on `exc_info=(exc_type, exc_value, exc_traceback)` which is valid but because it is truthy it reports a lint error.

Besides `True` and a tuple any instance of `BaseException` is also valid

### Version

ruff 0.12.0

---

_Renamed from "LOG014 Is gives false positives" to "LOG014 Gives false positives" by @hmvp on 2025-06-17 15:48_

---

_Label `bug` added by @ntBre on 2025-06-17 17:04_

---

_Label `rule` added by @ntBre on 2025-06-17 17:04_

---

_Comment by @ntBre on 2025-06-17 17:09_

Thanks for the report! This looks like a departure from the upstream linter's behavior too:

```python
# try.py
import logging

logging.warning("warning", exc_info=(True,))
```

```shell
> uvx --with flake8-logging flake8 try.py --select LOG014
> echo $?
0
```

They also check if the value is an [`ast.Constant`](https://docs.python.org/3/library/ast.html#ast.Constant) ([source](https://github.com/adamchainz/flake8-logging/blob/dbe88e76947450a7c4c1ebfb7a7a44875004f410/src/flake8_logging/__init__.py#L334-L338)) whereas we only check if it's truthy:

https://github.com/astral-sh/ruff/blob/05e28451bec6f3a00750e1e1445f6f8b5cb8ebeb/crates/ruff_linter/src/rules/flake8_logging/rules/exc_info_outside_except_handler.rs#L101

---

_Label `help wanted` added by @ntBre on 2025-06-17 17:09_

---

_Closed by @ntBre on 2025-06-20 18:43_

---
