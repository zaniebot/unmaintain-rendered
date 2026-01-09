---
number: 10456
title: "Re-write `Q001-003` to use string flags"
type: issue
state: open
author: dhruvmanila
labels:
  - rule
assignees: []
created_at: 2024-03-18T13:26:11Z
updated_at: 2024-03-18T13:26:12Z
url: https://github.com/astral-sh/ruff/issues/10456
synced_at: 2026-01-07T13:12:15-06:00
---

# Re-write `Q001-003` to use string flags

---

_Issue opened by @dhruvmanila on 2024-03-18 13:26_

Currently, the rules `Q001`, `Q002`, and `Q003` extracts the trivia information from the source code. With https://github.com/astral-sh/ruff/pull/10298, we have all of the information available on the AST node. This can help simplify the implementation.

There's a `raw_text` field on `Trivia` which might still be required but need to check it's usages to confirm.

This is a low priority task.

---

_Label `rule` added by @dhruvmanila on 2024-03-18 13:26_

---

_Referenced in [astral-sh/ruff#10312](../../astral-sh/ruff/pulls/10312.md) on 2024-03-19 09:54_

---
