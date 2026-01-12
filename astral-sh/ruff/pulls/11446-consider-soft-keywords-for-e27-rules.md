```yaml
number: 11446
title: "Consider soft keywords for `E27` rules"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/e27-soft-keyword
created_at: 2024-05-16T11:39:48Z
updated_at: 2024-05-20T05:47:44Z
url: https://github.com/astral-sh/ruff/pull/11446
synced_at: 2026-01-12T15:55:38Z
```

# Consider soft keywords for `E27` rules

---

_@dhruvmanila_

## Summary

This is a follow-up PR to #11445 update the `E27` rules to consider soft keywords as well.

## Test Plan

Add test cases consisting of soft keywords and update the snapshot.


---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-16 11:40_

---

_Label `rule` added by @dhruvmanila on 2024-05-16 11:43_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-05-16 11:43_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-17 11:50_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-17 11:50_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/token.rs`:547 on 2024-05-17 11:58_

I worry that this is somewhat fragile — if we added `TokenKind::Aardvark` in the future because of some new Python syntax, we might forget to update this method, which could possibly cause subtle bugs. But I think my comment here also applies to the current implementation of this method, so I think this is fine 

---

_@AlexWaygood approved on 2024-05-17 11:59_

Nice

---

_@dhruvmanila reviewed on 2024-05-17 12:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:547 on 2024-05-17 12:14_

Yeah, I think it's fine unless Python introduces a very obscure keyword ;)

(This change was part of the previous PR #11445)

---

_Comment by @github-actions[bot] on 2024-05-17 12:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-05-20 05:38_

---

_Closed by @dhruvmanila on 2024-05-20 05:38_

---

_Branch deleted on 2024-05-20 05:38_

---
