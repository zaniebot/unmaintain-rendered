---
number: 10107
title: "new rule - variable is not used before it's assigned again"
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2024-02-24T02:43:47Z
updated_at: 2025-01-23T02:26:23Z
url: https://github.com/astral-sh/ruff/issues/10107
synced_at: 2026-01-07T13:12:15-06:00
---

# new rule - variable is not used before it's assigned again

---

_Issue opened by @DetachHead on 2024-02-24 02:43_

```py
a = 1 # error: asignment is useless because it's not used before being assigned again
a = 2
print(a)
```

---

_Label `rule` added by @MichaReiser on 2024-02-24 13:30_

---

_Comment by @MichaReiser on 2024-02-24 13:33_

I like that! This rule is similar to dead code analysis (and should maybe be integrated into a dead code) and would profit from a data flow analysis framework so that it also catches

```python
a = 1
if x:
	a = 2
else: 
	a = 3
```

---

_Comment by @mikeleppane on 2024-03-01 10:09_

This rule would be helpful and considered part of the dead code analysis. I might start looking into this soonish. 

---

_Comment by @InSyncWithFoo on 2025-01-23 02:26_

This sounds like something [`F811`](https://docs.astral.sh/ruff/rules/redefined-while-unused/) should catch, but it currently doesn't.

---
