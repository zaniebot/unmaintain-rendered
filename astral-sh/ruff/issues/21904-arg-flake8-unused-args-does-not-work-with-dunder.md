---
number: 21904
title: ARG flake8-unused-args does not work with dunder methods
type: issue
state: open
author: alexruddick-grantek
labels:
  - question
assignees: []
created_at: 2025-12-10T19:59:11Z
updated_at: 2025-12-11T08:41:51Z
url: https://github.com/astral-sh/ruff/issues/21904
synced_at: 2026-01-10T01:23:02Z
---

# ARG flake8-unused-args does not work with dunder methods

---

_Issue opened by @alexruddick-grantek on 2025-12-10 19:59_

### Summary

My initial concern was finding (bad) code that tried to pass `*args` to `__aenter()__`.  
The `*args` has no effect, and should be caught by a linter IMO. 
https://docs.python.org/3/reference/datamodel.html#object.__aenter

The maintainers at `flake8-asyncio` [pointed out](https://github.com/python-trio/flake8-async/issues/421#event-21464789729) that this is not only just `__aenter__` but also affects a number of other dunder methods.

I also realized there is flake8 plugin that catches this: `flake8-unused-arguments`.  `ruff` implements these rules as `ARG`.  After doing some testing, I realized that `ruff` doesn't flag unused arguments in dunder methods _at all_.  Not `some_arg`, not `*args`, and not `**kwargs`.

```python
class Class:
    def foo(self, arg1, arg2):
        print(arg1)  # ARG002 Unused method argument: `arg2`

    def __aenter__(self, unused):  # ruff does not error, flake8 does
        return self

class Foo:
    async def __aenter__(self, *args):   # ruff does not error, flake8 does
        return self

class Bar:
    async def __aenter__(self, **kwargs):   # ruff does not error, flake8 does
        return self

class Baz:
    async def __aenter__(self):  # OK
        return self
```
See reproduction repo here: https://github.com/alexrudd2/unused-args (ruff + flake8) or [playground](https://play.ruff.rs/7fa84f6b-b5cf-43fb-8fde-2a3c3dfd48c0).


### Version

0.14.8

---

_Comment by @alexrudd2 on 2025-12-10 20:00_

(posting here to subscribe to notifications on my intended account)

---

_Comment by @amyreese on 2025-12-10 20:11_

I assume the ARG rule in ruff is just trying to avoid needing a catalog of 100+ dunder methods to know which ones have arguments that need to be used or not, as it is common to not use arguments in methods like `__exit__` or `__aexit__` for decorators that don't care about exception state, etc.

---

_Label `question` added by @amyreese on 2025-12-10 20:11_

---

_Comment by @ntBre on 2025-12-10 20:18_

Related: https://github.com/astral-sh/ruff/issues/1796, the corresponding PR is where the exception for dunder methods was added.

---
