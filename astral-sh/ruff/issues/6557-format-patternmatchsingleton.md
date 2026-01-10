```yaml
number: 6557
title: "Format `PatternMatchSingleton`"
type: issue
state: closed
author: dhruvmanila
labels:
  - good first issue
  - formatter
assignees: []
created_at: 2023-08-14T12:40:26Z
updated_at: 2023-08-22T06:23:48Z
url: https://github.com/astral-sh/ruff/issues/6557
synced_at: 2026-01-10T11:09:48Z
```

# Format `PatternMatchSingleton`

---

_Issue opened by @dhruvmanila on 2023-08-14 12:40_

This would be done in a similar manner as mentioned here (https://github.com/astral-sh/ruff/issues/6555#issuecomment-1677241864) except that this would be delegated to [`ExprConstant`](https://github.com/astral-sh/ruff/blob/b05574babd6224af99938a429d0b19ba3297513c/crates/ruff_python_formatter/src/expression/expr_constant.rs) formatting instead.

Implement formatting for singleton formatting

```python
match x:
    case None:
        ...
```

---

_Label `good first issue` added by @dhruvmanila on 2023-08-14 12:41_

---

_Label `formatter` added by @dhruvmanila on 2023-08-14 12:41_

---

_Added to milestone `Formatter: Alpha` by @dhruvmanila on 2023-08-14 13:41_

---

_Comment by @LaBatata101 on 2023-08-14 22:24_

I would like to work on this

---

_Assigned to @LaBatata101 by @dhruvmanila on 2023-08-15 01:47_

---

_Closed by @MichaReiser on 2023-08-22 06:23_

---
