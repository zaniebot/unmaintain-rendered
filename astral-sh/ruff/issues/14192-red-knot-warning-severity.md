```yaml
number: 14192
title: "[red-knot] Warning severity"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-11-08T08:02:14Z
updated_at: 2024-12-12T12:39:16Z
url: https://github.com/astral-sh/ruff/issues/14192
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Warning severity

---

_Issue opened by @MichaReiser on 2024-11-08 08:02_

Support `warning` severity for rules


**Implementation Note**

We can use two `BitSet`s to represent the severity of a rule:

* One bitset tracks if a rule is enabled or disabled. 
* The second bitset tracks if a rule has the severity warning or error

Depends on https://github.com/astral-sh/ruff/issues/14191

---

_Label `red-knot` added by @MichaReiser on 2024-11-08 08:02_

---

_Added to milestone `Red Knot 2024` by @MichaReiser on 2024-11-08 08:02_

---

_Closed by @MichaReiser on 2024-12-12 12:39_

---
