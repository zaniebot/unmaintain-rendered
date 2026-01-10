```yaml
number: 11971
title: "[red-knot]: Implement `HasTy` for `Alias`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: alias-has-type
created_at: 2024-06-21T16:11:00Z
updated_at: 2024-07-04T07:27:04Z
url: https://github.com/astral-sh/ruff/pull/11971
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot]: Implement `HasTy` for `Alias`

---

_Pull request opened by @MichaReiser on 2024-06-21 16:11_

## Summary

This PR implements `HasTy` for `Alias` as a preparation to implement the bad-imports lint rules. 


## Test Plan

Implemented the `bad-overrides` lint rule on top of the  new API


---

_Converted to draft by @MichaReiser on 2024-06-21 16:11_

---

_Label `red-knot` added by @MichaReiser on 2024-06-21 16:11_

---

_Comment by @MichaReiser on 2024-06-21 16:11_

This is a draft PR because it requires more documentation and some of the implementation feels errorprone. 

---

_Comment by @github-actions[bot] on 2024-06-21 16:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-07-02 13:46_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-03 13:28_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-03 13:28_

---

_@AlexWaygood approved on 2024-07-03 13:31_

---

_Comment by @carljm on 2024-07-03 21:46_

This is obsolete now, right? Should it be closed?

EDIT: never mind, I guess this is rebased now on top of make-definition-an-ingredient.

---

_@carljm approved on 2024-07-03 21:48_

---

_Comment by @MichaReiser on 2024-07-04 06:11_

>  never mind, I guess this is rebased now on top of make-definition-an-ingredient.

Yes, it's now a very minimal change :)

---

_Merged by @MichaReiser on 2024-07-04 07:17_

---

_Closed by @MichaReiser on 2024-07-04 07:17_

---

_Branch deleted on 2024-07-04 07:17_

---
