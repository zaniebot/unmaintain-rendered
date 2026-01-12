```yaml
number: 19246
title: "[`pydoclint`] Fix `SyntaxError` from fixes with line continuations (`D201`, `D202`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-D201-D202-fix-error
created_at: 2025-07-09T21:13:29Z
updated_at: 2025-07-14T19:01:41Z
url: https://github.com/astral-sh/ruff/pull/19246
synced_at: 2026-01-12T15:56:35Z
```

# [`pydoclint`] Fix `SyntaxError` from fixes with line continuations (`D201`, `D202`)

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

This PR fixes #7172 by suppressing the fixes for [docstring-missing-returns (DOC201)](https://docs.astral.sh/ruff/rules/docstring-missing-returns/#docstring-missing-returns-doc201) / [docstring-extraneous-returns (DOC202)](https://docs.astral.sh/ruff/rules/docstring-extraneous-returns/#docstring-extraneous-returns-doc202) if there is a surrounding line continuation character `\` that would make the fix cause a syntax error.

To do this, the lints are changed from `AlwaysFixableViolation` to `Violation` with `FixAvailability::Sometimes`.

In the case of `DOC201`, the fix is not given if the non-break line ends in a line continuation character `\`. Note that lines are iterated in reverse from the docstring to the function definition.

In the case of `DOC202`, the fix is not given if the docstring ends with a line continuation character `\`.

## Test Plan

<!-- How was it tested? -->

Added a test case.

---

_Comment by @github-actions[bot] on 2025-07-09 21:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-10 14:55_

Thanks, this looks reasonable to me. Could the same happen with classes? I noticed `blank_before_after_class.rs` in the same directory, and the implementation looks similar.

---

_Label `bug` added by @ntBre on 2025-07-10 14:55_

---

_Label `fixes` added by @ntBre on 2025-07-10 14:55_

---

_Comment by @ntBre on 2025-07-10 14:59_

I do wonder if something like iterating over the `LogicalLines` could be a better solution here:

https://github.com/astral-sh/ruff/blob/49d72273d3471168883643ada9e3a47fe2b7c5b5/crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/mod.rs#L61-L62

but I'm not totally sure if that's how it works. That might also skip the diagnostic entirely, which could also be reasonable.

---

_Comment by @MeGaGiGaGon on 2025-07-10 18:06_

> Thanks, this looks reasonable to me. Could the same happen with classes? I noticed `blank_before_after_class.rs` in the same directory, and the implementation looks similar.

It looks like the same can happen with the before check (`D211`) since it works the same, but not the after check since it enforces 1 blank line https://play.ruff.rs/a2fac120-588a-4a1d-9287-0663cd0242e3

---

_Comment by @ntBre on 2025-07-14 17:31_

Sorry I left this in kind of an ambiguous state. I'll merge this now and we can follow up on classes and the `LogicalLines` idea separately (if at all, especially in the `LogicalLines` case). Thanks again!

---

_Merged by @ntBre on 2025-07-14 17:31_

---

_Closed by @ntBre on 2025-07-14 17:31_

---

_Branch deleted on 2025-07-14 19:01_

---
