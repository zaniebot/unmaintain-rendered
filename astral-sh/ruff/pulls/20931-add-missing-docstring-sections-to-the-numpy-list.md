```yaml
number: 20931
title: Add missing docstring sections to the numpy list
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - docstring
assignees: []
merged: true
base: main
head: missing-sections
created_at: 2025-10-16T21:36:37Z
updated_at: 2025-10-25T20:30:22Z
url: https://github.com/astral-sh/ruff/pull/20931
synced_at: 2026-01-10T16:59:49Z
```

# Add missing docstring sections to the numpy list

---

_Pull request opened by @augustelalande on 2025-10-16 21:36_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add docstring sections which were missing from the numpy list as pointed out here #20923. For now these are only the official sections as documented [here](https://numpydoc.readthedocs.io/en/latest/format.html#sections).

## Test Plan

Added a test case for DOC102


---

_Comment by @github-actions[bot] on 2025-10-16 21:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-10-19 10:16_

---

_Label `rule` added by @MichaReiser on 2025-10-20 07:36_

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC102_numpy.py`:377 on 2025-10-20 23:30_

Should this produce a test snapshot change?

---

_@amyreese reviewed on 2025-10-20 23:30_

---

_Review comment by @augustelalande on `crates/ruff_linter/resources/test/fixtures/pydoclint/DOC102_numpy.py`:377 on 2025-10-20 23:33_

no. it would have previously emitted a false positive, but not anymore.

---

_@augustelalande reviewed on 2025-10-20 23:33_

---

_Label `docstring` added by @ntBre on 2025-10-24 21:17_

---

_@ntBre approved on 2025-10-24 21:17_

Thank you!

---

_Renamed from "[docstring] Add missing sections to the numpy list" to "Add missing docstring sections to the numpy list" by @ntBre on 2025-10-24 21:19_

---

_Merged by @ntBre on 2025-10-24 21:19_

---

_Closed by @ntBre on 2025-10-24 21:19_

---

_Branch deleted on 2025-10-25 20:30_

---
