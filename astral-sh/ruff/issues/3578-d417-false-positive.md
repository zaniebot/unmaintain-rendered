```yaml
number: 3578
title: D417 false-positive
type: issue
state: closed
author: divaltor
labels:
  - bug
assignees: []
created_at: 2023-03-17T16:18:37Z
updated_at: 2023-03-18T17:14:04Z
url: https://github.com/astral-sh/ruff/issues/3578
synced_at: 2026-01-12T15:54:43Z
```

# D417 false-positive

---

_@divaltor_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello, there is bug with enabled D417 rule (pydocstyle). Tested on **ruff 0.0.256 and Python 3.10**

```python
class Test:
  def run(self, /, arg1: int) -> None:
    """
    Some beauty description.

    Args:
      arg1: some description of arg
    """
```
```shell
$ ruff .
> D417 Missing argument description in the docstring: `self`
```

If we don't mark arguments as positional-keyword only like this
```python
class Test:
  def run(self, arg1: int) -> None:
    """
    Some beauty description.

    Args:
      arg1: some description of arg
    """
```
Then ruff gives clear log

```shell
$ ruff .
> Found 0 errors.
```

---

_Comment by @JonathanPlasse on 2023-03-17 16:34_

I would like to take on this.

---

_Assigned to @JonathanPlasse by @MichaReiser on 2023-03-17 16:37_

---

_Label `bug` added by @MichaReiser on 2023-03-17 16:37_

---

_Closed by @charliermarsh on 2023-03-18 17:14_

---
