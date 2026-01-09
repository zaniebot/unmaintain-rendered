---
number: 13342
title: new rule - enforce (or ban) parenthesized tuples everywhere
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-13T05:19:18Z
updated_at: 2025-01-07T17:34:02Z
url: https://github.com/astral-sh/ruff/issues/13342
synced_at: 2026-01-07T13:12:15-06:00
---

# new rule - enforce (or ban) parenthesized tuples everywhere

---

_Issue opened by @DetachHead on 2024-09-13 05:19_

[`incorrectly-parenthesized-tuple-in-subscript` (`RUF031`)](https://docs.astral.sh/ruff/rules/incorrectly-parenthesized-tuple-in-subscript/) allows you to enforce (or ban) them inside subscripts, but imo there should also be a rule to enforce it everywhere else as well:

```py
foo = 1, 2 # error: tuple should be wrapped in parentheses
```

mainly for code consistency reasons.

---

_Comment by @MichaReiser on 2024-09-13 13:08_

I did a quick look and such a rule would be incompatible with the formatter when tuples are used in a `for` loop target because the formatter removes those parentheses ([playground](https://play.ruff.rs/0632c292-29de-40ae-8bb5-5c699696c688)). 



---

_Label `rule` added by @MichaReiser on 2024-09-13 13:08_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-13 13:08_

---

_Comment by @DetachHead on 2024-09-14 00:17_

perhaps an exception can be made when the tuple is used to unpack a value, eg:

```py
for a, b in (1, 2), (3, 4):
    pass
a, b = 1, 2
```
would get converted to:
```py
for a, b in ((1, 2), (3, 4)):
    pass
a, b = (1, 2)
```

---

_Comment by @AlexWaygood on 2025-01-07 14:49_

It doesn't seem like there's much community support for this, and it conflicts with the formatter, so I think I'll close this as rejected. But thanks for the suggestion! :-)

---

_Closed by @AlexWaygood on 2025-01-07 14:49_

---

_Comment by @tdulcet on 2025-01-07 17:34_

I would support a rule that removes unnecessary parentheses, but either way, improving constancy in a code base would be great.

---
