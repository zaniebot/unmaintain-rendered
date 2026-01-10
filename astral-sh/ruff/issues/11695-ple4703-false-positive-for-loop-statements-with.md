```yaml
number: 11695
title: PLE4703 false positive for loop statements with break
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2024-06-02T17:26:16Z
updated_at: 2024-08-12T14:32:27Z
url: https://github.com/astral-sh/ruff/issues/11695
synced_at: 2026-01-10T11:09:53Z
```

# PLE4703 false positive for loop statements with break

---

_Issue opened by @Skylion007 on 2024-06-02 17:26_

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
```python
my_set = {1, 2, 3, 4}
for a in my_set:
    if a == "2":
        my_set.remove(a)
        break
```
runs fine, but is flagged by PLE4703. If the break statement was not here, it would indeed be an error. However, since it is added after the remove statement before the iterator is accessed again, it is valid. 

Still an issue on `ruff 0.4.7`

---

_Label `bug` added by @charliermarsh on 2024-06-02 17:32_

---

_Comment by @augustelalande on 2024-06-04 02:06_

Not necessarily saying PLE4703 shouldn't be changed, but is there a *useful* example where this comes up?

The construction above should really be rewritten as
```python
my_set.discard("2")
```

---

_Comment by @AlexWaygood on 2024-08-12 14:32_

Thanks for the report! Looks like it was already raised in https://github.com/astral-sh/ruff/issues/10721, however, so I'll fold this into that one :-)

---

_Closed by @AlexWaygood on 2024-08-12 14:32_

---
