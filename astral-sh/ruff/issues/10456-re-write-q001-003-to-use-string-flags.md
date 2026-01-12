```yaml
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
synced_at: 2026-01-12T15:54:50Z
```

# Re-write `Q001-003` to use string flags

---

_@dhruvmanila_

Currently, the rules `Q001`, `Q002`, and `Q003` extracts the trivia information from the source code. With https://github.com/astral-sh/ruff/pull/10298, we have all of the information available on the AST node. This can help simplify the implementation.

There's a `raw_text` field on `Trivia` which might still be required but need to check it's usages to confirm.

This is a low priority task.

---

_Label `rule` added by @dhruvmanila on 2024-03-18 13:26_

---
