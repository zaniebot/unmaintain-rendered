```yaml
number: 13359
title: DTZ007 - arguably a false positive
type: issue
state: open
author: rbubley
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-15T22:26:08Z
updated_at: 2025-03-25T08:19:15Z
url: https://github.com/astral-sh/ruff/issues/13359
synced_at: 2026-01-12T15:54:53Z
```

# DTZ007 - arguably a false positive

---

_@rbubley_

A not uncommon pattern to parse a date string to a Python date uses similar code to the below. Times/time-zones shouldn't matter.

```
Ruff: DTZ007 Naive datetime constructed using `datetime.datetime.strptime()` without %z
  |
1 |         # Parse the date parameter
2 |         try:
3 |             input_date = datetime.strptime(date, "%Y-%m-%d").date()
  |                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ DTZ007
4 |         except ValueError:
5 |             print("Invalid date format. Use YYYY-MM-DD.")
  |
    = help: Call `.replace(tzinfo=<timezone>)` or `.astimezone()` to convert to an aware datetime
```

---

_Comment by @MichaReiser on 2024-09-16 08:45_

To summarize your request: It is about detecting the specific usage of `datetime.strptime` where the pattern is `%Y-%m-%d`) which is followed by a call to `date()`. 

I'm not a datetime expert, but I think this pattern, indeed, doesn't require time zone information. The main challenge I see is that detecting this usage consistently without surprises requires quiet involved analysis. E.g what about:

```python
input = datetime.strptime(date, "%Y-%m-%d")
input_date = input.date()
```

What if there are multiple statements in-between? That's why I think using a `noqa` comment in this situation is probably fine.

To me, using `DateTime.strptime` over `date.fromisoformat` would express the intent more clearly. What's your reason for preferring `strptime`?

---

_Label `rule` added by @MichaReiser on 2024-09-16 08:45_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-16 08:45_

---

_Comment by @rbubley on 2024-09-16 12:25_

I think you are right. I had forgotten about `date.fromisoformat`. 

(Although if the dates were in a different, non-ISO format, e.g. `%d/%m/%Y`, it's a bit more marginal).

---

_Comment by @MichaReiser on 2024-09-16 13:04_

> (Although if the dates were in a different, non-ISO format, e.g. %d/%m/%Y, it's a bit more marginal).

That's fair. I think adding a `noqa` in this situation is reasonable. We may be able to do better once we have type inference and can guarantee that the value returned by `datetime.strptime` isn't used anywhere else other than for calling `.date`. 

---

_Comment by @axxeny on 2025-03-25 08:19_

I think it would be great to at least cover the case where `strptime` is immediately followed by `.date`. To me that looks similar to what was done in https://github.com/astral-sh/ruff/issues/1296 .

---
