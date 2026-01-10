```yaml
number: 3055
title: RET505 False Positive
type: issue
state: closed
author: jasonwashburn
labels: []
assignees: []
created_at: 2023-02-20T11:56:02Z
updated_at: 2023-02-20T15:08:17Z
url: https://github.com/astral-sh/ruff/issues/3055
synced_at: 2026-01-10T11:09:46Z
```

# RET505 False Positive

---

_Issue opened by @jasonwashburn on 2023-02-20 11:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Seems like `RET505` has come up in issues a few times before, but I seem to be getting false positives as well. Here is a pretty simple example of a code snippet that provokes the error.

```python
def is_even(value: int) -> bool:
    """Check if a value is even."""
    if value % 2 == 0:
        print(f"{value} is even")
        return True
    else:
        print(f"{value} is odd")
        return False
```

When running ruff with the defaults (no config file) and adding select for RET505 produces the following:

```bash
❯ ruff --select=RET505 false_positive.py
false_positive.py:6:5: RET505 Unnecessary `else` after `return` statement
Found 1 error.
```

```bash
❯ ruff --version        
ruff 0.0.247
```






---

_Comment by @charliermarsh on 2023-02-20 14:47_

I believe this is actually working as intended -- it wants you to do:

```py
def is_even(value: int) -> bool:
    """Check if a value is even."""
    if value % 2 == 0:
        print(f"{value} is even")
        return True

    print(f"{value} is odd")
    return False
```

Since that code is equivalent: if the number is even, it'll hit the `if`, then `return` at the end of the `if` block. So the `else` isn't necessary.

---

_Closed by @charliermarsh on 2023-02-20 14:47_

---

_Comment by @jasonwashburn on 2023-02-20 15:06_

ugh...you're right...lol, it's so simple I looked right past that...that's what I get for validating/submitting an issue before my second cup of coffee...



---

_Comment by @charliermarsh on 2023-02-20 15:08_

Haha no it’s all good! These rules can be hard to reason about.

---
