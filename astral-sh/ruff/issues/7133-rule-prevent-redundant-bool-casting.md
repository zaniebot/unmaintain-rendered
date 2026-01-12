```yaml
number: 7133
title: "rule: prevent redundant bool casting"
type: issue
state: open
author: dorschw
labels:
  - rule
assignees: []
created_at: 2023-09-04T14:22:26Z
updated_at: 2023-09-05T08:25:33Z
url: https://github.com/astral-sh/ruff/issues/7133
synced_at: 2026-01-12T15:54:46Z
```

# rule: prevent redundant bool casting

---

_@dorschw_

```diff
- if bool(condition):
+ if condition:
```

I'd suggest it to [flake8-simplify](https://github.com/MartinThoma/flake8-simplify/), but their repo seems a bit deserted since late march :( 

---

_Comment by @MichaReiser on 2023-09-05 06:55_

Thanks for proposing a new rule. To clarify, you propose to warn about all `bool` casts in clause headers (that, by definition), evaluate to a thruthy value? 

---

_Label `rule` added by @MichaReiser on 2023-09-05 06:56_

---

_Comment by @dorschw on 2023-09-05 08:25_

Exactly

---
