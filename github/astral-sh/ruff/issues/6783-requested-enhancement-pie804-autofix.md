---
number: 6783
title: "[Requested enhancement] `PIE804` autofix"
type: issue
state: closed
author: tylerlaprade
labels:
  - fixes
assignees: []
created_at: 2023-08-22T17:09:40Z
updated_at: 2023-10-11T01:07:35Z
url: https://github.com/astral-sh/ruff/issues/6783
synced_at: 2026-01-07T13:12:15-06:00
---

# [Requested enhancement] `PIE804` autofix

---

_Issue opened by @tylerlaprade on 2023-08-22 17:09_

It seems non-breaking to replace
```
**{
    "foo": foo_value,
    "bar": bar_value,
  }
```
with
```
foo=foo_value,
bar=bar_value,
```
, unless I'm missing something. Dynamic keys might be trickier but could be excluded from an initial implementation.

---

_Label `autofix` added by @zanieb on 2023-08-22 17:22_

---

_Referenced in [astral-sh/ruff#7884](../../astral-sh/ruff/pulls/7884.md) on 2023-10-10 03:22_

---

_Closed by @charliermarsh on 2023-10-11 01:07_

---
