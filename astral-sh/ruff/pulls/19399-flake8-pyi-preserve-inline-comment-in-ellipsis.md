```yaml
number: 19399
title: "[`flake8-pyi`] Preserve inline comment in ellipsis removal (`PYI013`)"
type: pull_request
state: merged
author: Luunynliny
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/19385_piy013_deletes_comments
created_at: 2025-07-17T14:11:27Z
updated_at: 2025-07-29T19:06:05Z
url: https://github.com/astral-sh/ruff/pull/19399
synced_at: 2026-01-12T15:56:38Z
```

# [`flake8-pyi`] Preserve inline comment in ellipsis removal (`PYI013`)

---

_@Luunynliny_

## Summary

Fixes #19385.

Based on [unnecessary-placeholder (PIE790)](https://docs.astral.sh/ruff/rules/unnecessary-placeholder/) behavior, [ellipsis-in-non-empty-class-body (PYI013)](https://docs.astral.sh/ruff/rules/ellipsis-in-non-empty-class-body/) now safely preserve inline comment on ellipsis removal.

## Test Plan

A new test class was added:

```python
class NonEmptyChildWithInlineComment:
    value: int
    ... # preserve me
```

with the following expected fix:

```python
class NonEmptyChildWithInlineComment:
    value: int
    # preserve me
```


---

_Review requested from @AlexWaygood by @Luunynliny on 2025-07-17 14:11_

---

_Label `bug` added by @ntBre on 2025-07-17 14:21_

---

_Label `fixes` added by @ntBre on 2025-07-17 14:21_

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:08_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI013.pyi`:5 on 2025-07-28 18:50_

I think we should revert these comment removals. Maybe you accidentally ran ruff on the file before your fix? 😄 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/ellipsis_in_non_empty_class_body.rs`:67 on 2025-07-28 18:53_

tiny nit: I think we could do without these two comments, but I like the comment on the outer `let edit`.

---

_@ntBre reviewed on 2025-07-28 18:57_

Very nice, thank you! And apologies for the delay in reviewing this.

I had a couple of small nits, but the overall fix looks perfect.

---

_Comment by @github-actions[bot] on 2025-07-28 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-07-28 19:19_

Don't worry about the ecosystem changes yet. I think it's relative to `main` when the PR was opened rather than when it actually ran. After we push any changes I expect that to update to something more reasonable.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-28 23:12_

---

_Review requested from @ntBre by @Luunynliny on 2025-07-28 23:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI013_PYI013.pyi.snap`:1 on 2025-07-29 17:47_

I think you need to update the snapshots now for the comment restoration. This is ready to go otherwise!

---

_@ntBre reviewed on 2025-07-29 17:47_

---

_@ntBre approved on 2025-07-29 19:05_

Thank you!

---

_Renamed from "[flake8-pyi] Preserve inline comment in ellipsis removal (PYI013)" to "[`flake8-pyi`] Preserve inline comment in ellipsis removal (`PYI013`)" by @ntBre on 2025-07-29 19:05_

---

_Merged by @ntBre on 2025-07-29 19:06_

---

_Closed by @ntBre on 2025-07-29 19:06_

---
