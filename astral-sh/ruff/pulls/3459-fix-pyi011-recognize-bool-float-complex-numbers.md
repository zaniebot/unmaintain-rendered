```yaml
number: 3459
title: "[FIX] PYI011: recognize `Bool` / `Float` / `Complex` numbers as simple defaults"
type: pull_request
state: merged
author: XuehaiPan
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-PYI011
created_at: 2023-03-12T11:47:14Z
updated_at: 2023-03-12T17:42:28Z
url: https://github.com/astral-sh/ruff/pull/3459
synced_at: 2026-01-12T15:55:12Z
```

# [FIX] PYI011: recognize `Bool` / `Float` / `Complex` numbers as simple defaults

---

_@XuehaiPan_

In https://github.com/PyCQA/flake8-pyi/blob/40d547e9624a50f06f0a8369cb2787addabf9ce8/pyi.py#L733-L738

```python
    def _is_valid_Num(node: ast.expr) -> TypeGuard[ast.Num]:
        # The maximum character limit is arbitrary, but here's what it's based on:
        # Hex representation of 32-bit integers tend to be 10 chars.
        # So is the decimal representation of the maximum positive signed 32-bit integer.
        # 0xFFFFFFFF --> 4294967295
        return isinstance(node, ast.Num) and len(str(node.n)) <= 10
```

constant numbers its representation of fewer than 10 characters are considered simple defaults. The definition of `ast.Num` is:

```python
class Num(Constant, metaclass=...):
    value: int | float | complex
```

Currently, `ruff` only considers `int`s are simple defaults. Constant numbers of:

- `bool`: `True` / `False`
- `float`: `3.14`, `2.0`, `0.5`
- `complex`: `2j`, `-3.14j`

are not.

Ref:

- #3238

------

In Python, `bool` is a subclass of `int`, which has only two instances `True` and `False`.

`True` and `False` should be considered simple defaults and should be allowed in `.pyi`.

```python
# test.pyi
def func(flag: bool = False) -> None: ...
```

Output:

```console
$ ruff --version
ruff 0.0.254

$ ruff check --select=PYI test.pyi
test.pyi:2:23: PYI011 Only simple default values allowed for typed arguments
Found 1 error.
```

```console
$ pip3 install -U flake8 flake8-pyi

$ flake8 --select=Y test.pyi
.. # empty output and no violation
```

I think this should be considered a bug in `rustpython_parser::ast::Constant`. It does not recognize `True` / `False` also to be a `Constain::Int(..)` (note that `bool` is a subclass of `int`).

```python
>>> bool.mro()
[bool, int, object]

>>> isinstance(True, int)
True

>>> isinstance(False, int)
True

>>> True == 1
True
>>> hash(True) == hash(1)
True
>>> len({True, 1})
1
>>> False == 0
True
>>> hash(False) == hash(0)
True
>>> len({False, 0})
1
```

---

_Renamed from "[FIX] PYI011: recognize `True` and `False` as simple defaults" to "[FIX] PYI011: recognize `Constant::Bool` and `Constant::Float` as simple defaults" by @XuehaiPan on 2023-03-12 11:51_

---

_Comment by @github-actions[bot] on 2023-03-12 11:59_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Renamed from "[FIX] PYI011: recognize `Constant::Bool` and `Constant::Float` as simple defaults" to "[FIX] PYI011: recognize `Bool` / `Float` / `Complex` numbers as simple defaults" by @XuehaiPan on 2023-03-12 14:15_

---

_Comment by @charliermarsh on 2023-03-12 17:18_

Thanks! This looks right to me.

---

_Label `bug` added by @charliermarsh on 2023-03-12 17:28_

---

_Merged by @charliermarsh on 2023-03-12 17:34_

---

_Closed by @charliermarsh on 2023-03-12 17:34_

---

_Branch deleted on 2023-03-12 17:42_

---
