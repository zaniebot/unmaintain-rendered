```yaml
number: 16265
title: "[red-knot] Use salsa query in `Class::own_instance_member`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/own-instance-member-query
created_at: 2025-02-19T21:03:05Z
updated_at: 2025-05-03T17:47:44Z
url: https://github.com/astral-sh/ruff/pull/16265
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Use salsa query in `Class::own_instance_member`

---

_@MichaReiser_

## Summary
Fixes https://github.com/astral-sh/ruff/issues/16172

I considered making `member` a query but that has the downside that `name` needs to be a `String` (or at least a `Name`, it can't be a borrowed `&str` because Salsa has to intern the value). 

That's why I opted to make a local query around the specific `own_instance_member` branch that is problematic because it only requires interning `Class` and `ScopedSymbolId`, which should be cheap, considering that both are u32.


## Test Plan

I have to write one of those terrible salsa `did_run_query` tests, ugh


---

_Label `red-knot` added by @MichaReiser on 2025-02-19 21:03_

---

_Comment by @MichaReiser on 2025-02-19 21:11_

This seems to be a neglectable `cold` speedup (0.2ms) and a 1% incremental speedup.

---

_Closed by @MichaReiser on 2025-02-20 11:32_

---

_Comment by @MichaReiser on 2025-02-20 11:42_

Closing in favor of https://github.com/astral-sh/ruff/pull/16268

---

_Branch deleted on 2025-05-03 17:47_

---
