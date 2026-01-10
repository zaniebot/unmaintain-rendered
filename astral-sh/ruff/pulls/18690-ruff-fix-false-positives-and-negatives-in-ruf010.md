```yaml
number: 18690
title: "[`ruff`] Fix false positives and negatives in `RUF010`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-RUF010
created_at: 2025-06-15T23:25:35Z
updated_at: 2025-06-26T20:27:10Z
url: https://github.com/astral-sh/ruff/pull/18690
synced_at: 2026-01-10T18:39:08Z
```

# [`ruff`] Fix false positives and negatives in `RUF010`

---

_Pull request opened by @LaBatata101 on 2025-06-15 23:25_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

The fix is suppressed if the f-string has debug text or call expression arguments contains a starred expression, ex:
```python
f"{ascii(1)=}"
f"{ascii(*arg)}"
```

Fixes #16325

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression tests
<!-- How was it tested? -->


---

_Comment by @LaBatata101 on 2025-06-15 23:26_

I don't get why the rule is not being trigged for the tests I added when ran with `cargo test`. But it's fine in a standalone file.

```python
f"{str({})}"

f"{ascii({} | {})}"

import builtins

f"{builtins.repr(1)}"

f"{ascii(1)=}"

f"{ascii(lambda: 1)}"

f"{ascii(x := 2)}"

```
Running with `cargo run -p ruff -- check sample.py --preview --no-cache --select RUF010 ` give me these diagnostics
```
sample2.py:1:4: RUF010 [*] Use explicit conversion flag
  |
1 | f"{str({})}"
  |    ^^^^^^^ RUF010
2 |
3 | f"{ascii({} | {})}"
  |
  = help: Replace with conversion flag

sample2.py:3:4: RUF010 [*] Use explicit conversion flag
  |
1 | f"{str({})}"
2 |
3 | f"{ascii({} | {})}"
  |    ^^^^^^^^^^^^^^ RUF010
4 |
5 | import builtins
  |
  = help: Replace with conversion flag

sample2.py:7:4: RUF010 [*] Use explicit conversion flag
  |
5 | import builtins
6 |
7 | f"{builtins.repr(1)}"
  |    ^^^^^^^^^^^^^^^^ RUF010
8 |
9 | f"{ascii(1)=}"
  |
  = help: Replace with conversion flag

sample2.py:9:4: RUF010 Use explicit conversion flag
   |
 7 | f"{builtins.repr(1)}"
 8 |
 9 | f"{ascii(1)=}"
   |    ^^^^^^^^ RUF010
10 |
11 | f"{ascii(lambda: 1)}"
   |
   = help: Replace with conversion flag

sample2.py:11:4: RUF010 [*] Use explicit conversion flag
   |
 9 | f"{ascii(1)=}"
10 |
11 | f"{ascii(lambda: 1)}"
   |    ^^^^^^^^^^^^^^^^ RUF010
12 |
13 | f"{ascii(x := 2)}"
   |
   = help: Replace with conversion flag

sample2.py:13:4: RUF010 [*] Use explicit conversion flag
   |
11 | f"{ascii(lambda: 1)}"
12 |
13 | f"{ascii(x := 2)}"
   |    ^^^^^^^^^^^^^ RUF010
   |
   = help: Replace with conversion flag

Found 6 errors.
[*] 5 fixable with the `--fix` option.
``` 

But in the tests it only flags for the `f"{str({})}"` and `f"{builtins.repr(1)}"`.

---

_Comment by @chirizxc on 2025-06-16 08:23_

> I don't get why the rule is not being trigged for the tests I added when ran with `cargo test`. But it's fine in a standalone file.

i think you forgot to add a new snapshot

---

_Marked ready for review by @LaBatata101 on 2025-06-16 14:06_

---

_Comment by @github-actions[bot] on 2025-06-16 14:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-06-17 05:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:101 on 2025-06-20 07:09_

I think we should move this right after line 72 or integrate it into the match on line 74 like so:

```rust
        let Some(builtin_symbol) = checker.semantic().resolve_builtin_symbol(&call.func) else {
	continue;
}

        let arg = match builtin_symbol {
            // Handles the cases: `f"{str(object=arg)}"` and `f"{str(arg)}"`
            "str" if call.arguments.len() == 1 => {
                let Some(arg) = call.arguments.find_argument_value("object", 0) else {
                    continue;
                };
                arg
            }
            "repr" | "ascii" => {
                // Can't be a conversion otherwise.
                if !call.arguments.keywords.is_empty() {
                    continue;
                }

                // Can't be a conversion otherwise.
                let [arg] = call.arguments.args.as_ref() else {
                    continue;
                };
                arg
            },
						_ => continue,
        };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:131 on 2025-06-20 07:11_

Can't we match on the `builtin_symbol` here (e.g. by having a `Conversion` enum that we resolve early on) rather than resolving the caller name again?

That should also remove the unreachable `anyhow::bail`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:172 on 2025-06-20 07:13_

I think this is overly strict. What we want here is to know if the left-most expression has a curly brace. See https://github.com/astral-sh/ruff/blob/45cd117ea4a361db9041304462bca7b8be7a8a80/crates/ruff_python_formatter/src/other/interpolated_string_element.rs#L276

An easier approach here could be to inspect the token stream instead and see if the first token in the expression body is a ` {`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-20 07:15_

Can you explain why it's necessary to rewrite the entire fix here? 

---

_@MichaReiser reviewed on 2025-06-20 07:15_

---

_@LaBatata101 reviewed on 2025-06-20 14:11_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-20 14:11_

With `transform_expression` I didn't have much control of how the f-string output was created. I didn't find a way to handle the special cases for curly braces, which we need to prepend a white space, and the lambda/named expressions that needed to be parenthesized.

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-20 14:46_

---

_@MichaReiser reviewed on 2025-06-23 08:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:133 on 2025-06-23 08:28_

I don't think this is correct. E.g we don't want to add a space for an expression like:

```py
1 if b({ "key": "test" }) else 10
```

---

_@MichaReiser reviewed on 2025-06-23 08:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-23 08:30_

Can't we do the same as you're doing here by:

1. Testing if the expression starts with a `{` 
2. If so, pad the start with a space

Another option would be to preserve the whitespace between `{` and the `expression`. 

---

_@LaBatata101 reviewed on 2025-06-23 14:27_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-23 14:27_

It could probably work for the `{` case, but for the case where we need to parenthesize the `lambda` and `named` expression, I think it wouldn't work. In the previous implementation of the fix, it would change the node fields to apply the fix and then use the node to generate the string. There is no way -- at least I couldn't find it -- to tell the code that is generating the string from the node to parenthesize the `lambda` /`named` expression before the conversion flag.

Basically, there's no way of telling it to do this part:
https://github.com/astral-sh/ruff/blob/9c2bce2758fb618c0eb4c50fe250b52eccee84c2/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs#L141-L142

---

_@MichaReiser reviewed on 2025-06-23 14:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-23 14:44_

Can't we do the wrapping on `output`?

```rust
.map(|output| Fix::safe_edit(Edit::range_replacement(format!(" {output}), f_string.range())))
```

I wasn't aware of the lambda changes (doesn't seem to be mentioned in the summary). Could we, once again, try to preserve the parentheses instead of adding special handling?

---

_@LaBatata101 reviewed on 2025-06-23 22:45_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:118 on 2025-06-23 22:45_

> doesn't seem to be mentioned in the summary

My bad, sorry.

Ok, so I managed to use the old fix implementation for the changes. But there is still the need for this check: 
https://github.com/astral-sh/ruff/blob/568f4f4c3608d8a302235cdc04d0afca1f8b2da8/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs#L188-L190
because we can't use `parenthesized_range` with the nodes from `libcst`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:160 on 2025-06-24 08:32_

Can we use the `conversion` that we extracted in the outer call instead of matching on `name.value`. This would allow us to remove this arm entirely

---

_@MichaReiser reviewed on 2025-06-24 08:32_

---

_@MichaReiser reviewed on 2025-06-24 08:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:142 on 2025-06-24 08:36_

I suggest moving this out of `convert_call_to_conversion_flag` as this is a known limitations that doesn't require logging when it fails.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:110 on 2025-06-24 08:37_

Nit:
```suggestion
        if arg.is_expr_starred() {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:181 on 2025-06-24 08:39_

This incorrectly returns `false` for 

```py
a = "some string"
f"{repr({a: 1} + b)}"
```

We need to look at the left most expression. 

I think the easiest is to look at the token stream and see if the first non trivia token is a `{`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:175 on 2025-06-24 08:40_

Can't we move this into the `map` arm and make it based on Ruff's input AST?

---

_@MichaReiser reviewed on 2025-06-24 08:40_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:175 on 2025-06-24 13:15_

I think we can't do that without reparsing the f-string expression, because in the `map` arm we only have the generated string of the `fstring` CST node. 

---

_@LaBatata101 reviewed on 2025-06-24 13:15_

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-24 14:18_

---

_@MichaReiser reviewed on 2025-06-24 14:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:175 on 2025-06-24 14:29_

but can't we inspect the `f_string` variable?

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:175 on 2025-06-25 00:49_

We can, but I don't see how it is going to help. In the `map` we already have the built fix string, and in this case we don't want to parenthesize the entire f-string, just a part of it.

---

_@LaBatata101 reviewed on 2025-06-25 00:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:185 on 2025-06-25 08:23_

We should add tests demonstrating this behavior. I think this is also another place where we could use `OperatorPrecedence` instead of listing all expressions where any precedence lower or equal to lambda require parenthesizing.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:175 on 2025-06-25 08:26_

That's what I had in mind:

```patch
Subject: [PATCH] Rename `knot_benchmark` to `ty_benchmark`
---
Index: crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs b/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs
--- a/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs	(revision 13921af61d95c86f1cddfbad51471c0654b72162)
+++ b/crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs	(date 1750839908748)
@@ -122,7 +122,7 @@
         }
 
         diagnostic.try_set_fix(|| {
-            convert_call_to_conversion_flag(checker, conversion, f_string, index, element)
+            convert_call_to_conversion_flag(checker, conversion, f_string, index, arg)
         });
     }
 }
@@ -133,7 +133,7 @@
     conversion: Conversion,
     f_string: &ast::FString,
     index: usize,
-    element: &InterpolatedStringElement,
+    arg: &Expr,
 ) -> Result<Fix> {
     let source_code = checker.locator().slice(f_string);
     transform_expression(source_code, checker.stylist(), |mut expression| {
@@ -145,7 +145,7 @@
 
         formatted_string_expression.conversion = Some(conversion.as_str());
 
-        if contains_brace(checker, element) {
+        if starts_with_lbrace(checker, arg) {
             formatted_string_expression.whitespace_before_expression = space();
         }
 
@@ -163,26 +163,19 @@
     .map(|output| Fix::safe_edit(Edit::range_replacement(output, f_string.range())))
 }
 
-fn contains_brace(checker: &Checker, element: &InterpolatedStringElement) -> bool {
-    let Some(interpolation) = element.as_interpolation() else {
-        return false;
-    };
-    let Some(call) = interpolation.expression.as_call_expr() else {
-        return false;
-    };
-
+fn starts_with_lbrace(checker: &Checker, arg: &Expr) -> bool {
     checker
         .tokens()
-        .after(call.arguments.start())
+        .in_range(arg.range())
         .iter()
         // Skip the trivia tokens and the `(` from the arguments
         .find(|token| !token.kind().is_trivia() && token.kind() != TokenKind::Lpar)
         .is_some_and(|token| matches!(token.kind(), TokenKind::Lbrace))
 }
 
-fn needs_paren(expr: &Expression) -> bool {
-    matches!(expr, Expression::Lambda(_) | Expression::NamedExpr(_))
-}
+// fn needs_paren(expr: &Expr) -> bool {
+//     matches!(expr, Expr::Lambda(_) | Expr::Named(_))
+// }
 
 /// Represents the three built-in Python conversion functions that can be replaced
 /// with f-string conversion flags.
```

---

_@MichaReiser reviewed on 2025-06-25 08:28_

---

_@LaBatata101 reviewed on 2025-06-25 21:20_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:185 on 2025-06-25 21:20_

> We should add tests demonstrating this behavior.

We already have it, here:
https://github.com/astral-sh/ruff/blob/13921af61d95c86f1cddfbad51471c0654b72162/crates/ruff_linter/resources/test/fixtures/ruff/RUF010.py#L50-L52

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-25 22:13_

---

_@MichaReiser reviewed on 2025-06-26 08:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:185 on 2025-06-26 08:21_

It might then be unnecessary? Because I don't see any failing tests if I change the code to:


```rust
        formatted_string_expression.expression =
        // if needs_paren(OperatorPrecedence::from_expr(arg))
        // {
        //     call.args[0]
        //         .value
        //         .clone()
        //         .with_parens(LeftParen::default(), RightParen::default())
        // } else {
            call.args[0].value.clone();
        // };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:174 on 2025-06-26 08:28_

I don't think we still need to skip over lparentheses still because we're now only iterating over the `arg` tokens?

```suggestion
    checker
        .tokens()
        .in_range(arg.range())
        .iter()
        // Skip the trivia tokens and the `(` from the arguments
        .skip_while(|token| token.kind().is_trivia())
        .next()
        .is_some_and(|token| matches!(token.kind(), TokenKind::Lbrace))
```

---

_@MichaReiser reviewed on 2025-06-26 08:28_

---

_@LaBatata101 reviewed on 2025-06-26 14:01_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:185 on 2025-06-26 14:01_

Oh, the diagnostic was not triggering for those cases in the test file because the `ascii` binding is being shadowed. I didn't notice that, thanks for pointing that out! Should be fixed now. 

---

_@LaBatata101 reviewed on 2025-06-26 14:03_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:174 on 2025-06-26 14:03_

You're right, thanks!

---

_@MichaReiser approved on 2025-06-26 15:53_

Thank you. This is great!

---

_Label `fixes` removed by @MichaReiser on 2025-06-26 15:53_

---

_Label `rule` added by @MichaReiser on 2025-06-26 15:53_

---

_Label `bug` added by @MichaReiser on 2025-06-26 15:53_

---

_Merged by @MichaReiser on 2025-06-26 15:53_

---

_Closed by @MichaReiser on 2025-06-26 15:53_

---

_Branch deleted on 2025-06-26 20:27_

---
