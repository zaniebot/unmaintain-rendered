---
number: 13547
title: "Idea: replace integer value for month with constants"
type: issue
state: open
author: hofrob
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-09-28T15:53:42Z
updated_at: 2024-09-29T15:11:52Z
url: https://github.com/astral-sh/ruff/issues/13547
synced_at: 2026-01-07T13:12:15-06:00
---

# Idea: replace integer value for month with constants

---

_Issue opened by @hofrob on 2024-09-28 15:53_

# `calendar` package

Python 3.12 added constants to the calendar package: https://docs.python.org/3.12/library/calendar.html#calendar.JANUARY

`ruff` should be able to find and replace integer values for the `datetime.datetime` or `datetime.date` constructor (and maybe others) with the corresponding constants.

# weekday

There are also constants for weekdays. But I haven't thought where this could be useful (and easy to find/autofix).

---

_Label `rule` added by @AlexWaygood on 2024-09-29 15:11_

---

_Label `needs-decision` added by @AlexWaygood on 2024-09-29 15:11_

---
