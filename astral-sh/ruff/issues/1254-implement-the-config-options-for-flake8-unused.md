```yaml
number: 1254
title: Implement the config options for flake8-unused-arguments
type: issue
state: closed
author: lovetox
labels:
  - configuration
assignees: []
created_at: 2022-12-15T22:51:31Z
updated_at: 2022-12-25T22:22:51Z
url: https://github.com/astral-sh/ruff/issues/1254
synced_at: 2026-01-12T15:54:41Z
```

# Implement the config options for flake8-unused-arguments

---

_@lovetox_

Hi, this is very useful check but we cant use it because it detects **kwargs / *args as unused.
I believe its not necessary to mark this unsued vars, as bei convention **kwargs / *args is in most cases expected to not be used as it acts as a catch all.
I have to say i never seen a codebase that prefixes that with underscore.

i was not able to use any of the config options

i tried

```
[tool.ruff.flake8-unused-arguments]
unused-arguments-ignore-variadic-names = "true"
```

but no luck

---

_Comment by @charliermarsh on 2022-12-15 22:57_

Yeah I can add that option. Are there others that are important to you right now?


---

_Comment by @lovetox on 2022-12-15 23:16_

I would say `unused-arguments-ignore-variadic-names` is the most important

Did you implement everything from that package?

Because i see no errors on `overload-functions`, or `empty stub` functions currently, which i find ok, because it makes no sense to check those, i mean both have no implementation obviously no arg will be used.

Im not sure if you implement the checks on abstract methods and dunder, but if,i probably would like to disable them.

These 2 are probably less important:

unused-arguments-ignore-lambdas
unused-arguments-ignore-nested-functions

---

_Label `configuration` added by @charliermarsh on 2022-12-15 23:22_

---

_Comment by @charliermarsh on 2022-12-15 23:23_

Yes, we ignore overloaded and stub functions, and I think we ignore abstract methods when we can detect them (not always possible because we aren't doing deep static analysis, e.g., when extending a parent class). I'd have to review the code though.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-16 04:20_

---

_Closed by @charliermarsh on 2022-12-16 05:22_

---

_Comment by @g-as on 2022-12-25 22:06_

@charliermarsh 

Are you sure that overloaded functions are ignored?

With
```python
import typing


@typing.overload
def concat(a: str, b: str) -> str:
    ...

@typing.overload
def concat(a: int, b: int) -> str:
    ...


def concat(a, b):
    return f"{a}{b}"
```
on latest *ruff* i'm getting
```
site_register/toto.py:5:12: ARG001 Unused function argument: `a`
site_register/toto.py:5:20: ARG001 Unused function argument: `b`
site_register/toto.py:9:12: ARG001 Unused function argument: `a`
site_register/toto.py:9:20: ARG001 Unused function argument: `b`
```

---

_Comment by @charliermarsh on 2022-12-25 22:15_

Lemme check.

---

_Comment by @charliermarsh on 2022-12-25 22:17_

I think I was mixing up unused arguments with missing type annotations.

---

_Comment by @charliermarsh on 2022-12-25 22:18_

Tracking here: #1372.

---

_Comment by @charliermarsh on 2022-12-25 22:22_

@g-as - Fixed in https://github.com/charliermarsh/ruff/pull/1373. Will cut a new release tonight, want to fix a few more things.

---
