```yaml
number: 7273
title: False positive redefined-loop-name (PLW2901) with in-place augmented assignment
type: issue
state: closed
author: mihaic
labels: []
assignees: []
created_at: 2023-09-11T16:44:05Z
updated_at: 2023-09-12T01:08:53Z
url: https://github.com/astral-sh/ruff/issues/7273
synced_at: 2026-01-12T15:54:46Z
```

# False positive redefined-loop-name (PLW2901) with in-place augmented assignment

---

_@mihaic_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Considering the following contents of `issue.py` (no other files, no `pyproject.toml`):
```python
mutables = ([0, 1, 2], [10, 11, 12])
for mutable in mutables:
    mutable += [100]
```

PLW2901 is detected:
```
ruff check --select PLW2901 --isolated issue.py
issue.py:3:5: PLW2901 `for` loop variable `mutable` overwritten by assignment target
Found 1 error.
```

In this case, the augmented assignment is performed in-place and is equivalent to calling `extend` (which does not trigger PLW2901).

```
ruff --version
ruff 0.0.287
```

---

_Comment by @charliermarsh on 2023-09-11 23:11_

Hmm, I think there are reasonable arguments both ways but I'm going to opt to leave this as-is for now. Pylint has the same behavior (`W2901: Redefining 'mutable' from loop (line 2) (redefined-loop-name)`), and the variable _is_ being reassigned here. This strikes me as the rule working as-intended.

---

_Closed by @charliermarsh on 2023-09-11 23:11_

---

_Comment by @mihaic on 2023-09-11 23:28_

Thanks for your quick response. I find `redefined-loop-name` generally useful. Considering that Ruff behavior will stay the same, do you have any thoughts on dealing with similar code? I can think of using the method form of in-place augmented assignment, but verbosity is an issue, e.g., `set.symmetric_difference_update` vs. `^=`.

---

_Comment by @charliermarsh on 2023-09-12 01:08_

My honest recommendation would be to use the in-place update methods. Even in the example above, I find the augmented assignment a bit less clear, since, e.g., _this_ doesn't work:

```python
mutables = ([0, 1, 2], [10, 11, 12])
for mutable in mutables:
    mutable = mutable + [100]
```

In other words, it's relying on the fact that Python uses in-place operations for data types that support it.

Alternatively, you could assign `mutable` to a temporary variable to avoid the lint error, or just use a `noqa` in these cases.


---
