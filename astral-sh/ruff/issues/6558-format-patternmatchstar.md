```yaml
number: 6558
title: "Format `PatternMatchStar`"
type: issue
state: closed
author: dhruvmanila
labels:
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-14T12:44:51Z
updated_at: 2023-08-24T01:58:07Z
url: https://github.com/astral-sh/ruff/issues/6558
synced_at: 2026-01-12T15:54:46Z
```

# Format `PatternMatchStar`

---

_@dhruvmanila_

This would be done in a similar manner as mentioned here (https://github.com/astral-sh/ruff/issues/6555#issuecomment-1677241864) except that this would be delegated to [`Identifier`](https://github.com/astral-sh/ruff/blob/b05574babd6224af99938a429d0b19ba3297513c/crates/ruff_python_formatter/src/other/identifier.rs) formatting instead.

Implement formatting for the `*pat` pattern

```python
match x:
    case [1, 2, *rest]:
        ...
    case [*_]:
        ...
```

---

_Label `good first issue` added by @dhruvmanila on 2023-08-14 12:45_

---

_Label `formatter` added by @dhruvmanila on 2023-08-14 12:45_

---

_Added to milestone `Formatter: Alpha` by @dhruvmanila on 2023-08-14 13:41_

---

_Label `help wanted` added by @MichaReiser on 2023-08-17 09:58_

---

_Comment by @harupy on 2023-08-17 12:05_

Can I work on this?

---

_Assigned to @harupy by @MichaReiser on 2023-08-17 12:12_

---

_Comment by @MichaReiser on 2023-08-17 12:12_

Sure!

---

_Closed by @charliermarsh on 2023-08-24 01:58_

---
