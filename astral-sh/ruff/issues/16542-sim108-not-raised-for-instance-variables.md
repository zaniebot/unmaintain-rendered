---
number: 16542
title: SIM108 not raised for instance variables
type: issue
state: closed
author: cspoelstra
labels:
  - rule
assignees: []
created_at: 2025-03-06T19:46:59Z
updated_at: 2025-03-13T00:51:30Z
url: https://github.com/astral-sh/ruff/issues/16542
synced_at: 2026-01-10T01:22:57Z
---

# SIM108 not raised for instance variables

---

_Issue opened by @cspoelstra on 2025-03-06 19:46_

### Summary

SIM108 is not being raised for instance variables.

```
class A:
    def func(self):
        if cond:
            self.x = a
        else:
            self.x = b
```

Expected behavior:
SIM108 raised to suggest
```
self.x = a if cond else b
```
Actual behavior:
SIM108 not raised.

Removing `self.` causes the rule to be raised.

---

_Label `rule` added by @ntBre on 2025-03-06 20:43_

---

_Label `help wanted` added by @ntBre on 2025-03-06 20:44_

---

_Comment by @ntBre on 2025-03-06 20:47_

Thanks for the report! It looks like the rule is currently restricted to simple variable name expressions like the [upstream rule](https://github.com/MartinThoma/flake8-simplify/blob/4f1b89f74feb8032f82c1e1eb1f2f77dd7005d96/flake8_simplify/rules/ast_if.py#L138-L139), but I think it makes sense to expand it to attribute accesses (as in this example) and probably subscripts too. I think that should cover valid left-hand sides of assignments.

https://github.com/astral-sh/ruff/blob/b3c884f4f33a81cd08e9ebc13fd0d5a538c8afc7/crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs#L128-L133

---

_Label `help wanted` removed by @MichaReiser on 2025-03-07 09:28_

---

_Comment by @MichaReiser on 2025-03-07 09:31_

The challenge with allowing more expressions is that we then get pushback that the resulting expression is now too complicated and isn't a simplification. That's why, for now, I'd prefer to keep the rule aligned with the upstream rule. 

---

_Closed by @charliermarsh on 2025-03-13 00:51_

---
