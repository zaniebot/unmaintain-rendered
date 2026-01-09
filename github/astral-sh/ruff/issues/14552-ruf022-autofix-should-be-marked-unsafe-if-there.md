---
number: 14552
title: "RUF022 autofix should be marked unsafe if there are own-line comments between `__all__` items"
type: issue
state: closed
author: phil65
labels:
  - fixes
assignees: []
created_at: 2024-11-23T04:12:59Z
updated_at: 2024-11-24T12:49:30Z
url: https://github.com/astral-sh/ruff/issues/14552
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF022 autofix should be marked unsafe if there are own-line comments between `__all__` items

---

_Issue opened by @phil65 on 2024-11-23 04:12_

- I searched issues for `__all__`, couldnt see anything from today.

Since 0.8.0, ruff wants to sort my `__all__` array.
Thats generally very nice, but it also messes up something like.

``` python
__all__ = [
    # Core components
    "TaskManager",
    "TaskExecutor",
    # Models
    "TaskContext",
    "TaskProvider",
    "TaskResult",
    # Utilities
    "execute_concurrent",
]
```

gets sorted to:

``` python
__all__ = [
    # Models
    "TaskContext",
    "TaskExecutor",
    # Core components
    "TaskManager",
    "TaskProvider",
    "TaskResult",
    # Utilities
    "execute_concurrent",
]
```

So it messes up the groups.
Comments are only attached to one item instead of the group when sorting. I am not sure if that is a good assumption. Perhaps ignore sorting it when comments are inside the __all__ array?

Thanks! :)



---

_Comment by @MichaReiser on 2024-11-23 08:49_

Thanks for opening this issue. 

Comments are difficult to get right because they have no semantic meaning. Is `# Core components` commenting `TaskManager` or the entire group? I also know that @AlexWaygood spent a lot of time adding good comment support. So I'm interested to hear his thoughts too.


I see two possible changes instead of removing the fix entirely:

1. Mark the fix as unsafe if there are own-line comments because the fix might change the comment's semantic
2. Improve the heuristic. For example, the rule could implement sort-by-group similar to isort if some items are separated by blank lines

```py
__all__ = [
    # Core components
    "TaskManager",
    "TaskExecutor",

    # Models
    "TaskContext",
    "TaskProvider",
    "TaskResult",
    
    # Utilities
    "execute_concurrent",
]
```

Is formatted to


```py
__all__ = [
    # Core components
    "TaskExecutor",
    "TaskManager",

    # Models
    "TaskContext",
    "TaskProvider",
    "TaskResult",
    
    # Utilities
    "execute_concurrent",
]
```

I'm sure @AlexWaygood has some thought

---

_Closed by @MichaReiser on 2024-11-23 08:49_

---

_Reopened by @MichaReiser on 2024-11-23 08:49_

---

_Label `rule` added by @MichaReiser on 2024-11-23 08:50_

---

_Label `rule` removed by @MichaReiser on 2024-11-23 08:50_

---

_Label `fixes` added by @MichaReiser on 2024-11-23 08:50_

---

_Comment by @AlexWaygood on 2024-11-23 09:25_

Thanks for opening the issue @phil65!

There's already some extensive documentation on the fix safety of this rule at https://docs.astral.sh/ruff/rules/unsorted-dunder-all/#fix-safety, and we note this problem there. However, I'm open to making this fix unsafe if there are any own-line comments. I think that's in the spirit of the policy on fix safety we recently adopted when it comes to comments. We should make the same change to `sort-dunder-slots` as well.

> 2\. Improve the heuristic. For example, the rule could implement sort-by-group similar to isort if some items are separated by blank lines

Unfortunately doing this would make the rule incompatible with the formatter, because the formatter will remove blank lines in between the items, which would then lead to the rule applying a different sort to the items than if the blank lines were present.

---

_Comment by @MichaReiser on 2024-11-23 09:40_

> Unfortunately doing this would make the rule incompatible with the formatter, because the formatter will remove blank lines in between the items, which would then lead to the rule applying a different sort to the items than if the blank lines were present.

Right, I think we even discussed this back then. Thanks for reminding me.

---

_Renamed from "Over-eager __all__ sorting" to "RUF022 autoroute shouldOver-eager __all__ sorting" by @AlexWaygood on 2024-11-23 16:11_

---

_Renamed from "RUF022 autoroute shouldOver-eager __all__ sorting" to "RUF022 autofix should be marked unsafe if there are own-line comments" by @AlexWaygood on 2024-11-23 16:12_

---

_Renamed from "RUF022 autofix should be marked unsafe if there are own-line comments" to "RUF022 autofix should be marked unsafe if there are own-line comments between `__all__` items" by @AlexWaygood on 2024-11-23 16:12_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-23 22:49_

---

_Referenced in [astral-sh/ruff#14560](../../astral-sh/ruff/pulls/14560.md) on 2024-11-23 23:18_

---

_Comment by @phil65 on 2024-11-23 23:20_

Thank you for quick handling and a great piece of software. :)

---

_Closed by @AlexWaygood on 2024-11-24 12:49_

---
