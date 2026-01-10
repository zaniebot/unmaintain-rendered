```yaml
number: 19565
title: "Force parentheses for `a or b <op> c`"
type: issue
state: open
author: jonashaag
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-25T21:01:48Z
updated_at: 2025-07-28T09:54:05Z
url: https://github.com/astral-sh/ruff/issues/19565
synced_at: 2026-01-10T11:09:59Z
```

# Force parentheses for `a or b <op> c`

---

_Issue opened by @jonashaag on 2025-07-25 21:01_

### Summary

Example: `a or b > c`

What might be meant here: `(a or b) > c`

Actual precedence: `a or (b > c)`

Misleading example:

```py
amount: int | None

amount or 0 < 100

# with amount = 101 evaluates to 101, not False!
```

---

_Label `rule` added by @ntBre on 2025-07-25 21:34_

---

_Label `needs-decision` added by @ntBre on 2025-07-25 21:34_

---

_Comment by @gorewilliams on 2025-07-26 08:58_

Looks to me like something that should be added to `RUF021`

---

_Comment by @MichaReiser on 2025-07-28 09:54_

RUF021 certainly has overlap but we've also seen in the past that users like to have very fine grain control. That might justify a separate rule. (for context, this is where RUF021 was introduced https://github.com/astral-sh/ruff/issues/8721)

---
