```yaml
number: 20341
title: "`SIM910` doesn't recognize `get` access on dictionary when the key is a property/method call"
type: issue
state: closed
author: marianosimone
labels:
  - rule
assignees: []
created_at: 2025-09-10T22:31:52Z
updated_at: 2025-09-12T14:17:55Z
url: https://github.com/astral-sh/ruff/issues/20341
synced_at: 2026-01-10T11:09:59Z
```

# `SIM910` doesn't recognize `get` access on dictionary when the key is a property/method call

---

_Issue opened by @marianosimone on 2025-09-10 22:31_

### Summary

For this case:

```python
ages = {"Tom": 23, "Maria": 23, "Dog": 11}
key_source = {"Thomas": "Tom"}
age = ages.get(key_source.get("Thomas", "Tom"), None)
```

I'd expect SIM910 to detect that the `None` is unnecessary

### Version

ruff 0.13.0

---

_Renamed from "SIM910 doesn't recognize `get` access on dictionary when the key is a property/method call" to "`SIM910` doesn't recognize `get` access on dictionary when the key is a property/method call" by @marianosimone on 2025-09-10 22:32_

---

_Comment by @ntBre on 2025-09-10 22:57_

Interesting, it looks like we bail out if the key argument isn't a literal or  variable name:

https://github.com/astral-sh/ruff/blob/34ba738c19c557653567defc5cdaaa75445ad0d4/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs#L267-L269

We could possibly just remove that restriction, I'm not seeing anywhere it would cause a problem.

---

_Label `rule` added by @ntBre on 2025-09-10 22:57_

---

_Closed by @ntBre on 2025-09-12 14:17_

---
