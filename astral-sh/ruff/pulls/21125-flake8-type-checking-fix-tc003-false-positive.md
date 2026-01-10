```yaml
number: 21125
title: "[`flake8-type-checking`] Fix `TC003` false positive with `future-annotations`"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/future-annotations-kw-only
created_at: 2025-10-29T14:53:04Z
updated_at: 2025-10-30T18:14:30Z
url: https://github.com/astral-sh/ruff/pull/21125
synced_at: 2026-01-10T16:59:49Z
```

# [`flake8-type-checking`] Fix `TC003` false positive with `future-annotations`

---

_Pull request opened by @ntBre on 2025-10-29 14:53_

Summary
--

Fixes #21121 by upgrading `RuntimeEvaluated` annotations like
`dataclasses.KW_ONLY` to `RuntimeRequired`. We already had special handling for
`TypingOnly` annotations in this context but not `RuntimeEvaluated`. Combining
that with the `future-annotations` setting, which allowed ignoring the
`RuntimeEvaluated` flag, led to the reported bug where we would try to move
`KW_ONLY` into a `TYPE_CHECKING` block.

Test Plan
--

A new test based on the issue


---

_Label `bug` added by @ntBre on 2025-10-29 14:53_

---

_Marked ready for review by @ntBre on 2025-10-29 15:03_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-29 15:03_

---

_Comment by @github-actions[bot] on 2025-10-29 15:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @ntBre on 2025-10-29 15:16_

---

_Comment by @ntBre on 2025-10-29 15:17_

I should have waited a second longer before taking it out of draft. I think the ecosystem hit is a false positive, taking a closer look now.

---

_Comment by @ntBre on 2025-10-29 15:45_

It looks like we have 4 pre-existing UP037 false positives on this project:
[`src/latch/ldata/_transfer/upload.py`](https://github.com/latchbio/latch/blob/main/src/latch/ldata/_transfer/upload.py):121:27: UP037 `[*]` Remove quotes from type annotation
[`src/latch/ldata/_transfer/upload.py`](https://github.com/latchbio/latch/blob/main/src/latch/ldata/_transfer/upload.py):122:33: UP037 `[*]` Remove quotes from type annotation
[`src/latch/ldata/_transfer/upload.py`](https://github.com/latchbio/latch/blob/main/src/latch/ldata/_transfer/upload.py):132:28: UP037 `[*]` Remove quotes from type annotation
[`src/latch/types/directory.py`](https://github.com/latchbio/latch/blob/main/src/latch/types/directory.py):162:36: UP037 `[*]` Remove quotes from type annotation

but this PR adds a fifth.



---

_Comment by @ntBre on 2025-10-29 16:47_

Now the ecosystem check looks good! We needed to visit the whole `RuntimeEvaluated` annotation as `RuntimeRequired` for the special `dataclass` types, unlike the existing `TypingOnly` branch, which could treat the annotation and slice separately.

---

_Marked ready for review by @ntBre on 2025-10-29 16:47_

---

_Review requested from @amyreese by @MichaReiser on 2025-10-29 21:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1428 on 2025-10-30 13:18_

Nit: I would move this up right below the other `AnnotationContext::RuntimeEvaluated` arm. Not only reduces it the size of the diff, it also makes it easier to "match" the different arms when reading the code because you don't need to keep track of the only partially matched arms (or, you have but only up to the next arm)

---

_@MichaReiser approved on 2025-10-30 13:18_

---

_Merged by @ntBre on 2025-10-30 18:14_

---

_Closed by @ntBre on 2025-10-30 18:14_

---

_Branch deleted on 2025-10-30 18:14_

---
