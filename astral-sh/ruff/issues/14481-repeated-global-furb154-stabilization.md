---
number: 14481
title: "repeated-global: `FURB154` stabilization"
type: issue
state: open
author: MichaReiser
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-20T08:38:52Z
updated_at: 2024-11-20T08:38:52Z
url: https://github.com/astral-sh/ruff/issues/14481
synced_at: 2026-01-10T01:22:55Z
---

# repeated-global: `FURB154` stabilization

---

_Issue opened by @MichaReiser on 2024-11-20 08:38_

* Name: The name suggests to me that it's about repeated variables in a global statement, but that's not what the rule catches. The rule catches consecutive `global` or `nonlocal` statements. I suggest renaming the rule to `consecutive-global`
* Consecutive global statements can be desired to avoid overlong lines because parenthesizing the names and breaking them over multiple lines is not valid syntax :( 

---

_Label `rule` added by @MichaReiser on 2024-11-20 08:38_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-20 08:38_

---
