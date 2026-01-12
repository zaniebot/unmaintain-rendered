```yaml
number: 10921
title: "[`refurb`] Implement `fstring-number-format` (`FURB116`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-FURB116
created_at: 2024-04-14T01:58:24Z
updated_at: 2024-04-26T01:19:14Z
url: https://github.com/astral-sh/ruff/pull/10921
synced_at: 2026-01-12T15:55:33Z
```

# [`refurb`] Implement `fstring-number-format` (`FURB116`)

---

_@diceroll123_

## Summary

Adds `FURB116`

See #1348 

## Test Plan

`cargo test`


---

_Comment by @diceroll123 on 2024-04-14 01:59_

Not too sure how I'd reliably be able to support expressions that can include strings, such as calls, like in my `int(f"{num}")` test, for example. Please advise if that can be better! 😄 

---

_Comment by @github-actions[bot] on 2024-04-14 02:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@diceroll123 reviewed on 2024-04-14 02:45_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:112 on 2024-04-14 02:45_

If #10919 is merged before this, I'll update accordingly.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:112 on 2024-04-16 12:00_

it was merged ;)

---

_@AlexWaygood reviewed on 2024-04-16 12:00_

---

_@charliermarsh approved on 2024-04-26 01:07_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-04-26 01:07_

---

_Label `preview` added by @charliermarsh on 2024-04-26 01:07_

---

_Renamed from "[`refurb`] - add `fstring-number-format` with sometimes-autofix (`FURB116`)" to "[`refurb`] Implement `fstring-number-format` (`FURB116`)" by @charliermarsh on 2024-04-26 01:07_

---

_Merged by @charliermarsh on 2024-04-26 01:15_

---

_Closed by @charliermarsh on 2024-04-26 01:15_

---
