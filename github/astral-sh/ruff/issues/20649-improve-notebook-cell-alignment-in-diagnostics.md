---
number: 20649
title: Improve notebook cell alignment in diagnostics
type: issue
state: open
author: ntBre
labels:
  - needs-design
  - diagnostics
assignees: []
created_at: 2025-09-30T15:58:43Z
updated_at: 2025-09-30T15:58:43Z
url: https://github.com/astral-sh/ruff/issues/20649
synced_at: 2026-01-07T13:12:16-06:00
---

# Improve notebook cell alignment in diagnostics

---

_Issue opened by @ntBre on 2025-09-30 15:58_

This is somewhat related to https://github.com/astral-sh/ruff/issues/20648, but when rendering diagnostics for notebooks, there is sometimes an offset between different cells' line number separators, if the cells have different numbers of lines:

https://github.com/astral-sh/ruff/blob/bedfc6fd063e08f4d1e76ae8a780162404079298/crates/ruff/tests/format.rs#L683-L709

This example shows two discontinuities, one from the [manual width calculation](https://github.com/astral-sh/ruff/blob/bedfc6fd063e08f4d1e76ae8a780162404079298/crates/ruff/src/commands/format.rs#L767-L778) in the formatter (the --> arrow in the header is misaligned relative to the first cell), which uses the maximum cell line number, and a second from the diff code shared with the linter (the header and line number separators for cell 4 are offset relative to the earlier cells because it has 10 lines).

The tricky part here is that we're calculating the width per-cell by inspecting the actual `TextDiff` from the `similar` crate:

https://github.com/astral-sh/ruff/blob/bedfc6fd063e08f4d1e76ae8a780162404079298/crates/ruff_db/src/diagnostic/render/full.rs#L162-L165

I think the most promising approach here would be to introduce an intermediate representation for the diff lines, much like `DisplayLine` in `annotate-snippets` and thus the relation to #20648. This would allow us to get a precise line number from the actual `similar::TextDiff`, but without storing all of the diffs at once. A more naive approach would just be something like:

```rust
let diffs: Vec<_> = cells.into_iter().map(|cell| TextDiff::from_lines(...)).collect();
let lineno_width = diffs.iter().max_by(...);

for diff in diffs {
    // existing rendering code
}
```

This might also be sufficient, but I think adding something like `DiffLine` would help with #20648 eventually too. A third approach would be to reuse the manual width calculation from the formatter, but we'd also like to move away from that, especially as it relies on some internal details of the `similar` crate.

---

_Label `needs-design` added by @ntBre on 2025-09-30 15:58_

---

_Label `diagnostics` added by @ntBre on 2025-09-30 15:58_

---
