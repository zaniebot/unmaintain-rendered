```yaml
number: 18838
title: "[`pyupgrade`] Add fix safety section to `UP010`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-06-20T22:24:18Z
updated_at: 2025-06-23T16:56:41Z
url: https://github.com/astral-sh/ruff/pull/18838
synced_at: 2026-01-12T15:56:26Z
```

# [`pyupgrade`] Add fix safety section to `UP010`

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #15584

This adds a `Fix safety` section to [unnecessary-future-import (UP010)](https://docs.astral.sh/ruff/rules/unnecessary-future-import/#unnecessary-future-import-up010)

The unsafety is determined here:
https://github.com/astral-sh/ruff/blob/d9266284df170cd70908761b5d5ae45b25523a40/crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs#L128-L132

Unsafe code example:
[playground](https://play.ruff.rs/c07d8c41-9ab8-4b86-805b-8cf482d450d9)
```py
from __future__ import (print_function,# ...
__annotations__)  # ...
```

Edit: It looks like there was already a PR for this, #17490, but I missed it since they said `UP029` instead of `UP010` :/

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected

---

_Comment by @github-actions[bot] on 2025-06-20 22:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-21 15:39_

---

_@dylwil3 approved on 2025-06-23 13:20_

Perfect, thank you!

---

_Merged by @dylwil3 on 2025-06-23 13:20_

---

_Closed by @dylwil3 on 2025-06-23 13:20_

---

_Branch deleted on 2025-06-23 16:56_

---
