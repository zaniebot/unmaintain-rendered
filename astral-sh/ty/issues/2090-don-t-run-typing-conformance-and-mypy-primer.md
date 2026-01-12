```yaml
number: 2090
title: "Don't run \"typing conformance\" and mypy_primer workflows on PRs that only touch the ty_ide crate"
type: issue
state: closed
author: AlexWaygood
labels:
  - ci
assignees: []
created_at: 2025-12-18T21:59:46Z
updated_at: 2025-12-19T19:31:22Z
url: https://github.com/astral-sh/ty/issues/2090
synced_at: 2026-01-12T15:54:26Z
```

# Don't run "typing conformance" and mypy_primer workflows on PRs that only touch the ty_ide crate

---

_@AlexWaygood_

A PR that only touches code in that crate should never have any impact on memory usage or diagnostics produced. And the comments from the bot just lead to additional notifications which is annoying.

---

_Label `ci` added by @AlexWaygood on 2025-12-18 21:59_

---

_Closed by @AlexWaygood on 2025-12-19 19:31_

---
