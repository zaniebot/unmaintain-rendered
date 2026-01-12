```yaml
number: 993
title: D101 does not consider __all__
type: issue
state: closed
author: onerandomusername
labels:
  - bug
  - docstring
assignees: []
created_at: 2022-12-02T05:17:46Z
updated_at: 2023-05-11T17:43:53Z
url: https://github.com/astral-sh/ruff/issues/993
synced_at: 2026-01-12T15:54:40Z
```

# D101 does not consider __all__

---

_@onerandomusername_

A class without a `_` preceeding its name is not public unless included in `__all__` when `__all__` is defined.

For example, the following file should not raise a D101.

```py
__all__ = ()


class Hello:
    ...

```

---

_Comment by @charliermarsh on 2022-12-02 05:32_

Cool, this makes sense.

---

_Label `bug` added by @charliermarsh on 2022-12-02 05:33_

---

_Label `docstring` added by @charliermarsh on 2022-12-31 18:17_

---

_Comment by @henryborchers on 2023-05-04 14:56_

"Publicity" is the case for all in the D1xx group, not just D101. If there is an `__all__` in the module, only the items listed as part of it should receive these checks.  [See here for details.](https://www.pydocstyle.org/en/2.1.1/error_codes.html#publicity)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-04 19:56_

---

_Comment by @charliermarsh on 2023-05-05 03:11_

I'm working on this, but it requires some refactoring, since we can no longer track visibility as we iterate over the AST (since we might visit nodes before we see `__all__`).


---

_Comment by @henryborchers on 2023-05-08 14:30_

@charliermarsh  BTW, I heard you on Talk Python. We need more people like you.

---

_Closed by @charliermarsh on 2023-05-11 17:43_

---
