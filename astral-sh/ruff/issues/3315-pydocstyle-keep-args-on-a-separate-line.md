```yaml
number: 3315
title: "pydocstyle: keep 'Args:' on a separate line"
type: issue
state: open
author: janosh
labels:
  - docstring
assignees: []
created_at: 2023-03-02T23:24:45Z
updated_at: 2023-03-02T23:34:41Z
url: https://github.com/astral-sh/ruff/issues/3315
synced_at: 2026-01-12T15:54:43Z
```

# pydocstyle: keep 'Args:' on a separate line

---

_@janosh_

`ruff check . --select D --fix` currently reformats

```py
class Dummy:
    """Dummy class for testing."""

    def __init__(self, arg1, arg2, arg3):
        """
        Args:
            arg1: some arg
            arg2: some arg
            arg3: some arg
        """
        pass
```

to

```py

class Dummy:
    """Dummy class for testing."""

    def __init__(self, arg1, arg2, arg3):
        """Args:
        arg1: some arg
        arg2: some arg
        arg3: some arg.
        """
        pass
```

The de-indenting of args in particular is bad. Could we give `'Args':` special treatment and always leave it on its own line?

Related issue: https://github.com/PyCQA/pydocstyle/issues/189.

---

_Label `docstring` added by @charliermarsh on 2023-03-02 23:29_

---

_Comment by @charliermarsh on 2023-03-02 23:29_

Ohh interesting, is that because it wants the docstring to start on the first (summary) line?

---

_Comment by @janosh on 2023-03-02 23:30_

That's right.

---

_Comment by @charliermarsh on 2023-03-02 23:32_

Are you thinking that we should avoid autofixing this, or avoid flagging it as an error altogether?

---

_Comment by @janosh on 2023-03-02 23:33_

I'd prefer not flagging altogether.

---

_Comment by @janosh on 2023-03-02 23:34_

In the [linked pydocstyle discussion](https://github.com/PyCQA/pydocstyle/issues/189), I think they make a good case for having the summary line in the class doc string and therefore not needing another one in `__init__`.

---
