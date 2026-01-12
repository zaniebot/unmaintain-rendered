```yaml
number: 8578
title: Add TRIO210 rule
type: pull_request
state: closed
author: karpetrosyan
labels: []
assignees: []
base: main
head: add-trio-210-support
created_at: 2023-11-09T09:23:15Z
updated_at: 2023-11-10T03:46:33Z
url: https://github.com/astral-sh/ruff/pull/8578
synced_at: 2026-01-10T23:40:55Z
```

# Add TRIO210 rule

---

_Pull request opened by @karpetrosyan on 2023-11-09 09:23_

## Summary

Adds TRIO210 from the [flake8-trio plugin](https://github.com/Zac-HD/flake8-trio).
Relates to: https://github.com/astral-sh/ruff/issues/8451

---

_Comment by @github-actions[bot] on 2023-11-09 10:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-10 01:28_

I think this might be the same as `ASYNC100` (`blocking_http_call.rs`). Could we instead extend the list there to include all of these methods?

---

_Closed by @karpetrosyan on 2023-11-10 03:46_

---
