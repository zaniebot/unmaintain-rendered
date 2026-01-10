```yaml
number: 15840
title: "[`pyupgrade`] Reuse replacement logic from `UP046` and `UP047` (`UP040`)"
type: pull_request
state: merged
author: ntBre
labels:
  - fixes
assignees: []
merged: true
base: main
head: brent/pep695-string-fix
created_at: 2025-01-30T22:54:54Z
updated_at: 2025-01-31T13:10:55Z
url: https://github.com/astral-sh/ruff/pull/15840
synced_at: 2026-01-10T19:57:22Z
```

# [`pyupgrade`] Reuse replacement logic from `UP046` and `UP047` (`UP040`)

---

_Pull request opened by @ntBre on 2025-01-30 22:54_

## Summary

This is a follow-up to #15565, tracked in #15642, to reuse the string replacement logic from the other PEP 695 rules instead of the `Generator`, which has the benefit of preserving more comments. However, comments in some places are still dropped, so I added a check for this and update the fix safety accordingly. I also added a `## Fix safety` section to the docs to reflect this and the existing `isinstance` caveat.

## Test Plan

Existing UP040 tests, plus some new cases.


---

_Comment by @github-actions[bot] on 2025-01-30 23:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-01-30 23:01_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:225 on 2025-01-30 23:01_

This is unrelated to my changes here, but I think this TODO is actually done? All of the TODO test cases added in #6314 were fixed in #6749, but maybe there are more types of generics that are still not handled.

https://github.com/astral-sh/ruff/blob/4f2aea8d5088c3ba2f7857540bca97784b1f6ff1/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs#L187-L188

---

_@AlexWaygood approved on 2025-01-30 23:03_

Fantastic! Simplifies the code, improves the quality of the fix, improves the rule's docs and improves our test coverage 🚀

---

_@AlexWaygood reviewed on 2025-01-30 23:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:225 on 2025-01-30 23:04_

Ooh interesting, this might well be done now. I can check in the morning. Doesn't need to hold up this PR though, I don't think!

---

_Label `fixes` added by @AlexWaygood on 2025-01-30 23:05_

---

_@AlexWaygood reviewed on 2025-01-31 11:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:225 on 2025-01-31 11:23_

Yeah, this TODO seems done!

---

_@ntBre reviewed on 2025-01-31 12:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:225 on 2025-01-31 12:49_

Awesome! I'll delete it and then get ready to merge. Thanks again for the review and for checking this!

---

_Merged by @ntBre on 2025-01-31 13:10_

---

_Closed by @ntBre on 2025-01-31 13:10_

---

_Branch deleted on 2025-01-31 13:10_

---
