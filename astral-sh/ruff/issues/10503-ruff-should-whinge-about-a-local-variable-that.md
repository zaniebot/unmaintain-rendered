```yaml
number: 10503
title: (🎁) Ruff should whinge about a local variable that prefixed with an underscore, and is also used
type: issue
state: open
author: KotlinIsland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-03-21T04:31:09Z
updated_at: 2024-03-21T08:18:31Z
url: https://github.com/astral-sh/ruff/issues/10503
synced_at: 2026-01-12T15:54:50Z
```

# (🎁) Ruff should whinge about a local variable that prefixed with an underscore, and is also used

---

_@KotlinIsland_

"underscore", "variable", "used"

```py
def f():
    _a = 1
    print(_a)  # no error (about _a)
```
`_a` is used, it shouldn't have an underscore here.

- related https://github.com/astral-sh/ruff/issues/7539#issuecomment-1782462333

---

_Label `rule` added by @dhruvmanila on 2024-03-21 07:47_

---

_Label `needs-decision` added by @dhruvmanila on 2024-03-21 07:47_

---

_Comment by @MichaReiser on 2024-03-21 08:18_

I like the idea. One challenge that #7539 has pointed out that underscore prefixes are also used to mark private variables on the module level. The rule would either exclude module-level variables or use another heuristic to disambiguate.

---
