```yaml
number: 13852
title: "[`ruff`] - add autofix for `useless-if-else` (`RUF034`)"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
base: main
head: autofix-RUF034
created_at: 2024-10-21T03:54:50Z
updated_at: 2024-10-22T00:53:01Z
url: https://github.com/astral-sh/ruff/pull/13852
synced_at: 2026-01-10T20:59:37Z
```

# [`ruff`] - add autofix for `useless-if-else` (`RUF034`)

---

_Pull request opened by @diceroll123 on 2024-10-21 03:54_

## Summary

Adds (sometimes-applicable) autofix for `useless-if-else` https://docs.astral.sh/ruff/rules/RUF034

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-10-21 04:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-10-21 10:57_

I'm unsure if an autofix is a good idea for this rule, because it's hard for us to detect exactly what the mistake was. We can "fix" the code so that the violation no longer occurs, but we almost certainly can't figure out what the user actually wanted to write.

The decision not to implement an autofix for this rule in its initial implementation was a deliberate choice: see https://github.com/astral-sh/ruff/pull/13218#issuecomment-2326283487

---

_Closed by @diceroll123 on 2024-10-22 00:53_

---
