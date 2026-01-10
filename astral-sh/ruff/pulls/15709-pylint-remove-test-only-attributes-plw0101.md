```yaml
number: 15709
title: "[`pylint`] Remove \"test-only\" attributes (`PLW0101`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels: []
assignees: []
base: main
head: PLW0101
created_at: 2025-01-24T04:27:13Z
updated_at: 2025-01-24T23:25:26Z
url: https://github.com/astral-sh/ruff/pull/15709
synced_at: 2026-01-10T19:57:22Z
```

# [`pylint`] Remove "test-only" attributes (`PLW0101`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-24 04:27_

## Summary

Resolves #15707, reverts #15627.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-24 04:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-01-24 08:18_

As explained by @dhruvmanila in the linked issue: This rule is intentionally a test rule. @dylwil3 plans to do some more testing before promoting it to preview.

---

_Closed by @MichaReiser on 2025-01-24 08:18_

---

_Branch deleted on 2025-01-24 23:25_

---
