```yaml
number: 6560
title: "Formatter: Drops `/` separator in lambdas if it is the last parameter"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-14T13:31:07Z
updated_at: 2023-08-16T07:11:36Z
url: https://github.com/astral-sh/ruff/issues/6560
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: Drops `/` separator in lambdas if it is the last parameter

---

_@MichaReiser_


```python
lambda a, /: a

# Gets formatted as 
lambda: a

# Instead of 
lambda a, /: a
```

[Playground](https://play.ruff.rs/1c91e1fb-8d32-4f90-bc5b-52cba871b1e2)

---

_Label `bug` added by @MichaReiser on 2023-08-14 13:31_

---

_Label `formatter` added by @MichaReiser on 2023-08-14 13:31_

---

_Label `help wanted` added by @MichaReiser on 2023-08-14 13:31_

---

_Closed by @MichaReiser on 2023-08-14 15:38_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
