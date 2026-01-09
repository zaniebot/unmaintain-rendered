---
number: 18739
title: RUF055 could support equivalent cases using byte strings
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - rule
assignees: []
created_at: 2025-06-18T01:20:41Z
updated_at: 2025-07-20T21:58:41Z
url: https://github.com/astral-sh/ruff/issues/18739
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF055 could support equivalent cases using byte strings

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 01:20_

### Summary

RUF055 does not trigger when the regex contains a byte string [playground](https://play.ruff.rs/75ad86db-b4c0-428f-8538-f9fa3b016799)
```py
re.sub(rb"x", b"y", b"x")
```
This could be replaced with
```py
b"x".replace(rb"x", b"y")
```
I'm unsure if every string replacement would also work for byte string replacement, or if there would be some exceptions.

### Version

playground

---

_Label `rule` added by @ntBre on 2025-06-18 03:13_

---

_Referenced in [astral-sh/ruff#18926](../../astral-sh/ruff/pulls/18926.md) on 2025-06-24 22:26_

---

_Closed by @ntBre on 2025-07-20 21:58_

---
