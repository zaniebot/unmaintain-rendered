```yaml
number: 9734
title: "Support `IfExp` with dual string arms in `invalid-envvar-default`"
type: pull_request
state: merged
author: covracer
labels:
  - bug
assignees: []
merged: true
base: main
head: invalid-envvar-default
created_at: 2024-01-31T12:43:00Z
updated_at: 2024-02-03T01:09:28Z
url: https://github.com/astral-sh/ruff/pull/9734
synced_at: 2026-01-10T22:57:09Z
```

# Support `IfExp` with dual string arms in `invalid-envvar-default`

---

_Pull request opened by @covracer on 2024-01-31 12:43_

## Summary

Just like #6537 and #6538 but for the `default` second parameter to `getenv()`.

Also rename "BAD" to "BAR" in the tests, since those strings shouldn't trigger the rule.

## Test Plan

Added passing and failing examples to `invalid_envvar_default.py`.

---

_Comment by @github-actions[bot] on 2024-01-31 13:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-01-31 15:41_

Nice, thank you!

---

_Label `bug` added by @charliermarsh on 2024-01-31 15:41_

---

_Merged by @charliermarsh on 2024-01-31 15:41_

---

_Closed by @charliermarsh on 2024-01-31 15:41_

---

_Branch deleted on 2024-02-03 01:09_

---
