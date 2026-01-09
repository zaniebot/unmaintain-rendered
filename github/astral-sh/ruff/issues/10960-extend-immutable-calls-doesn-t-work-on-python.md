---
number: 10960
title: "extend-immutable-calls doesn't work on Python scripts (as opposed to modules)"
type: issue
state: closed
author: fofoni
labels:
  - configuration
assignees: []
created_at: 2024-04-15T19:24:32Z
updated_at: 2024-04-18T01:42:52Z
url: https://github.com/astral-sh/ruff/issues/10960
synced_at: 2026-01-07T13:12:15-06:00
---

# extend-immutable-calls doesn't work on Python scripts (as opposed to modules)

---

_Issue opened by @fofoni on 2024-04-15 19:24_

Keywords: extend-immutable-calls, B008, script, module, `__main__`
Ruff version: 0.3.7

### Working MWE

In a directory containing just the file `test.py`:
```python
from other_module import A

def f(a=A()):
    pass
```
checking with:
```bash
ruff check --isolated --select=B008 \
    --config 'lint.flake8-bugbear.extend-immutable-calls=["other_module.A"]' \
    test.py
```
yields no errors.

### Buggy MWE

If I declare `A` in the script instead of importing it:
```python
class A: pass

def f(a=A()):
    pass
```
now ruff will find a B008 error in the `def` line, no matter what I configure in `extend-immutable-calls`. I tried `A`, `test.A`, `__main__.A`, and even `builtins.A` and `__builtins__.A`. Nothing makes it recognize `A()` as an immutable call.


---

_Comment by @charliermarsh on 2024-04-16 01:24_

Hmm yeah... We need some way to identify `A`, and if you pass a script, we probably can't find a module root against which to resolve the path.

---

_Comment by @charliermarsh on 2024-04-16 01:24_

`__main__.A` is kind of interesting.

---

_Label `configuration` added by @charliermarsh on 2024-04-16 01:24_

---

_Comment by @fofoni on 2024-04-16 02:01_

(Updated the OP to note that I also tried with `test.A`.)

> `__main__.A` is kind of interesting.

I tried `__main__.A` because I didn't know what would work and was exploring possibilities, but I'm not sure if like it: after all, depending on how you invoke Python, *any* .py file could be the `__main__` module. In the end, fully-qualified names aren't really well-defined statically -- you need to look them up in `sys.path` and follow import hooks etc (mypy can sometimes [run into a similar problem](https://github.com/pypa/setuptools/issues/3518)).

Since ruff interprets module paths statically, I think the best thing to do in this case would be to check that `test.py` is not contained in an import package (i.e. not alongside an `__init.__.py`) and then accept `test.A` as `A`'s fully-qualified name.

---

_Comment by @charliermarsh on 2024-04-16 02:32_

That seems reasonable to me.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 02:49_

---

_Referenced in [astral-sh/ruff#10965](../../astral-sh/ruff/pulls/10965.md) on 2024-04-16 02:58_

---

_Closed by @charliermarsh on 2024-04-18 01:42_

---
