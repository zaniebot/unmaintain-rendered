```yaml
number: 21319
title: Reduce notebook memory footprint
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/refactor-notebook-index
created_at: 2025-11-07T16:26:20Z
updated_at: 2025-11-11T11:13:50Z
url: https://github.com/astral-sh/ruff/pull/21319
synced_at: 2026-01-12T15:57:21Z
```

# Reduce notebook memory footprint

---

_@MichaReiser_

## Summary

Reduce the memory footprint of `NotebookIndex` from `O(lines)` to `O(cells)`. 

The `NotebookIndex` stores a mapping from rows in the concatenated notebook document
to the relative row numbers within each cell and from absolute line number to the corresponding cell.

The way this is implemented today is by having a vector with one entry for every absolute row number
where the value is the index of the cell. While this allows `O(1)` lookup, it does require an entry for every single line.

This PR rewrites our representation (and uses one `Vec` instead of two) to only store the 
start line number per cell and use a binary search to find to which cell a line number belongs. 
This makes lookups slightly slower (from `O(1)` to `O(log(n))` but reduces memory usage
from `O(rows)` to `O(cells)`.

I noticed this improvement when working on ty's notebook support but decided to extract it into its own PR to make reviewing easier.

## Test Plan

Existing tests


---

_Label `internal` added by @MichaReiser on 2025-11-07 16:26_

---

_Renamed from "Reduce memory footprint of notebooks" to "Reduce notebook memory footprint" by @MichaReiser on 2025-11-07 16:31_

---

_Marked ready for review by @MichaReiser on 2025-11-07 16:31_

---

_Review requested from @carljm by @MichaReiser on 2025-11-07 16:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-07 16:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-07 16:31_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-07 16:31_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-07 16:31_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-07 16:31_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-07 16:31_

---

_Comment by @github-actions[bot] on 2025-11-07 16:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-11-10 17:50_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/index.rs`:42 on 2025-11-11 09:34_

```suggestion
    /// This yields one entry per Python cell (skipping over Markdown cell).
```

---

_Merged by @MichaReiser on 2025-11-11 09:43_

---

_Closed by @MichaReiser on 2025-11-11 09:43_

---

_Branch deleted on 2025-11-11 09:43_

---

_@dhruvmanila approved on 2025-11-11 09:49_

---

_@MichaReiser reviewed on 2025-11-11 11:13_

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/index.rs`:42 on 2025-11-11 11:13_

Thanks. I pushed the fix to https://github.com/astral-sh/ruff/pull/21175

---
