```yaml
number: 19649
title: Introduce a system to suppress existing errors
type: issue
state: closed
author: InsidersByte
labels: []
assignees: []
created_at: 2025-07-30T20:57:24Z
updated_at: 2025-07-30T20:59:52Z
url: https://github.com/astral-sh/ruff/issues/19649
synced_at: 2026-01-10T11:09:59Z
```

# Introduce a system to suppress existing errors

---

_Issue opened by @InsidersByte on 2025-07-30 20:57_

I would like there to be a system to suppress existing errors from being reported, so that I can select a new rule and be notified when new ones show up.

This would allow for gradual adoption of the new rule within a code base instead of doing it all at once.

[ESLint has recently added this as a feature](https://eslint.org/blog/2025/04/introducing-bulk-suppressions/). Their RFC for it can be found [here](https://github.com/eslint/rfcs/tree/main/designs/2024-baseline-support).

---

_Comment by @MichaReiser on 2025-07-30 20:59_

Makes sense, see https://github.com/astral-sh/ruff/issues/1149

---

_Closed by @MichaReiser on 2025-07-30 20:59_

---
