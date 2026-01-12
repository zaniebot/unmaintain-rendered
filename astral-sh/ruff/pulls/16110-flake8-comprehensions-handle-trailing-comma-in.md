```yaml
number: 16110
title: "[`flake8-comprehensions`]: Handle trailing comma in `C403` fix"
type: pull_request
state: merged
author: ayushbaweja
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-c403-trailing-comma
created_at: 2025-02-12T01:04:05Z
updated_at: 2025-02-16T01:33:22Z
url: https://github.com/astral-sh/ruff/pull/16110
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-comprehensions`]: Handle trailing comma in `C403` fix

---

_@ayushbaweja_

## Summary

Resolves [#16099 ](https://github.com/astral-sh/ruff/issues/16099) based on [#15929 ](https://github.com/astral-sh/ruff/pull/15929)

## Test Plan

Added test case `s = set([x for x in range(3)],)` and updated snapshot.


---

_Comment by @github-actions[bot] on 2025-02-12 01:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-02-12 02:08_

---

_Label `fixes` added by @ntBre on 2025-02-12 02:08_

---

_Renamed from "fix(flake8-comprehensions): Handle trailing comma in C403 fix" to "[flake8-comprehensions]: Handle trailing comma in C403 fix" by @ayushbaweja on 2025-02-12 04:23_

---

_@dhruvmanila reviewed on 2025-02-12 04:36_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:82 on 2025-02-12 04:36_

I think we should use `checker.tokens().in_range(...)` here as we already have access to the parsed tokens. I think the range would be `TextRange::new(argument.end(), call.end())`. You can refer to other references of `in_range` method to check how it's being used.

---

_@dylwil3 reviewed on 2025-02-12 16:24_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:82 on 2025-02-12 16:24_

Good call @dhruvmanila ! Do you (@ayushbaweja) want to also make the same changes to `unnecessary_generator_list.rs` and `unnecessary_generator_set.rs`? Here's the patch for the first of those, for example:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs
index cc389e1f4..2e3b7fbc9 100644
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_generator_list.rs
@@ -4,7 +4,7 @@ use ruff_python_ast as ast;
 use ruff_python_ast::comparable::ComparableExpr;
 use ruff_python_ast::parenthesize::parenthesized_range;
 use ruff_python_ast::ExprGenerator;
-use ruff_python_trivia::{SimpleTokenKind, SimpleTokenizer};
+use ruff_python_parser::TokenKind;
 use ruff_text_size::{Ranged, TextRange, TextSize};
 
 use crate::checkers::ast::Checker;
@@ -125,10 +125,12 @@ pub(crate) fn unnecessary_generator_list(checker: &Checker, call: &ast::ExprCall
 
         // Replace `)` with `]`.
         // Place `]` at argument's end or at trailing comma if present
-        let mut tokenizer =
-            SimpleTokenizer::new(checker.source(), TextRange::new(argument.end(), call.end()));
-        let right_bracket_loc = tokenizer
-            .find(|token| token.kind == SimpleTokenKind::Comma)
+        let after_arg_tokens = checker
+            .tokens()
+            .in_range(TextRange::new(argument.end(), call.end()));
+        let right_bracket_loc = after_arg_tokens
+            .iter()
+            .find(|token| token.kind() == TokenKind::Comma)
             .map_or(call.arguments.end(), |comma| comma.end())
             - TextSize::from(1);
         let call_end = Edit::replacement("]".to_string(), right_bracket_loc, call.end());

```

---

_Review comment by @ayushbaweja on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:82 on 2025-02-12 16:47_

Sure! Will make the change and also update the unnecessary_generator files

---

_@ayushbaweja reviewed on 2025-02-12 16:47_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-15 14:09_

---

_@dylwil3 approved on 2025-02-15 17:39_

Thanks so much, this is great!

---

_Merged by @dylwil3 on 2025-02-15 17:45_

---

_Closed by @dylwil3 on 2025-02-15 17:45_

---

_Renamed from "[flake8-comprehensions]: Handle trailing comma in C403 fix" to "[`flake8-comprehensions`]: Handle trailing comma in `C403` fix" by @dylwil3 on 2025-02-15 17:45_

---

_Branch deleted on 2025-02-16 01:33_

---
