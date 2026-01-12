```yaml
number: 10548
title: "Move `Q004` to AST based checker"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/move-q004
created_at: 2024-03-24T17:27:30Z
updated_at: 2024-03-25T03:35:42Z
url: https://github.com/astral-sh/ruff/pull/10548
synced_at: 2026-01-12T15:55:32Z
```

# Move `Q004` to AST based checker

---

_@dhruvmanila_

## Summary

Continuing with #7595, this PR moves the `Q004` rule to the AST checker.

## Test Plan

- [x] Existing test cases should pass
- [x] No ecosystem updates


---

_Label `internal` added by @dhruvmanila on 2024-03-24 17:27_

---

_Comment by @github-actions[bot] on 2024-03-24 17:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-03-24 17:58_

---

_@AlexWaygood reviewed on 2024-03-24 18:59_

Nice! You could possibly reduce duplication between the `StringLiteral` and `BytesLiteral` branches by converting the flags into `AnyStringKind`s -- something like this? It might also be possible to reduce duplication between the `FString` branch and the other two branches -- not sure

```diff
(dhruv/move-q004)⚡ % git diff                                                                                                            ~/dev/ruff
diff --git a/crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs b/crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs
index d84dec0dc..6abef285e 100644
--- a/crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs
+++ b/crates/ruff_linter/src/rules/flake8_quotes/rules/unnecessary_escaped_quote.rs
@@ -1,9 +1,9 @@
 use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix};
 use ruff_macros::{derive_message_formats, violation};
 use ruff_python_ast::str::raw_contents;
-use ruff_python_ast::{self as ast, StringLike};
+use ruff_python_ast::{self as ast, AnyStringKind, StringLike};
 use ruff_source_file::Locator;
-use ruff_text_size::Ranged;
+use ruff_text_size::{Ranged, TextRange};
 
 use crate::checkers::ast::Checker;
 
@@ -52,15 +52,15 @@ pub(crate) fn unnecessary_escaped_quote(checker: &mut Checker, string_like: Stri
 
     match string_like {
         StringLike::String(expr) => {
-            for string in &expr.value {
-                if let Some(diagnostic) = check_string(locator, string) {
+            for ast::StringLiteral { range, flags, .. } in &expr.value {
+                if let Some(diagnostic) = check_stringlike((*flags).into(), *range, locator) {
                     checker.diagnostics.push(diagnostic);
                 }
             }
         }
         StringLike::Bytes(expr) => {
-            for bytes in &expr.value {
-                if let Some(diagnostic) = check_bytes(locator, bytes) {
+            for ast::BytesLiteral { range, flags, .. } in &expr.value {
+                if let Some(diagnostic) = check_stringlike((*flags).into(), *range, locator) {
                     checker.diagnostics.push(diagnostic);
                 }
             }
@@ -68,7 +68,9 @@ pub(crate) fn unnecessary_escaped_quote(checker: &mut Checker, string_like: Stri
         StringLike::FString(expr) => {
             for part in &expr.value {
                 if let Some(diagnostic) = match part {
-                    ast::FStringPart::Literal(string) => check_string(locator, string),
+                    ast::FStringPart::Literal(ast::StringLiteral { range, flags, .. }) => {
+                        check_stringlike((*flags).into(), *range, locator)
+                    }
                     ast::FStringPart::FString(f_string) => check_f_string(locator, f_string),
                 } {
                     checker.diagnostics.push(diagnostic);
@@ -78,54 +80,27 @@ pub(crate) fn unnecessary_escaped_quote(checker: &mut Checker, string_like: Stri
     }
 }
 
-fn check_string(locator: &Locator, string: &ast::StringLiteral) -> Option<Diagnostic> {
-    let ast::StringLiteral { range, flags, .. } = string;
-    if flags.is_triple_quoted() || flags.prefix().is_raw() {
-        return None;
-    }
-
-    let contents = raw_contents(locator.slice(string))?;
-    let quote = flags.quote_style();
-    let opposite_quote_char = quote.opposite().as_char();
-
-    if !contains_escaped_quote(contents, opposite_quote_char) {
-        return None;
-    }
-
-    let mut diagnostic = Diagnostic::new(UnnecessaryEscapedQuote, *range);
-    diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
-        format!(
-            "{prefix}{quote}{value}{quote}",
-            prefix = flags.prefix(),
-            value = unescape_string(contents, opposite_quote_char)
-        ),
-        *range,
-    )));
-    Some(diagnostic)
-}
-
-fn check_bytes(locator: &Locator, bytes: &ast::BytesLiteral) -> Option<Diagnostic> {
-    let ast::BytesLiteral { range, flags, .. } = bytes;
-    if flags.is_triple_quoted() || flags.prefix().is_raw() {
+fn check_stringlike(
+    kind: AnyStringKind,
+    range: TextRange,
+    locator: &Locator,
+) -> Option<Diagnostic> {
+    if kind.is_triple_quoted() || kind.is_raw_string() {
         return None;
     }
 
-    let contents = raw_contents(locator.slice(bytes))?;
-    let quote = flags.quote_style();
+    let contents = raw_contents(locator.slice(range))?;
+    let quote = kind.quote_style();
     let opposite_quote_char = quote.opposite().as_char();
 
     if !contains_escaped_quote(contents, opposite_quote_char) {
         return None;
     }
 
-    let mut diagnostic = Diagnostic::new(UnnecessaryEscapedQuote, *range);
+    let mut diagnostic = Diagnostic::new(UnnecessaryEscapedQuote, range);
     diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
-        format!(
-            "{prefix}{quote}{value}{quote}",
-            prefix = flags.prefix(),
-            value = unescape_string(contents, opposite_quote_char)
-        ),
-        *range,
+        kind.format_string_contents(&unescape_string(contents, opposite_quote_char)),
+        range,
     )));
     Some(diagnostic)
 }
```

---

_@AlexWaygood approved on 2024-03-24 19:08_

LGTM — my suggested simplification could be done as a follow-up 

---

_Comment by @dhruvmanila on 2024-03-25 02:28_

Thank you for the suggestion. That's a good idea. I'll just do the change for the string and bytes literal here and merge it.

---

_Comment by @codspeed-hq[bot] on 2024-03-25 03:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-q004)

### Merging #10548 will **improve performances by 5.59%**

<sub>Comparing <code>dhruv/move-q004</code> (5aaf7d3) with <code>main</code> (d625f55)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/move-q004` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 20.3 ms | 19.3 ms | +5.59% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 1.6 ms | 1.5 ms | +4.63% |


---

_Merged by @dhruvmanila on 2024-03-25 03:31_

---

_Closed by @dhruvmanila on 2024-03-25 03:31_

---

_Branch deleted on 2024-03-25 03:31_

---
