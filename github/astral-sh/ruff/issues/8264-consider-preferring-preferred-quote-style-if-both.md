---
number: 8264
title: Consider preferring preferred quote style if both styles require backslashes
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
  - style
assignees: []
created_at: 2023-10-26T22:42:18Z
updated_at: 2023-11-28T06:09:45Z
url: https://github.com/astral-sh/ruff/issues/8264
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider preferring preferred quote style if both styles require backslashes

---

_Issue opened by @charliermarsh on 2023-10-26 22:42_

Given:

```python
"json enter input={'a': 2} kwargs={\"strict\": None, \"context\": {\"c\": 2}}"
```

Black will reformat as:

```python
'json enter input={\'a\': 2} kwargs={"strict": None, "context": {"c": 2}}'
```

Ruff does the same thing, so it's not a compatibility issue. But I'm wondering if it makes more sense to bias towards the preferred quote style if you need backslashes regardless (even if the non-preferred style results in _fewer_ backslashes).

This came up in the Pydantic migration: https://github.com/pydantic/pydantic/pull/7930/files#r1373829517


---

_Label `formatter` added by @charliermarsh on 2023-10-26 22:42_

---

_Referenced in [pydantic/pydantic#7930](../../pydantic/pydantic/pulls/7930.md) on 2023-10-26 22:42_

---

_Label `needs-decision` added by @MichaReiser on 2023-10-26 23:22_

---

_Comment by @MichaReiser on 2023-10-26 23:24_

I'm leaning toward Black compatibility. It doesn't seem we have strong reasoning to diverge. E.g. for the above example, I find Black's formatting easier to read and it results in a shorter string overall. 

```
"json enter input={'a': 2} kwargs={\"strict\": None, \"context\": {\"c\": 2}}"
```

---

_Label `needs-decision` removed by @MichaReiser on 2023-10-26 23:53_

---

_Label `needs-decision` added by @MichaReiser on 2023-10-26 23:53_

---

_Label `style` added by @MichaReiser on 2023-10-26 23:53_

---

_Label `needs-decision` removed by @MichaReiser on 2023-10-27 02:01_

---

_Comment by @MichaReiser on 2023-11-28 06:09_

It doesn't seem to have come up again. We can improve our documentation if this comes up more often. @charliermarsh feel free to re-open if you disagree with my decision.

---

_Closed by @MichaReiser on 2023-11-28 06:09_

---
