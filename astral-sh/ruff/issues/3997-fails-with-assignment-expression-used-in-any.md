```yaml
number: 3997
title: "Fails with assignment expression used in \"any()\" function"
type: issue
state: closed
author: lorien
labels:
  - bug
assignees: []
created_at: 2023-04-17T14:46:42Z
updated_at: 2023-04-29T22:45:32Z
url: https://github.com/astral-sh/ruff/issues/3997
synced_at: 2026-01-12T15:54:44Z
```

# Fails with assignment expression used in "any()" function

---

_@lorien_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Content of "test.py":
```python
if any((key := x) for x in ["ok"]):
    print(key)
```
Ruff command:
```
ruff --isolated test.py
```

Ruff output:
```
test.py:2:11: F821 Undefined name `key`
```

Ruff version: 0.0.261

---

_Renamed from "Fails with assignment expression used in "any" function" to "Fails with assignment expression used in "any()" function" by @lorien on 2023-04-17 14:55_

---

_Comment by @charliermarsh on 2023-04-17 15:47_

Hmm yeah, I think we can err on the side of considering this a bug since Mypy and Pyright don't flag `key` here.

I will point out, though, that in the general case, it's hard to guarantee that `key` is defined here, e.g., this also passes Mypy and Pyright but errors at runtime:

```py
if all((key := x) for x in []):
    print(key)
```

---

_Label `bug` added by @charliermarsh on 2023-04-17 15:48_

---

_Comment by @lorien on 2023-04-17 15:58_

```python
if all((key := x) for x in []):
    print(key)
```

To me it looks like bad language design and might be considered as bug in python itself.

UPD: It does not look wrong to me anymore. The logic is that "all()" return True for empty list or empty iterator. It does not know anything about what happened inside iterator. And nothing was happened there, including not running code "(key := x)" because there is nothing to run it fore. I could not call it a bug, but maybe a bad design. 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-18 02:35_

---

_Comment by @charliermarsh on 2023-04-18 03:20_

Yeah it's "correct" behavior (`any` is false on empty lists, `all` is true), it's just that we have no way of knowing whether `key` will be defined within the loop. It looks like other static tools assume it will be.

---

_Comment by @charliermarsh on 2023-04-18 03:25_

Looks like it is formally defined that the `NamedExpr` needs to be treated as a definition in the parent scope: https://peps.python.org/pep-0572/#scope-of-the-target

---

_Comment by @zanieb on 2023-04-25 01:15_

Is ruff capable of detecting empty iterators? It seems like the name will always be defined otherwise. It seems like if we cannot tell if it is empty, we must assume that it is not and that the name is indeed defined.

---

_Comment by @charliermarsh on 2023-04-25 04:18_

We don't have that capability right now (to detect empty iterators). Agree that we should assume that the comprehension will execute, and that the name will thus be added to the parent scope. I started on this a bit, but it requires some changes to our semantic model (to add a binding to the non-current scope).

---

_Closed by @charliermarsh on 2023-04-29 22:45_

---
