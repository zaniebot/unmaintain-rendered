---
number: 13024
title: SIM102 Warning when checking value returned from walrus operator
type: issue
state: closed
author: emilal
labels: []
assignees: []
created_at: 2024-08-21T07:31:47Z
updated_at: 2024-08-21T14:53:10Z
url: https://github.com/astral-sh/ruff/issues/13024
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM102 Warning when checking value returned from walrus operator

---

_Issue opened by @emilal on 2024-08-21 07:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In the case of using the walrus operator with an if to weed out `None` values, checking the non-`None` value and then potentially using it for something, ruff gives a SIM102 warning and a non-working suggestion. E.g. this code:

```python
#!/usr/bin/env python3

from typing import Optional


def do_something(n):
    print(n)


def return_something() -> Optional[int]:
    return 42


if k := return_something():
    if k > 3:
        do_something(k)
```

will print 42, but get the following feedback with S enabled:

```
sim102.py:14:1: SIM102 Use a single `if` statement instead of nested `if` statements
   |
14 | / if k := return_something():
15 | |     if k > 3:
   | |_____________^ SIM102
16 |           do_something(k)
   |
   = help: Combine `if` statements using `and`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

However, combining these two if statements does not work:

```python
#!/usr/bin/env python3

from typing import Optional


def do_something(_):
    pass


def return_something() -> Optional[int]:
    return 42


if k := return_something() and k > 3:
    do_something(k)
```

will get the following feedback:

```
sim102e.py:14:32: F821 Undefined name `k`
   |
14 | if k := return_something() and k > 3:
   |                                ^ F821
15 |     do_something(k)
   |

Found 1 error.
```

and if you actually run it:

```
Traceback (most recent call last):
  File "/path/to/sim102e.py", line 14, in <module>
    if k := return_something() and k > 3:
                                   ^
NameError: name 'k' is not defined
```

As far as I can tell, the `if` needs to have branched and concluded whether `k` contains a `None` before we can do another comparison; and SIM102 should not warn for tests using values obtained from walrus operators in previous `if`s.

---

_Comment by @trag1c on 2024-08-21 07:37_

The (unsafe) autofix ruff provides correctly puts the first condition in parentheses:
```
λ ruff check foo.py --select=SIM --fix --unsafe-fixes
Found 1 error (1 fixed, 0 remaining).

λ tail -n 2 foo.py
if (k := return_something()) and k > 3:
    do_something(k)
```

---

_Closed by @emilal on 2024-08-21 14:53_

---
