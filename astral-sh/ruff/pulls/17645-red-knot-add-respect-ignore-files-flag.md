```yaml
number: 17645
title: "[red-knot] Add --respect-ignore-files flag"
type: pull_request
state: merged
author: thejchap
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: feature/red-knot-respect-ignore-files
created_at: 2025-04-26T19:36:51Z
updated_at: 2025-04-27T09:55:41Z
url: https://github.com/astral-sh/ruff/pull/17645
synced_at: 2026-01-12T15:56:03Z
```

# [red-knot] Add --respect-ignore-files flag

---

_@thejchap_

## Summary
https://github.com/astral-sh/ty/issues/219
Un-revert https://github.com/astral-sh/ruff/pull/17569 (fix tests on Windows)

This PR adds a --respect-ignore-files, --no-respect-ignore-files, and [knot.respect-ignore-files] CLI/config file options that drive whether standard ignore files are respected when doing file discovery. Borrowed the CLI flag pattern (positive/negative CLI flags) from Ruff.

Multiple `TestCase`s per test break the filter setup it seems - refactored the test to just use one case

## Test Plan
Snapshot tests

---

_Comment by @github-actions[bot] on 2025-04-26 19:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @thejchap on 2025-04-27 03:54_

---

_Review requested from @carljm by @thejchap on 2025-04-27 03:54_

---

_Review requested from @MichaReiser by @thejchap on 2025-04-27 03:54_

---

_Review requested from @AlexWaygood by @thejchap on 2025-04-27 03:54_

---

_Review requested from @sharkdp by @thejchap on 2025-04-27 03:54_

---

_Review requested from @dcreager by @thejchap on 2025-04-27 03:54_

---

_Label `configuration` added by @MichaReiser on 2025-04-27 09:54_

---

_Label `red-knot` added by @MichaReiser on 2025-04-27 09:54_

---

_@MichaReiser reviewed on 2025-04-27 09:55_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:40 on 2025-04-27 09:55_

Ohh, was the problem that we created more and more `TestCases` in the same scope that each applied their own filters? Nice find!

---

_Merged by @MichaReiser on 2025-04-27 09:55_

---

_Closed by @MichaReiser on 2025-04-27 09:55_

---
