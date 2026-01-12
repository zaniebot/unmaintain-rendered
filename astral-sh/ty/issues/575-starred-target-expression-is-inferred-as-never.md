```yaml
number: 575
title: "Starred target expression is inferred as `Never`"
type: issue
state: open
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2025-06-03T13:45:34Z
updated_at: 2025-11-14T14:34:50Z
url: https://github.com/astral-sh/ty/issues/575
synced_at: 2026-01-12T15:54:23Z
```

# Starred target expression is inferred as `Never`

---

_@dhruvmanila_

Random thing I noticed while playing around with this branch locally just now: do you know why `Never` is given as the inlay type on the playground here? (This is a local deploy of the ty playground on your branch)

![image](https://github.com/user-attachments/assets/c9efc181-f47b-426f-ab2c-6c495803419a)

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/issues/18438#issuecomment-2934413527_
            

---

_Label `bug` added by @dhruvmanila on 2025-06-03 13:45_

---

_Comment by @dhruvmanila on 2025-06-03 13:47_

The revealed type here correctly reveals `list[Literal[2, 3]]` which is the actual type of `b`: https://play.ty.dev/73e18c9c-b67d-4d6f-a71c-2848e52a2564

---

_Comment by @dhruvmanila on 2025-06-05 03:42_

I think I know why this is happening -- in `infer_target_impl` there's no branch for `ExprStarred` which means it falls through to `infer_expression_type` which tries to infer the expression via `infer_name_expression` and as the context is `Store` it's inferred as `Never`.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:34_

---
