```yaml
number: 8685
title: "[isort] Simplify code structure for ordering imports"
type: pull_request
state: merged
author: bluthej
labels:
  - internal
  - isort
assignees: []
merged: true
base: main
head: simplify-isort-sorting-logic
created_at: 2023-11-14T21:22:51Z
updated_at: 2023-11-26T16:26:28Z
url: https://github.com/astral-sh/ruff/pull/8685
synced_at: 2026-01-12T15:55:26Z
```

# [isort] Simplify code structure for ordering imports

---

_@bluthej_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

While fixing #8661 I noticed that the code structure for sorting imports could be simplified.

## Summary

- Move the logic for `force_sort_within_sections` from `isort/mod.rs` to `isort/ordering.rs` => now there is just one line in `isort/mod.rs`: `let imports = order_imports(import_block, settings);` which yields the sorted imports
- Change the function signature of `order_imports` to directly return a `Vec<EitherImport<'a>>`  => no need for `OrderedImportBlock`

I think this is a bit of an improvement because the code is simpler and there should be a bit of a speedup when setting `force-sort-within-sections` to true. Indeed, when it's set to true we're now directly ordering all the imports, whereas before we would first order the straight imports, then the from imports, combine them and finally sort the combination a second time (this is probably not noticeable in practice though).
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

No tests added, this is a simple refactor.
<!-- How was it tested? -->


---

_@charliermarsh approved on 2023-11-14 21:30_

---

_Comment by @github-actions[bot] on 2023-11-14 21:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-11-14 21:43_

---

_Closed by @charliermarsh on 2023-11-14 21:43_

---

_Label `internal` added by @charliermarsh on 2023-11-14 21:43_

---

_Label `isort` added by @charliermarsh on 2023-11-14 21:43_

---

_Comment by @charliermarsh on 2023-11-14 21:43_

Thanks!

---

_Branch deleted on 2023-11-26 16:26_

---
