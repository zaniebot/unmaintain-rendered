```yaml
number: 14043
title: Temporary comment out certain release steps
type: pull_request
state: closed
author: dhruvmanila
labels: []
assignees: []
draft: true
base: main
head: dhruv/temp-release
created_at: 2024-11-01T15:41:15Z
updated_at: 2024-11-01T16:19:17Z
url: https://github.com/astral-sh/ruff/pull/14043
synced_at: 2026-01-12T15:55:46Z
```

# Temporary comment out certain release steps

---

_@dhruvmanila_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @charliermarsh on 2024-11-01 15:42_

@dhruvmanila -- You might instead need to remove these from the `Cargo.toml`, then install `cargo-dist` at the right version and run `cargo-dist dist generate`.

---

_Comment by @github-actions[bot] on 2024-11-01 16:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Closed by @dhruvmanila on 2024-11-01 16:19_

---
