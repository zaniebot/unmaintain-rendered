```yaml
number: 19384
title: "[docs] DTZ011: Docs confuse `date` with `datetime`"
type: issue
state: closed
author: srittau
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-07-16T10:52:19Z
updated_at: 2025-10-10T13:02:26Z
url: https://github.com/astral-sh/ruff/issues/19384
synced_at: 2026-01-12T15:54:56Z
```

# [docs] DTZ011: Docs confuse `date` with `datetime`

---

_@srittau_

### Summary

Currently, [the docs](https://docs.astral.sh/ruff/rules/call-date-today/#call-date-today-dtz011) says:

> Python datetime objects can be naive or timezone-aware. While an aware object represents a specific moment in time, a naive object does not contain enough information to unambiguously locate itself relative to other datetime objects. Since this can lead to errors, it is recommended to always use timezone-aware objects.
> 
> datetime.date.today returns a naive datetime object. Instead, use datetime.datetime.now(tz=...).date() to create a timezone-aware object.

This is mentioning `datetime` objects multiple times, but this rule is about `date` objects. `date` objects can never be tz-aware (and are also not considered naïve, because that concept makes no sense for them). Overall I find the rule problematic conceptually, but the documentation should at least be accurate and don't mention `datetime` objects, or at least mention them in relation to `date` objects.

### Version

_No response_

---

_Comment by @srittau on 2025-07-16 11:04_

Ignore me. It's me who's confused about `date` and `datetime` in this rule.

---

_Closed by @srittau on 2025-07-16 11:04_

---

_Reopened by @srittau on 2025-07-16 11:12_

---

_Comment by @srittau on 2025-07-16 11:13_

Actually don't ignore me. Now I've confused DTZ002 (which is about `datetime`) and DTZ011 (which is about `date`).

---

_Label `documentation` added by @ntBre on 2025-07-16 13:35_

---

_Label `help wanted` added by @MichaReiser on 2025-07-18 13:42_

---

_Comment by @ageorgou on 2025-10-08 20:23_

[DTZ012](https://docs.astral.sh/ruff/rules/call-date-fromtimestamp/) also has the same issue. I can take a pass at correcting the documentation for them to mention `date` objects.

---

_Closed by @ntBre on 2025-10-10 13:02_

---
