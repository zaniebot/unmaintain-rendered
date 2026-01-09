---
number: 8906
title: PIE810 gives wrong suggestions
type: issue
state: closed
author: deliro
labels:
  - bug
assignees: []
created_at: 2023-11-29T08:36:56Z
updated_at: 2024-01-28T19:24:37Z
url: https://github.com/astral-sh/ruff/issues/8906
synced_at: 2026-01-07T13:12:15-06:00
---

# PIE810 gives wrong suggestions

---

_Issue opened by @deliro on 2023-11-29 08:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hey! Given the code:

```python
import sys

if __name__ == "__main__":
    x = ("hello", "world")
    y = "foo"

    msg = "abracadabra"

    if msg.startswith(x) or msg.startswith(y):
        sys.exit(1)
```

Ruff suggests (`ruff check x.py`):

```
x.py:9:8: PIE810 Call `startswith` once with a `tuple`
```

The problem is `x` is a tuple and the expression cannot be combined to `msg.startswith((x, y))`

##### Ruff settings

```yaml
[tool.ruff]
line-length = 120
target-version = "py311"
exclude = ["*migrations*"]
select = [
    "E", # pyflakes
    "F", # pycodestyle errors
    "W", # pycodestyle warnings
    "UP", # pyupgrade
    "I", # isort
    "C4", # flake8-comprehensions
    # pytest
    "PT018", # Assertion should be broken down into multiple parts
    "PT022", # No teardown in fixture {name}, use return instead of yield

    "T10", # flake8-debugger
    "SIM", # flake8-simplify
    "ISC", # flake8-implicit-str-concat
    "FA", # flake8-future-annotations
    "PIE", # flake8-pie
    "T20", # flake8-print
    "RET", # flake8-return
    "FLY", # flynt
]
ignore = [
    "E741", # Ambiguous variable name
    "SIM115", # Use context handler for opening files
    "SIM116", # Use a dictionary instead of consecutive `if` statements
    "RET501", # Do not explicitly return None in function if it is the only possible return value
    # broken ones in RET:
    "RET505", # Unnecessary `elif` after `return` statement
    "RET506", # Unnecessary `elif` after `raise` statement
    "RET507", # Unnecessary `elif` after `continue` statement
    "RET508", # Unnecessary {branch} after break statement
]
```

##### Ruff version

ruff 0.1.6 (f460f9c5c 2023-11-17)

---

_Comment by @prokie on 2023-11-29 08:46_

I guess you can do like this?

```python
if msg.startswith(x + (y,)):
    sys.exit(1)
```

---

_Comment by @deliro on 2023-11-29 08:50_

I can, but the point is ruff suggests invalid code that will throw

---

_Label `bug` added by @charliermarsh on 2023-11-29 14:53_

---

_Comment by @charliermarsh on 2023-11-29 14:53_

Makes sense, thanks!

---

_Comment by @dhruvmanila on 2023-11-30 21:54_

I guess one solution would be to avoid raising the violation if we're sure that it's a collection type like the mentioned code:

```python
x = ["a", "b"]
y = "c"

msg.startswith(x) or msg.startswith(y)
```

But, if there are more than 1 `startswith` calls which has non-collection type or an unknown type, then we can raise an error:

```python
x = ["a", "b"]
y = "c"
z = "d"

msg.startswith(x) or msg.startswith(y) or msg.startswith(z)
#                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#                    Raise here
```

---

_Referenced in [astral-sh/ruff#9661](../../astral-sh/ruff/pulls/9661.md) on 2024-01-28 12:11_

---

_Comment by @charliermarsh on 2024-01-28 19:24_

Closed by https://github.com/astral-sh/ruff/pull/9661.

---

_Closed by @charliermarsh on 2024-01-28 19:24_

---

_Referenced in [ArduPilot/ardupilot#30343](../../ArduPilot/ardupilot/pulls/30343.md) on 2025-06-14 09:04_

---
