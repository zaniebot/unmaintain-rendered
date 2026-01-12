```yaml
number: 1073
title: "Check for `if` conditions that are always truthy"
type: issue
state: open
author: kkpattern
labels:
  - lint
assignees: []
created_at: 2025-08-21T03:20:26Z
updated_at: 2025-10-06T16:51:48Z
url: https://github.com/astral-sh/ty/issues/1073
synced_at: 2026-01-12T15:54:24Z
```

# Check for `if` conditions that are always truthy

---

_@kkpattern_

I wish ty can check for comparing coroutine in if statement. Given the following code:

```python
async def check_foo() -> bool:
    return True

async def main():
    if check_foo():
        print("hello")
```

we almost never want to compare a coroutine in if statement because it will always be true. What we want is actually:

```python
async def main():
    if await check_foo():
        print("hello")
```

I wonder if this should be a ty rule or ruff rule? I don't know if ruff knows the `check_foo` is a coroutine during the check. If it should be a ruff rule I will move this issue to the ruff repo.

Thanks.

---

_Label `lint` added by @carljm on 2025-08-21 04:26_

---

_Comment by @carljm on 2025-08-21 04:28_

Ruff could check this, but only in a very incomplete way; if `check_foo()` came from a different module, it would not know that it is an async function.

Long-term our plan is to build type-powered linting on top of ty, but at the moment we are still focused on getting the type-checker feature set complete.

I do think this is an ideal example of a type-powered lint rule, and we should definitely build it, when we are at that point of adding such rules.

---

_Comment by @kkpattern on 2025-08-21 04:40_

Awesome!

---

_Renamed from "Check for comparing coroutine in if statement" to "Check for `if` conditions that are always truthy" by @AlexWaygood on 2025-10-06 16:51_

---
