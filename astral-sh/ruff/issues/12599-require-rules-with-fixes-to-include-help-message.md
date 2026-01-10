```yaml
number: 12599
title: Require rules with fixes to include help message
type: issue
state: open
author: chriskrycho
labels:
  - fixes
assignees: []
created_at: 2024-07-31T18:12:35Z
updated_at: 2024-07-31T18:16:00Z
url: https://github.com/astral-sh/ruff/issues/12599
synced_at: 2026-01-10T11:09:54Z
```

# Require rules with fixes to include help message

---

_Issue opened by @chriskrycho on 2024-07-31 18:12_

While working on #12595, we noticed that some fixes do not have a corresponding help message. This means that when displaying those to the user, the fix can be displayed with no explanation or context.

An alternative design here might be to fall back to “Suggested fix” or something like that as a stopgap when there is no help message available.

---

_Label `fixes` added by @AlexWaygood on 2024-07-31 18:16_

---
