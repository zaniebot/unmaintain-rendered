---
number: 15315
title: Please allow to address rules by names instead of codes, too
type: issue
state: closed
author: fraunhofersociety
labels: []
assignees: []
created_at: 2025-01-07T10:27:54Z
updated_at: 2025-01-07T12:05:14Z
url: https://github.com/astral-sh/ruff/issues/15315
synced_at: 2026-01-07T13:12:16-06:00
---

# Please allow to address rules by names instead of codes, too

---

_Issue opened by @fraunhofersociety on 2025-01-07 10:27_

Hello,
could you please support addressing warnings by rule names instead of rule codes?
That would make my code more readable for humans. 

**A.** For example I would like to use

`# noqa: some-rule-name`

instead of

`# noqa: XY123`


**B.** Similarly, in the configuration file, I would prefer names over codes, too:

```
[tool.ruff]
target-version = "py312"
select = ["ALL"]
ignore = [
    "some-rule-name",  
]
```

**C.** Please also include rule names in the output of ruff.


Related:

https://github.com/astral-sh/ruff/discussions/3442

https://docs.astral.sh/ruff/rules

---

_Comment by @MichaReiser on 2025-01-07 11:43_

Thanks. This is something we will consider supporting in the future, but it's not yet decided if as part of `now` or as a ruff-specific suppression comment, e.g., `ruff: ignore[name].` 

We plan on tackling this as part of https://github.com/astral-sh/ruff/issues/1773

---

_Closed by @MichaReiser on 2025-01-07 11:43_

---
