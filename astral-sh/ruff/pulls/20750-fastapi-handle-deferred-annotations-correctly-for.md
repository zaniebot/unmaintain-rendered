```yaml
number: 20750
title: "[`fastapi`] Handle deferred annotations correctly for `FAST001` and `FAST003`"
type: pull_request
state: closed
author: dylwil3
labels:
  - bug
  - rule
assignees: []
draft: true
base: main
head: defer-fast
created_at: 2025-10-07T16:40:47Z
updated_at: 2025-12-10T18:03:54Z
url: https://github.com/astral-sh/ruff/pull/20750
synced_at: 2026-01-10T16:42:11Z
```

# [`fastapi`] Handle deferred annotations correctly for `FAST001` and `FAST003`

---

_Pull request opened by @dylwil3 on 2025-10-07 16:40_

Progress towards #20747


---

_Label `bug` added by @dylwil3 on 2025-10-07 16:40_

---

_Label `rule` added by @dylwil3 on 2025-10-07 16:40_

---

_Comment by @github-actions[bot] on 2025-10-07 16:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-10-07 17:09_

Nice! This makes sense to me.

We just need to wait to merge since the release is already underway

---

_@MichaReiser reviewed on 2025-10-07 17:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:269 on 2025-10-07 17:53_

Does this change how the rule handles non-deferred type annotations? 

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:269 on 2025-10-17 18:29_

Unfortunately yes, it seems. For example if you shadow the class later in the file. Back to the drawing board...

(Incidentally, while investigating this I discovered a different issue with the stable behavior https://github.com/astral-sh/ruff/issues/20945)

---

_@dylwil3 reviewed on 2025-10-17 18:29_

---

_Converted to draft by @dylwil3 on 2025-10-27 15:27_

---

_Comment by @dylwil3 on 2025-12-10 18:03_

Gonna close this as stale and others can pick it up - it turned out to be fairly confusing and I haven't had time to revisit it.

---

_Closed by @dylwil3 on 2025-12-10 18:03_

---
