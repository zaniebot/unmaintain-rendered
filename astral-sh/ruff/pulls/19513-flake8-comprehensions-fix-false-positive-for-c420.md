```yaml
number: 19513
title: "[`flake8-comprehensions`] Fix false positive for `C420` with attribute, subscript, or slice assignment targets"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-19511
created_at: 2025-07-23T17:10:57Z
updated_at: 2025-08-08T19:05:34Z
url: https://github.com/astral-sh/ruff/pull/19513
synced_at: 2026-01-10T17:52:17Z
```

# [`flake8-comprehensions`] Fix false positive for `C420` with attribute, subscript, or slice assignment targets

---

_Pull request opened by @danparizher on 2025-07-23 17:10_

## Summary

Fixes #19511


---

_Comment by @github-actions[bot] on 2025-07-23 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:07_

---

_Label `bug` added by @ntBre on 2025-07-25 22:00_

---

_Label `rule` added by @ntBre on 2025-07-25 22:00_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C420_3.py`:15 on 2025-07-28 20:24_

These don't look like the test cases from the issue. I think these are the incorrect fixes we were generating before.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:105 on 2025-07-28 20:26_

With only one `match` arm, I think it's slightly more idiomatic to use `matches!`:


```suggestion
    if matches!(generator.target, Expr::Attribute(_) | Expr::Subscript(_) | Expr::Slice(_)) {
            return;
    }
```

---

_@ntBre requested changes on 2025-07-28 20:28_

Thanks. I think the fix is correct here, but I don't think the new test cases actually trigger the rule.

---

_Review requested from @ntBre by @danparizher on 2025-07-30 01:03_

---

_@ntBre reviewed on 2025-07-31 19:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:105 on 2025-07-31 19:52_

I didn't realize this initially, but we may need a simple `Visitor` here. This only checks the top-level expression, but the problem persists if the attribute, subscript, or slice is a sub-expression. As a simple example, we can embed the `C.a` case in a tuple:

```py
class C: a = None
{(C.a,): None for (C.a,) in "abc"}
print(C.a)
```

and it still prints `c`, but the target expression itself is no longer an attribute.

I feel like top-level slices and attributes should already be pretty rare, so I could probably be convinced that the cost of a `Visitor` isn't worth it, but we should at least document our decision and add a known-false-positive test case if we decide not to go for it.

---

_@danparizher reviewed on 2025-08-01 00:16_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:105 on 2025-08-01 00:16_

I'm curious to see how much it would cost. Going to commit to take a look at the performance, feel free to revert if you think it isn't worth it 🙂

---

_Review requested from @ntBre by @danparizher on 2025-08-01 00:28_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_comprehensions/C420.py`:1 on 2025-08-01 17:12_

Should we move these to `C420_3.py`?

---

_@ntBre approved on 2025-08-01 17:14_

Very nice! I was hoping we had a utility for this, but I didn't know about `any_over_expr` off the top of my head. Nice work!

I had one more tiny nit about test locations, but this is good to go otherwise.

---

_@ntBre reviewed on 2025-08-01 17:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:105 on 2025-08-01 17:14_

Benchmarks look good, no change!

---

_@ntBre reviewed on 2025-08-01 17:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:100 on 2025-08-01 17:16_

```suggestion
    // (attributes, subscripts, or slices).
```

One more nit, I guess :) I think "at the top level" was only true before

---

_Renamed from "[`flake8_comprehensions`] Fix false positive for `C420` with attribute, subscript, or slice assignment targets" to "[`flake8-comprehensions`] Fix false positive for `C420` with attribute, subscript, or slice assignment targets" by @danparizher on 2025-08-01 21:07_

---

_Review requested from @ntBre by @danparizher on 2025-08-01 21:15_

---

_@ntBre approved on 2025-08-08 19:02_

Thanks!

---

_Merged by @ntBre on 2025-08-08 19:02_

---

_Closed by @ntBre on 2025-08-08 19:02_

---

_Branch deleted on 2025-08-08 19:05_

---
