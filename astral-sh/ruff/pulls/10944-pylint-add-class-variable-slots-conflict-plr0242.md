```yaml
number: 10944
title: "[`pylint`] - add `class-variable-slots-conflict` (`PLR0242`)"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
base: main
head: add-E0242
created_at: 2024-04-15T05:49:30Z
updated_at: 2025-01-17T22:15:16Z
url: https://github.com/astral-sh/ruff/pull/10944
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] - add `class-variable-slots-conflict` (`PLR0242`)

---

_@diceroll123_

## Summary

Adds [`class-variable-slots-conflict`](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/class-variable-slots-conflict.html#class-variable-slots-conflict-e0242)

See #970 

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-04-15 06:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-04-15 08:24_

I think this PR and https://github.com/astral-sh/ruff/pull/10317 both implement the same rule

---

_Comment by @dylwil3 on 2025-01-17 22:15_

Gonna close this as a duplicate - hopefully we'll get the first PR sorted soon, too. Thank you for the contribution though, and apologies for the delay in an admittedly unsatisfactory resolution!

---

_Closed by @dylwil3 on 2025-01-17 22:15_

---
