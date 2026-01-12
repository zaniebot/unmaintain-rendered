```yaml
number: 14014
title: "[red-knot] Add understanding of terminal statements to control-flow analysis"
type: issue
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
created_at: 2024-10-31T12:04:57Z
updated_at: 2025-02-05T22:49:28Z
url: https://github.com/astral-sh/ruff/issues/14014
synced_at: 2026-01-12T15:54:53Z
```

# [red-knot] Add understanding of terminal statements to control-flow analysis

---

_@AlexWaygood_

red-knot currently issues false positives on the following snippets:

```py
def str_instance() -> str:
    return "foo"

some_string = str_instance()


try:
    x = some_string[5]
except IndexError:
    raise ValueError("no")
print(x)  # false positive 'possibly not defined' error here


def f():
    try:
        y = some_string[5]
    except IndexError:
        return
    print(y)  # false positive 'possibly not defined' error here


while True:
    try:
        z = some_string[5]
    except IndexError:
        continue
    print(z)  # false positive 'possibly not defined' error here

    try:
        aa = some_string[5]
    except IndexError:
        break
    print(aa)  # false positive 'possibly not defined' error here
```

In all of these situations, we see that a variable is declared in the `try` branch but not the `except` branch of the `try`/`except` block. We realise that either branch could be taken, so we union together the boundedness of the two branches to conclude that the variable is possibly unbound at the point where it is read later in the same scope.

What we're missing is special understanding of the `return`, `raise`, `break` and `continue` statements. When we union together the boundedness of the branches, we need to exclude all branches that always end in one of these statements, because we can determine that later statements in the same scope will be unreachable by branches that always end in one of those statements.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-31 12:05_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-10-31 12:05_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:24_

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:43_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:43_

---

_Unassigned @AlexWaygood by @carljm on 2025-01-09 17:46_

---

_Assigned to @dcreager by @carljm on 2025-01-09 22:12_

---

_Comment by @dcreager on 2025-02-05 22:49_

This is done with #15676 and #15817.  We might change how we handle terminal statements as part of figuring out what we want to do with unreachable code (https://github.com/astral-sh/ruff/issues/15797).

---

_Closed by @dcreager on 2025-02-05 22:49_

---
