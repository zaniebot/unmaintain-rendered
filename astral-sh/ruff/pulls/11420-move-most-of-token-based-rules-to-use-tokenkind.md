```yaml
number: 11420
title: "Move most of token-based rules to use `TokenKind`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/token-kind-1
created_at: 2024-05-14T03:40:44Z
updated_at: 2024-05-14T17:22:22Z
url: https://github.com/astral-sh/ruff/pull/11420
synced_at: 2026-01-10T22:05:26Z
```

# Move most of token-based rules to use `TokenKind`

---

_Pull request opened by @dhruvmanila on 2024-05-14 03:40_

## Summary

This PR moves the following rules to use `TokenKind` instead of `Tok`:
* `PLE2510`, `PLE2512`, `PLE2513`, `PLE2514`, `PLE2515`
* `E701`, `E702`, `E703`
* `ISC001`, `ISC002`
* `COM812`, `COM818`, `COM819`
* `W391`

I've paused here because the next set of rules (`pyupgrade::rules::extraneous_parentheses`) indexes into the token slice but we only have an iterator implementation. So, I want to isolate that change to make sure the logic is still the same when I move to using the iterator approach.

This is part of #11401 

## Test Plan

`cargo test`


---

_Label `internal` added by @dhruvmanila on 2024-05-14 03:40_

---

_Comment by @github-actions[bot] on 2024-05-14 10:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-05-14 11:33_

LGTM

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-14 11:43_

---

_Renamed from "Move token-based rules to use `TokenKind` (1)" to "Move most of token-based rules to use `TokenKind`" by @dhruvmanila on 2024-05-14 12:32_

---

_Merged by @dhruvmanila on 2024-05-14 17:16_

---

_Closed by @dhruvmanila on 2024-05-14 17:16_

---

_Branch deleted on 2024-05-14 17:16_

---
