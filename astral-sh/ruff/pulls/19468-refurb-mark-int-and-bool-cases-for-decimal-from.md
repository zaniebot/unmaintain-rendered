```yaml
number: 19468
title: "[`refurb`] Mark `int` and `bool` cases for `Decimal.from_float` as safe fixes in `FURB164` tests"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19460
created_at: 2025-07-21T15:57:38Z
updated_at: 2025-07-28T14:33:11Z
url: https://github.com/astral-sh/ruff/pull/19468
synced_at: 2026-01-12T15:56:40Z
```

# [`refurb`] Mark `int` and `bool` cases for `Decimal.from_float` as safe fixes in `FURB164` tests

---

_@danparizher_

## Summary

Fixes #19460


---

_@dscorbett reviewed on 2025-07-21 16:15_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:306 on 2025-07-21 16:15_

Does this work for `float("\x2dnan")` or `float("\N{HYPHEN-MINUS}nan")`? Cf. #16771.

---

_@danparizher reviewed on 2025-07-21 16:25_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:306 on 2025-07-21 16:25_

Yes, it covers both of those cases

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:07_

---

_Label `bug` added by @ntBre on 2025-07-24 17:02_

---

_Label `fixes` added by @ntBre on 2025-07-24 17:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:183 on 2025-07-24 17:04_

I think this should be equivalent, but it's probably more readable, even though I think I suggested the old version 😆 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:306 on 2025-07-24 17:15_

Could we add tests for those cases? I also think we could use the return value of `as_non_finite_float_string_literal` just above this instead of doing some of the manual manipulation ourselves. That should normalize the result so that we can just compare against "-nan" directly.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:189 on 2025-07-24 17:25_

We might want to update the name of this function and also the comment where it's called. `is_valid` made me think we would bail out without a diagnostic or without a fix if the result is invalid, but it's really just checking the fix safety.

---

_@ntBre reviewed on 2025-07-24 17:25_

Thanks! Just a couple of suggestions

---

_Comment by @github-actions[bot] on 2025-07-24 17:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @danparizher on 2025-07-24 21:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/unnecessary_from_float.rs`:291 on 2025-07-28 14:12_

```suggestion
    let replacement_text = format!(r#"{constructor_name}("{normalized}")"#);
```

I'm not sure GitHub is showing the suggestion properly because one of the lines wasn't changed, but I think we can inline the quote addition and avoid two `format!` calls. We can also use a raw string to avoid needing to escape the quotes.

---

_@ntBre approved on 2025-07-28 14:17_

Thanks! Just one more tiny nit that I'll probably just apply and then merge this.

---

_Merged by @ntBre on 2025-07-28 14:21_

---

_Closed by @ntBre on 2025-07-28 14:21_

---

_Branch deleted on 2025-07-28 14:33_

---
