```yaml
number: 11565
title: "Consider irrefutable pattern similar to `if .. else` for `C901`"
type: pull_request
state: merged
author: blueraft
labels:
  - rule
assignees: []
merged: true
base: main
head: match-case-catch-all
created_at: 2024-05-27T10:34:32Z
updated_at: 2024-05-27T17:55:10Z
url: https://github.com/astral-sh/ruff/pull/11565
synced_at: 2026-01-10T21:56:00Z
```

# Consider irrefutable pattern similar to `if .. else` for `C901`

---

_Pull request opened by @blueraft on 2024-05-27 10:34_

## Summary

Follow up to https://github.com/astral-sh/ruff/pull/11521

Removes the extra added complexity for catch all match cases. This matches the implementation of plain `else` statements. 

## Test Plan
Added new test cases.

---

_Comment by @github-actions[bot] on 2024-05-27 10:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:100 on 2024-05-27 12:14_

Can we use `saturating_sub` here to avoid a panic for `match a:`. This is not valid python but the parser might soon be able to recover from incomplete statements.
```suggestion
                let last_index = cases.len().saturating_sub(1);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:104 on 2024-05-27 12:15_

What's the reason that we restrict the logic to only when the `_` pattern is last? Can't we always subtract the complexity in that case? 



---

_@MichaReiser reviewed on 2024-05-27 12:15_

---

_@dhruvmanila reviewed on 2024-05-27 12:27_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:104 on 2024-05-27 12:27_

Because an [irrefutable pattern](https://peps.python.org/pep-0634/#irrefutable-case-blocks) can only occur in the last case block otherwise it's a syntax error (checked by the compiler). We don't raise this currently, but we should start doing so. I'm going to make an umbrella issue with such syntax errors raised by the compiler.

---

_Comment by @dhruvmanila on 2024-05-27 12:28_

I think we should just use `cases.last`:
```rs
if let Some(last_case) = cases.last() {
	// ...
}
```

---

_@blueraft reviewed on 2024-05-27 12:35_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:100 on 2024-05-27 12:35_

Went with Dhurv's suggestion here.

---

_@dhruvmanila approved on 2024-05-27 12:42_

---

_@dhruvmanila reviewed on 2024-05-27 12:55_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:105 on 2024-05-27 12:55_

Actually, there are more cases to consider here:
1. Make sure that the `guard` field is `None` (`last_case.guard.is_none()`)
2. Consider sequence pattern where if any pattern in the sequence is `_` or named capture, the entire sequence is considered irrefutable

---

_@dhruvmanila reviewed on 2024-05-27 12:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:105 on 2024-05-27 12:56_

(Sorry for sending this review after the approval)

---

_@MichaReiser reviewed on 2024-05-27 13:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:104 on 2024-05-27 13:09_

That makes sense. But that also implies that it would be safe to ignore the check here and simply assume it is the last?

---

_@blueraft reviewed on 2024-05-27 13:10_

---

_Review comment by @blueraft on `crates/ruff_linter/src/rules/mccabe/rules/function_is_too_complex.rs`:105 on 2024-05-27 13:10_

Good catch! Done. 

---

_@dhruvmanila approved on 2024-05-27 17:30_

Thank you! I've made `is_irrefutable` a method on `Pattern`.

---

_Label `rule` added by @dhruvmanila on 2024-05-27 17:30_

---

_Renamed from "Handle match catch all case for complexity check" to "Consider irrefutable pattern similar to `if .. else` for `C901`" by @dhruvmanila on 2024-05-27 17:31_

---

_Merged by @dhruvmanila on 2024-05-27 17:33_

---

_Closed by @dhruvmanila on 2024-05-27 17:33_

---

_Branch deleted on 2024-05-27 17:55_

---
