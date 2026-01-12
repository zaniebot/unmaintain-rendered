```yaml
number: 14017
title: SIM108 is bad for code coverage
type: issue
state: closed
author: maddrum
labels:
  - rule
assignees: []
created_at: 2024-10-31T13:23:01Z
updated_at: 2024-10-31T13:44:24Z
url: https://github.com/astral-sh/ruff/issues/14017
synced_at: 2026-01-12T15:54:53Z
```

# SIM108 is bad for code coverage

---

_@maddrum_

`SIM108` suggests using ternary operators whenever possible due to readability. 

But ternary operators can not be covered.

Here is a well-known issue of one of the most used Python coverage tools - `coverage.py`

[https://github.com/nedbat/coveragepy/issues/509](https://github.com/nedbat/coveragepy/issues/509)

SIM108 should be completely removed since that is not the only issue mentioned here. It creates more problems than it actually solves.

And at the end of the day, it is quite a personal opinion if a ternary operator is easier to read, than if/else block

---

_Label `rule` added by @MichaReiser on 2024-10-31 13:41_

---

_Comment by @MichaReiser on 2024-10-31 13:44_

Thanks for raising this. It's good to know that there are more concerns with SIM108.

I'll merge this into https://github.com/astral-sh/ruff/issues/5528 because it's fine if Ruff supports that don't fit everyone's needs. Users can disable the rule and it isn't part of the default rule set. But it's good to be aware of that this is another issue. 

---

_Closed by @MichaReiser on 2024-10-31 13:44_

---
