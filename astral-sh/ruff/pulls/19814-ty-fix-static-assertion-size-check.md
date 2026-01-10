```yaml
number: 19814
title: "[ty] Fix static assertion size check"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-member
created_at: 2025-08-07T18:22:19Z
updated_at: 2025-08-07T18:38:18Z
url: https://github.com/astral-sh/ruff/pull/19814
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix static assertion size check

---

_Pull request opened by @BurntSushi on 2025-08-07 18:22_

A `Segment` has a `Box` in it, which has a platform dependent size.
Restrict the check to only 64-bit targets.


---

_Review requested from @carljm by @BurntSushi on 2025-08-07 18:22_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-08-07 18:22_

---

_Review requested from @sharkdp by @BurntSushi on 2025-08-07 18:22_

---

_Review requested from @dcreager by @BurntSushi on 2025-08-07 18:22_

---

_Review request for @dcreager removed by @BurntSushi on 2025-08-07 18:22_

---

_Review request for @carljm removed by @BurntSushi on 2025-08-07 18:22_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-08-07 18:22_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-08-07 18:22_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-08-07 18:22_

---

_Review requested from @dylwil3 by @BurntSushi on 2025-08-07 18:22_

---

_Comment by @github-actions[bot] on 2025-08-07 18:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@dylwil3 had review dismissed on 2025-08-07 18:25_

Amazing, thank you!

---

_Comment by @github-actions[bot] on 2025-08-07 18:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dylwil3 reviewed on 2025-08-07 18:27_

Oh hmm... looks like clippy and testing wasm are upset about this.

---

_@dylwil3 approved on 2025-08-07 18:38_

---

_Merged by @dylwil3 on 2025-08-07 18:38_

---

_Closed by @dylwil3 on 2025-08-07 18:38_

---

_Branch deleted on 2025-08-07 18:38_

---
