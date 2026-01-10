```yaml
number: 21322
title: "[ty] Respect notebook cell boundaries when adding an auto import"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/notebook-auto-import
created_at: 2025-11-07T18:02:20Z
updated_at: 2025-11-13T17:58:40Z
url: https://github.com/astral-sh/ruff/pull/21322
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Respect notebook cell boundaries when adding an auto import

---

_Pull request opened by @MichaReiser on 2025-11-07 18:02_

## Summary

The text edits for auto-completion are constrained to within the same document (because there's only a range but no URI field). Given that the LSP represents each cell as a separate document, this means that auto import edits must be limited to the same cell (we can't change an import from another cell or add an import to the first cell). 

This PR does just that.

## Test plan

Added e2e tests

---

_Review requested from @carljm by @MichaReiser on 2025-11-07 18:02_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-07 18:02_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-07 18:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-07 18:02_

---

_Converted to draft by @MichaReiser on 2025-11-07 18:02_

---

_Label `server` added by @MichaReiser on 2025-11-07 18:02_

---

_Label `ty` added by @MichaReiser on 2025-11-07 18:02_

---

_Comment by @github-actions[bot] on 2025-11-07 18:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-11-07 18:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @MichaReiser on 2025-11-07 21:19_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-07 21:19_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-07 21:19_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-07 21:19_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-07 21:19_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-07 21:19_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-07 21:19_

---

_Comment by @github-actions[bot] on 2025-11-07 21:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@Gankra reviewed on 2025-11-08 00:35_

---

_Review comment by @Gankra on `.config/nextest.toml`:13 on 2025-11-08 00:35_

Was this intended to be checked in?

---

_@Gankra approved on 2025-11-08 00:37_

Interesting, when we were talking about it before I had convinced myself this would fall out more naturally from treating the cells as separate files, but I guess we're (reasonably) treating the notebooks as one file still.

---

_Review comment by @BurntSushi on `crates/ruff_python_importer/src/insertion.rs`:51 on 2025-11-10 14:08_

Could you add something to the docs here saying something about `within_range` and what it's for?

---

_@BurntSushi approved on 2025-11-10 14:13_

Makes sense to me!

---

_@MichaReiser reviewed on 2025-11-11 11:15_

---

_Review comment by @MichaReiser on `.config/nextest.toml`:13 on 2025-11-11 11:15_

Sort of. My hope was that it makes the lsp `e2e` tests less flaky

---

_@dhruvmanila reviewed on 2025-11-12 06:56_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/importer.rs`:156 on 2025-11-12 06:56_

Yes, this comment is correct. The `containing_range` will return the cell range that contains this offset.

---

_@dhruvmanila reviewed on 2025-11-12 07:00_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:317 on 2025-11-12 07:00_

I'm not exactly sure I follow this - are you saying that if the cell boundary is at `range.start`, then this method should return `false`?

---

_@MichaReiser reviewed on 2025-11-13 17:21_

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/cell.rs`:317 on 2025-11-13 17:21_

Yes. Otherwise, testing any range from the start of the cell always returns `true`. 

Cell 1:

```py
import c
```

Cell 2:

```py
import os
```

When checking the range between `import c`..`import os`, then `start` is before the cell boundary but it ends at the cell boundary. That means, this range contains the cell boundary.

When checking the range `import os`, then this shouldn't contain the cell boundary because it starts at the cell boundary, but it never crosses the boundary




---

_Merged by @MichaReiser on 2025-11-13 17:58_

---

_Closed by @MichaReiser on 2025-11-13 17:58_

---

_Branch deleted on 2025-11-13 17:58_

---

_Label `server` removed by @MichaReiser on 2025-11-13 17:58_

---

_Label `internal` added by @MichaReiser on 2025-11-13 17:58_

---

_Comment by @MichaReiser on 2025-11-13 17:58_

I marked this as internal because notebook support hasn't shipped yet. So this isn't a user visible change

---
