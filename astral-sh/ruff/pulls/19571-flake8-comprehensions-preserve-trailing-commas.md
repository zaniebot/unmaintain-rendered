```yaml
number: 19571
title: "[`flake8-comprehensions`] Preserve trailing commas for single-element lists (`C409`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19568
created_at: 2025-07-26T18:16:57Z
updated_at: 2025-09-19T13:36:57Z
url: https://github.com/astral-sh/ruff/pull/19571
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-comprehensions`] Preserve trailing commas for single-element lists (`C409`)

---

_Pull request opened by @danparizher on 2025-07-26 18:16_

## Summary

Fixes #19568


---

_Comment by @github-actions[bot] on 2025-07-26 18:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-26 18:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs`:129 on 2025-07-28 20:50_

These two checks, `needs_trailing_comma` and `has_trailing_comma` aren't mutually exclusive. We may need to either add a new comma (`needs_trailing_comma`) or preserve it (`has_trailing_comma`). You can see that deleting `needs_trailing_comma` was wrong because the `t6` test case below has now lost its comma.

I'm wondering if we could reuse the token-based check for `has_trailing_comma` too, but the main issue first is restoring the old functionality.

---

_@ntBre requested changes on 2025-07-28 20:51_

---

_Review requested from @ntBre by @danparizher on 2025-07-30 05:28_

---

_Label `bug` added by @ntBre on 2025-08-07 19:42_

---

_Label `fixes` added by @ntBre on 2025-08-07 19:42_

---

_Renamed from "[`flake8_comprehensions`] Fix `C409` to preserve trailing commas in tuple calls" to "[`flake8-comprehensions`] Fix `C409` to preserve trailing commas in tuple calls" by @ntBre on 2025-08-07 19:42_

---

_Comment by @ntBre on 2025-08-07 20:26_

Okay I looked at this much more closely today, and I believe the only diff we needed was this:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs
index b2d3af263f..30a250e7ce 100644
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs
@@ -124,7 +124,7 @@ pub(crate) fn unnecessary_literal_within_tuple_call(
                 let needs_trailing_comma = if let [item] = elts.as_slice() {
                     SimpleTokenizer::new(
                         checker.locator().contents(),
-                        TextRange::new(item.end(), call.end()),
+                        TextRange::new(item.end(), argument.end()),
                     )
                     .all(|token| token.kind != SimpleTokenKind::Comma)
                 } else {
```

This preserves the comma in `tuple([1],)` and also doesn't add an unnecessary comma to the `t10` case (`t10 = (1, 2,)` in the current fix).

We shouldn't check the tokens all the way to the end of the call because those are not preserved.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_literal_within_tuple_call.rs`:132 on 2025-08-08 18:48_

You should be able to revert all of this with the change I suggested.

---

_@ntBre reviewed on 2025-08-08 18:48_

---

_Comment by @MichaReiser on 2025-09-18 13:22_

@danparizher do you plan to come back to this or should we close this PR?

---

_Review requested from @ntBre by @danparizher on 2025-09-19 02:59_

---

_@ntBre approved on 2025-09-19 13:24_

Thank you!

---

_Renamed from "[`flake8-comprehensions`] Fix `C409` to preserve trailing commas in tuple calls" to "[`flake8-comprehensions`] Preserve trailing commas for single-element lists (`C409`)" by @ntBre on 2025-09-19 13:26_

---

_Merged by @ntBre on 2025-09-19 13:27_

---

_Closed by @ntBre on 2025-09-19 13:27_

---

_Branch deleted on 2025-09-19 13:36_

---
