```yaml
number: 22681
title: "[`refurb`] Make fix unsafe if it deletes comments (`FURB116`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: FURB116
created_at: 2026-01-18T13:44:58Z
updated_at: 2026-01-19T17:00:50Z
url: https://github.com/astral-sh/ruff/pull/22681
synced_at: 2026-01-19T17:26:16Z
```

# [`refurb`] Make fix unsafe if it deletes comments (`FURB116`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-18 13:56_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:148 on 2026-01-19 15:03_

nit: I would probably just nest the check here instead of repeating the `matches!`:


```suggestion
    let applicability = if matches!(maybe_number, Expr::NumberLiteral(_)) {
        if checker.comment_ranges().intersects(subscript.range()) {
			Applicability::Unsafe
		} else {
			Applicability::Safe
		}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:34 on 2026-01-19 15:05_

```suggestion
/// The fix is only marked as safe for integer literals, all other cases
/// are display-only, as they may change the runtime behavior of the program
/// or introduce syntax errors. The fix for integer literals is also marked as unsafe
/// if the expression contains comments that would be removed by the fix.
```

---

_@ntBre reviewed on 2026-01-19 15:05_

Thanks! Just a couple of nits

---

_Label `fixes` added by @ntBre on 2026-01-19 15:06_

---

_@ntBre approved on 2026-01-19 16:55_

Thank you!

---

_Merged by @ntBre on 2026-01-19 16:56_

---

_Closed by @ntBre on 2026-01-19 16:56_

---

_Branch deleted on 2026-01-19 17:00_

---
