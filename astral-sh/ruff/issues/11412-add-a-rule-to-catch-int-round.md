```yaml
number: 11412
title: Add a rule to catch int(round(...)
type: issue
state: closed
author: NeilGirdhar
labels:
  - rule
assignees: []
created_at: 2024-05-13T16:49:26Z
updated_at: 2024-12-05T09:30:08Z
url: https://github.com/astral-sh/ruff/issues/11412
synced_at: 2026-01-12T15:54:51Z
```

# Add a rule to catch int(round(...)

---

_@NeilGirdhar_

`int(round(x))`and  `int(round(x, 0))` and `int(round(x, None))` can all simply be written `round(x)`.  However, unnecessary casts to `int` are [prevalent](https://sourcegraph.com/search?q=context%3Aglobal+int%28round%28&patternType=keyword&sm=0&filters=%5B%5B%22lang%22%2C%22Python%22%2C%22lang%3Apython%22%5D%5D).  Please consider adding a rule to catch these?

---

_Label `rule` added by @zanieb on 2024-05-13 17:32_

---

_Comment by @zanieb on 2024-05-13 17:33_

Seems reasonable to me. `unnecessary-round-cast` or something?

---

_Comment by @Skylion007 on 2024-05-17 15:30_

This sounds like a good refurb rule. @dosisod 

---

_Closed by @MichaReiser on 2024-12-05 09:30_

---
