```yaml
number: 14290
title: "(🎁) new rule: in type annotations, prefer `None` appears at the end of unions"
type: issue
state: closed
author: KotlinIsland
labels:
  - rule
  - typing
assignees: []
created_at: 2024-11-12T01:28:57Z
updated_at: 2024-11-14T18:37:14Z
url: https://github.com/astral-sh/ruff/issues/14290
synced_at: 2026-01-10T11:09:56Z
```

# (🎁) new rule: in type annotations, prefer `None` appears at the end of unions

---

_Issue opened by @KotlinIsland on 2024-11-12 01:28_

```py
a: None | int
```
i would prefer this to be written `a: int | None`

---

_Label `rule` added by @MichaReiser on 2024-11-12 08:08_

---

_Label `typing` added by @MichaReiser on 2024-11-12 08:08_

---

_Closed by @MichaReiser on 2024-11-14 18:37_

---
