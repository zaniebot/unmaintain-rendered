---
number: 16949
title: "Ensure generator used with `contextlib.contextmanager` yields only once"
type: issue
state: open
author: Tinche
labels:
  - rule
assignees: []
created_at: 2025-03-24T14:15:41Z
updated_at: 2025-04-19T22:02:47Z
url: https://github.com/astral-sh/ruff/issues/16949
synced_at: 2026-01-07T13:12:16-06:00
---

# Ensure generator used with `contextlib.contextmanager` yields only once

---

_Issue opened by @Tinche on 2025-03-24 14:15_

### Summary

When applying `contextlib.contextmanager` (and its async sibling) to a generator, that generator should yield only once. It would be amazing if Ruff could detect there's a chance of it yielding more than once and raise a warning, since doing so leads to a runtime error. I was bitten by this the other day in prod ;)

Quick example: https://play.ruff.rs/86d8c906-5406-41cc-8ae9-d555b1e0d7f4

---

_Label `rule` added by @ntBre on 2025-03-24 14:19_

---

_Comment by @maxmynter on 2025-04-07 03:57_

Working on this :)

---

_Referenced in [astral-sh/ruff#17281](../../astral-sh/ruff/pulls/17281.md) on 2025-04-07 16:43_

---

_Assigned to @maxmynter by @ntBre on 2025-04-07 21:14_

---

_Comment by @maxmynter on 2025-04-19 22:02_

We have an implementation in #17281 but decided to wait on the rewrite of the control flow graph #17065 on which we can then (hopefully) use for a cleaner implementation.

Will come back to work on this once that is done. 

---
