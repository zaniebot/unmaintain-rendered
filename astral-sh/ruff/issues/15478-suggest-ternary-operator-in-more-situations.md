```yaml
number: 15478
title: Suggest ternary operator in more situations
type: issue
state: open
author: nickdrozd
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-14T20:32:04Z
updated_at: 2025-01-15T15:06:09Z
url: https://github.com/astral-sh/ruff/issues/15478
synced_at: 2026-01-12T15:54:54Z
```

# Suggest ternary operator in more situations

---

_@nickdrozd_

Rule `SIM108` suggests using assignment expressions (ternary operator) instead of if/else blocks. Very good. But it is only raised for assignment. Assignment expressions are great in other situations too. For example, function calls and returns:

```python
def f(x, y):
    if x == 3:  # flagged as SIM108, good
        z = 4
    else:
        z = 2

    # not flagged, but could be rewritten:
    # print(y if x + y == 0 else x)
    if x + y == 0:
        print(y)
    else:
        print(x)

    # not flagged, but could be rewritten:
    # return z if x == y else 2 + z
    if x == y:
        return z
    else:
        return 2 + z
```

It would be great if `SIM108` could flag these other situations as well. Just as autofixable I think.

A somewhat trickier circumstance is when the branches have a function call of multiple arguments where only one argument depends on the branch:

```python
def g(x, y):
    if x == 3:
        h(x, y, 5)
    else:
        h(x, y, 6)

    # could be rewritten

    h(x, y, 5 if x == 3 else 6)
```

---

_Renamed from "Suggest assignment expressions in more situations" to "Suggest ternary operator in more situations" by @dylwil3 on 2025-01-15 00:31_

---

_Label `rule` added by @dylwil3 on 2025-01-15 00:31_

---

_Comment by @dylwil3 on 2025-01-15 00:34_

The first block of examples sounds reasonable and in-scope for this rule, especially the return statement. The second block seems more controversial just in terms of style, even ignoring potential difficulties in implementation.

---

_Label `needs-decision` added by @dylwil3 on 2025-01-15 00:35_

---

_Comment by @dhruvmanila on 2025-01-15 05:06_

Thank you for the suggestion. I agree with Dylan's analysis on the provided snippets.

I think this rule is specifically tailored to assignment statements (as you've highlighted) and not really any general form that can be converted into a conditional expression. This will also diverge the rule from the original plugin, it might be useful to open an upstream issue regarding similar improvements.

---

_Comment by @nickdrozd on 2025-01-15 15:06_

Basic motivation is that the "scope" of the branching should be as narrow as possible. In many cases branching only extends to selection of values, but `if / else` construction tends to pull more stuff into branch scope.

In the first code block, three things happen: a variable is assigned, something is printed, and a value is returned. Very simple. But because of branches, it looks more complicated. In the second block, just one thing happens: a function of three arguments is called. Again, simple. Only thing that changes is one of the function args.

Not everyone likes of course. Could be new check, put behind `--aggressive` flag, something like that. Personally, I just want to see all annoying stuff autofixed 😎 

---
