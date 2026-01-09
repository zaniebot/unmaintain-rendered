---
number: 19014
title: RUF009 should allow attr.field(...) call
type: issue
state: closed
author: ivanychev
labels:
  - bug
  - rule
assignees: []
created_at: 2025-06-28T18:14:20Z
updated_at: 2025-07-03T14:29:56Z
url: https://github.com/astral-sh/ruff/issues/19014
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF009 should allow attr.field(...) call

---

_Issue opened by @ivanychev on 2025-06-28 18:14_

### Summary

I observed this when upgraded to 0.12.1, Python 3.12. I'm using `attrs==22.2.0`. The code:

```python
import attr


@attr.define
class SomeClass:
    a: list = attr.field(factory=list)
```

Latest ruff says `RUF009 Do not perform function call `attr.field` in dataclass defaults` at `attr.field(factory=list)`

I believe it's not expected behavior, is it?

### Version

0.12.1

---

_Comment by @ntBre on 2025-06-28 18:27_

Thanks for the report. I think this is a regression related to https://github.com/astral-sh/ruff/pull/18735. I don't get any errors on 0.12.0.

---

_Label `bug` added by @ntBre on 2025-06-28 18:27_

---

_Label `rule` added by @ntBre on 2025-06-28 18:27_

---

_Referenced in [astral-sh/ruff#19021](../../astral-sh/ruff/pulls/19021.md) on 2025-06-28 23:16_

---

_Closed by @ntBre on 2025-07-03 14:29_

---
