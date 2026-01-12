```yaml
number: 14265
title: Use bitshift consistently for bitflag definitions
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: bitflags-bitshift
created_at: 2024-11-11T10:07:32Z
updated_at: 2024-11-11T10:29:52Z
url: https://github.com/astral-sh/ruff/pull/14265
synced_at: 2026-01-12T15:55:47Z
```

# Use bitshift consistently for bitflag definitions

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Most places in the code use bit shifts to set bitflag values, apart from these few.
Equivalent code, just more consistent and easier to add new flags.

Found these when defining new bitflags in another PR.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @sbrugman on 2024-11-11 10:07_

---

_Review requested from @MichaReiser by @sbrugman on 2024-11-11 10:07_

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 10:07_

---

_Review requested from @sharkdp by @sbrugman on 2024-11-11 10:07_

---

_Review comment by @sbrugman on `crates/ruff/src/printer.rs`:32 on 2024-11-11 10:08_

Is there any particular reason to not change this to `1 << 1`?

---

_@sbrugman reviewed on 2024-11-11 10:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:32 on 2024-11-11 10:09_

It should be fine. I suspect that we used to have more flags and didn't update the values when we removed one. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:1 on 2024-11-11 10:10_

I would prefer if we can keep the formatting as is to reduce unnecessary changes. The same applies for crates/ruff_linter/src/message/text.rs

---

_@MichaReiser reviewed on 2024-11-11 10:10_

---

_Label `internal` added by @MichaReiser on 2024-11-11 10:10_

---

_@MichaReiser approved on 2024-11-11 10:19_

Thanks

---

_Merged by @MichaReiser on 2024-11-11 10:20_

---

_Closed by @MichaReiser on 2024-11-11 10:20_

---

_Branch deleted on 2024-11-11 10:21_

---

_Comment by @github-actions[bot] on 2024-11-11 10:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
