---
number: 4470
title: Make A001 warns when method shadows a builtin (or create a new rule for this)?
type: issue
state: closed
author: duongdominhchau
labels:
  - rule
assignees: []
created_at: 2023-05-17T13:37:45Z
updated_at: 2023-06-13T06:43:07Z
url: https://github.com/astral-sh/ruff/issues/4470
synced_at: 2026-01-07T13:12:14-06:00
---

# Make A001 warns when method shadows a builtin (or create a new rule for this)?

---

_Issue opened by @duongdominhchau on 2023-05-17 13:37_

Hi, first of all, thank you for building this tool, saved me time integrating tens of similar tools into the project.

Now, here's the problem I noticed recently: for variables we have `A001` to warn when it shadows a builtin. However, builtins can also be shadowed by methods, but `A001` does not catch that. Here's a situation where it can cause trouble:

```python
class C:
    @staticmethod
    def list() -> None:
        pass

    @staticmethod
    def repeat(value: int, times: int) -> list[int]:
        return [value] * times
```

If you pass this to `pyright` it will warn at the return type of `repeat`, like this:

```plain
a.py:7:43 - error: Expected type expression but received "() -> None" (reportGeneralTypeIssues)
```

From the error message, looks like `list` in the second method signature is understood as the `list` method we defined above, not the `list` builtin. This is highly confusing, but I'm not sure if this really belongs to `A001` though as this problem only happen at type level, you can't refer to that method inside another method.

Anyway, it costs me almost an hour to realize what's wrong (yep, I'm that noob), so I think I should just write this down in case another poor soul is also facing the same problem 🤷 

---

_Comment by @charliermarsh on 2023-05-17 16:46_

I think we may have done this originally, but it ended up being more confusing than helpful. I think this _could_ be useful though. We'd probably want to flag at the usage site (like `list[int]`), not the definition site (`def list`), since it's totally valid for a class to define a function named `list`. The confusion comes when you reference that function within the class scope.


---

_Label `rule` added by @charliermarsh on 2023-05-17 16:46_

---

_Comment by @charliermarsh on 2023-06-12 23:29_

Ah sorry, I believe this is A003?

```
foo.py:3:9: A003 Class attribute `list` is shadowing a Python builtin
```

---

_Closed by @charliermarsh on 2023-06-12 23:29_

---

_Comment by @duongdominhchau on 2023-06-13 06:43_

Ouch, sorry for wasting your time. I looked at the end of my config and see the `ignorelist` having only `id` there so I thought `A003` is being partially enabled, turned out there is another line completely disabled that rule. I tried with `A003` enabled and it does warn at the definition of `list()`, thank you.

---

_Referenced in [astral-sh/ruff#6524](../../astral-sh/ruff/issues/6524.md) on 2023-09-05 11:28_

---
