```yaml
number: 20611
title: "B904: add exceptions[sic]"
type: issue
state: open
author: smurfix
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-28T19:04:26Z
updated_at: 2025-09-29T20:53:58Z
url: https://github.com/astral-sh/ruff/issues/20611
synced_at: 2026-01-10T11:09:59Z
```

# B904: add exceptions[sic]

---

_Issue opened by @smurfix on 2025-09-28 19:04_

### Summary

Raising `StopIteration`, `StopAsyncIteration`, `GeneratorExit`, … should not trigger B904. Nobody is going to look at this exception (at least in the normal case), and forcing me to add a `from None` to it is unnecessary verbosity.

```
        try:
            ... # something file or network related
        except EOFError:
            raise StopAsyncIteration
```

---

_Label `rule` added by @ntBre on 2025-09-29 20:53_

---

_Label `needs-decision` added by @ntBre on 2025-09-29 20:53_

---

_Comment by @ntBre on 2025-09-29 20:53_

This does make some sense to me. These are more like control flow than regular exceptions. I'll just put `needs-decision` on this to allow others to weigh in, but I would support making this change in preview.

---
