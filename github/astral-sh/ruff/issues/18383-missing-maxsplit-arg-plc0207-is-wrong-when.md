---
number: 18383
title: missing-maxsplit-arg (PLC0207) is wrong when accessing on the last value
type: issue
state: closed
author: fedexman
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-05-30T06:49:36Z
updated_at: 2025-07-08T16:53:25Z
url: https://github.com/astral-sh/ruff/issues/18383
synced_at: 2026-01-07T13:12:16-06:00
---

# missing-maxsplit-arg (PLC0207) is wrong when accessing on the last value

---

_Issue opened by @fedexman on 2025-05-30 06:49_

### Summary

```
>>> url = "www.example.com"
>>> url.split(".", maxsplit=1)[0]
'www'
>>> url.split(".", maxsplit=1)[-1]
'example.com'
>>> url.split(".")[-1]
'com'
```
we can see the the maxsplit change the result when getting the last element, but ruff suggest: PLC0207 Accessing only the first or **last element** of `str.split()` without setting `maxsplit=1`
but this rule doesn't make sense for the last element, it should only be suggested for the first element, or I'm missing something 🤔 

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Comment by @ntBre on 2025-05-30 11:58_

You have to use `rsplit` for the fix in that case. We should update the error message and the docs to point this out.

```pycon
>>> url = "www.example.com"
>>> url.split(".", maxsplit=1)[-1]
'example.com'
>>> url.split(".")[-1]
'com'
>>> url.rsplit(".", maxsplit=1)[-1]
'com'
```

---

_Label `documentation` added by @ntBre on 2025-05-30 11:58_

---

_Label `help wanted` added by @ntBre on 2025-05-30 11:59_

---

_Referenced in [astral-sh/ruff#18949](../../astral-sh/ruff/pulls/18949.md) on 2025-06-26 00:42_

---

_Closed by @ntBre on 2025-07-08 16:53_

---
