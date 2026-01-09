---
number: 412
title: "Move check enablement validation in `add_check`"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-10-12T20:32:55Z
updated_at: 2023-01-16T19:46:44Z
url: https://github.com/astral-sh/ruff/issues/412
synced_at: 2026-01-07T13:12:14-06:00
---

# Move check enablement validation in `add_check`

---

_Issue opened by @charliermarsh on 2022-10-12 20:32_

We have conditionals that look like `if checker.settings.enabled.contains(&CheckCode::D405) { ... }` all over the place. Sometimes, this is useful to avoid unnecessary work. However, it can be a little excessive, e.g., if we have a function that performs multiple checks, then we first have to verify that _any_ of those checks are enabled, and _then_ we have to validate each individual check when adding it to the checker.

Instead, we should treat those `if` conditionals as "best effort" and enforce check addition in `add_check` (or even later, when reporting).


---

_Label `internal` added by @charliermarsh on 2022-10-12 20:32_

---

_Referenced in [astral-sh/ruff#628](../../astral-sh/ruff/pulls/628.md) on 2022-11-06 22:55_

---

_Closed by @charliermarsh on 2023-01-16 19:46_

---
