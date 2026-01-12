```yaml
number: 21480
title: "[`parser`] Fix panic when parsing IPython escape command expressions"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: fix-21465
created_at: 2025-11-16T17:40:27Z
updated_at: 2025-11-24T06:01:32Z
url: https://github.com/astral-sh/ruff/pull/21480
synced_at: 2026-01-12T15:57:25Z
```

# [`parser`] Fix panic when parsing IPython escape command expressions

---

_@danparizher_

## Summary

Fixes a panic when parsing IPython escape commands with `Help` kind (`?`) in expression contexts. The parser now reports an error instead of panicking.

Fixes #21465.

## Problem

The parser panicked with `unreachable!()` in `parse_ipython_escape_command_expression` when encountering escape commands with `Help` kind (`?`) in expression contexts, where only `Magic` (`%`) and `Shell` (`!`) are allowed.

## Approach

Replaced the `unreachable!()` panic with error handling that adds a `ParseErrorType::OtherError` and continues parsing, returning a valid AST node with the error attached.

## Test Plan

Added `test_ipython_escape_command_in_with_statement` and `test_ipython_help_escape_command_as_expression` to verify the fix.

---

_Review requested from @MichaReiser by @danparizher on 2025-11-16 17:40_

---

_Review requested from @dhruvmanila by @danparizher on 2025-11-16 17:40_

---

_Label `bug` added by @AlexWaygood on 2025-11-16 17:41_

---

_Label `parser` added by @AlexWaygood on 2025-11-16 17:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-16 17:52_


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

_@MichaReiser reviewed on 2025-11-16 18:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2781 on 2025-11-16 18:04_

Can you explain to me why the lexer emits the `Magic` and `Shell` tokens contrary to what the comment says? Should we fix the lexer instead?

---

_@danparizher reviewed on 2025-11-16 18:53_

---

_Review comment by @danparizher on `crates/ruff_python_parser/src/parser/expression.rs`:2781 on 2025-11-16 18:53_

That makes more sense, yeah. I'm pushing a change to have it fixed at the lexer level by adding an `in_expression_context` parameter to `lex_ipython_escape_command` and preventing Help kind conversion when `true` (e.g., `%foo?` stays Magic in expression contexts).

I think it makes sense to keep the parser check as a defensive measure for edge cases like `with a,?b`, where Help tokens can still be lexed at the start of logical lines. Does this approach look right?

---

_Comment by @dhruvmanila on 2025-11-17 06:01_

Thanks for putting up this PR!

I think the problem is in the way the parser is recovering. Considering your example:
```py
with a, ?b
?
```

As there's a newline before the final `?`, the lexer emits the `IpyEscapeCommand` token. Now, the parser is trying to parse a list of expressions that comprises of `with` items but (1) it tries to recover from an unexpected `?` before the `b` and then it doesn't consider the newline token (`\n`) as the end of the with items given that this is unparenthesized. I think a (better?) fix would be to try to make the parser recover from this newline token and consider that the end of with items so that the second `?` is parsed as statement which will then avoid this panic.

What do you think? This is what I'm referring to:

```diff
diff --git a/crates/ruff_python_parser/src/parser/mod.rs b/crates/ruff_python_parser/src/parser/mod.rs
index 5f35b84b04..59c1857152 100644
--- a/crates/ruff_python_parser/src/parser/mod.rs
+++ b/crates/ruff_python_parser/src/parser/mod.rs
@@ -1115,7 +1115,12 @@ impl RecoveryContextKind {
                     TokenKind::Colon => Some(ListTerminatorKind::ErrorRecovery),
                     _ => None,
                 },
-                WithItemKind::Unparenthesized | WithItemKind::ParenthesizedExpression => p
+                WithItemKind::Unparenthesized => matches!(
+                    p.current_token_kind(),
+                    TokenKind::Colon | TokenKind::Newline
+                )
+                .then_some(ListTerminatorKind::Regular),
+                WithItemKind::ParenthesizedExpression => p
                     .at(TokenKind::Colon)
                     .then_some(ListTerminatorKind::Regular),
             },
```

Edit: Oh, I think it should maybe be restricted to `WithItemKind::Unparenthesized`. I've updated the diff snippet.

---

_Comment by @dhruvmanila on 2025-11-17 06:15_

> What do you think? This is what I'm referring to:

Ok, I tried this and it seems to be working well and I think this is more correct approach. I've tried it on various code snippets to use parentheses in various places:

```py
with (a, ?b)
?
```

```py
with (a, ?b
?
```

```py
with a, ?b
?
```

---

_@MichaReiser reviewed on 2025-11-20 09:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/tests.rs`:199 on 2025-11-20 09:17_

Can you convert those tests to inline tests instead

https://github.com/astral-sh/ruff/blob/c4240076459bc9128fee6f83cbf0f51d134b663b/crates/ruff_python_parser/CONTRIBUTING.md#L26-L28

---

_@MichaReiser reviewed on 2025-11-20 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/tests.rs`:167 on 2025-11-20 09:18_

Can you add some case arms or statements coming after `with` so that it also demonstrates that the parser recovers properly?

---

_Review requested from @MichaReiser by @danparizher on 2025-11-21 02:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:2784 on 2025-11-21 04:41_

I think this needs to be reverted now that the parser correctly recovers from the syntax error

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:2777 on 2025-11-21 04:42_

This needs to be reverted as well

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:2807 on 2025-11-21 04:42_

And this, you can move the test cases above the recovery branch in `parser/mod.rs`

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2212 on 2025-11-21 04:43_

We should add some test cases that contains parentheses like the ones that I've mentioned in my comment

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1245 on 2025-11-21 04:43_

This should be reverted as well

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1315 on 2025-11-21 04:43_

And this

---

_@dhruvmanila requested changes on 2025-11-21 04:45_

Can you revert the original solution now that the parser recovers correctly? I'd also recommend to add some more test cases especially the ones that contains parentheses.

---

_Review requested from @dhruvmanila by @danparizher on 2025-11-23 00:09_

---

_@dhruvmanila approved on 2025-11-24 05:03_

Thank you!

---

_Comment by @dhruvmanila on 2025-11-24 05:06_

Hmm, actually the test cases now don't really test the bug which, I think, requires the `?` on the next line as mentioned here (https://github.com/astral-sh/ruff/pull/21480#issuecomment-3540148662). I'll update the test cases and merge.

---

_Merged by @dhruvmanila on 2025-11-24 05:40_

---

_Closed by @dhruvmanila on 2025-11-24 05:40_

---

_Branch deleted on 2025-11-24 06:01_

---
