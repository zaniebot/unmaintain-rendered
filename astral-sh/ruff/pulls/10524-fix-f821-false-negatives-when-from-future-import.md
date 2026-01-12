```yaml
number: 10524
title: "Fix F821 false negatives when `from __future__ import annotations` is active (attempt 2)"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: f821-try-2
created_at: 2024-03-22T11:28:26Z
updated_at: 2024-05-02T15:47:06Z
url: https://github.com/astral-sh/ruff/pull/10524
synced_at: 2026-01-12T15:55:32Z
```

# Fix F821 false negatives when `from __future__ import annotations` is active (attempt 2)

---

_@AlexWaygood_

A second attempt to fix https://github.com/astral-sh/ruff/issues/10340. The earlier attempt was https://github.com/astral-sh/ruff/pull/10362, which was reverted in https://github.com/astral-sh/ruff/pull/10513.

https://github.com/astral-sh/ruff/pull/10362 incorrectly only allowed forward references in annotations if we were in a "typing-only annotation". But forward references are allowed in all type annotations, not just typing-only annotations, iff `from __future__ import annotations` is at the top of the file.

## Test plan

`cargo test`. The tests added in https://github.com/astral-sh/ruff/pull/10513 should prevent a repeat of the earlier regression that https://github.com/astral-sh/ruff/pull/10362 caused.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-03-22 11:28_

---

_Comment by @github-actions[bot] on 2024-03-22 11:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-22 18:09_

---

_Merged by @AlexWaygood on 2024-03-22 18:11_

---

_Closed by @AlexWaygood on 2024-03-22 18:11_

---

_Branch deleted on 2024-03-22 18:11_

---

_Review comment by @ngnpope on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_27.py`:48 on 2024-05-02 10:32_

This isn't quite testing what was intended and ought to be:

```suggestion
class Tree2(list["Tree2 | Leaf"]): ...  # always okay
```

---

_@ngnpope reviewed on 2024-05-02 10:33_

---

_@AlexWaygood reviewed on 2024-05-02 15:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_27.py`:48 on 2024-05-02 15:47_

Good catch. Luckily the corrected test still passes, however.

---
