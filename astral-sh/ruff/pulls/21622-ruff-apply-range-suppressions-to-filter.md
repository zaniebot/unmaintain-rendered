```yaml
number: 21622
title: "[ruff] apply range suppressions to filter diagnostics"
type: pull_request
state: closed
author: amyreese
labels: []
assignees: []
base: main
head: suppressing-diagnostics
created_at: 2025-11-25T01:57:14Z
updated_at: 2025-11-25T01:59:11Z
url: https://github.com/astral-sh/ruff/pull/21622
synced_at: 2026-01-10T16:48:02Z
```

# [ruff] apply range suppressions to filter diagnostics

---

_Pull request opened by @amyreese on 2025-11-25 01:57_

- **New module for parsing generic ruff suppressions**
- **Add errors and test cases to exercise new parsing**
- **Fix lint**
- **Drop reference to ignore directive**
- **Basic suppression comment matching**
- **Replace CommentRanges based builder with Tokens based builder, some refactoring**
- **Move from usize to strings for tracking indents, better match target**
- **Track a reason that a comment is considered invalid**
- **Ignore `#ruff:noqa` file level suppressions for now**
- **More comments of what's happening**
- **Prototype filtering diagnostics by range suppressions**


---

_Closed by @amyreese on 2025-11-25 01:59_

---

_Branch deleted on 2025-11-25 01:59_

---
