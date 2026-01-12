```yaml
number: 15764
title: "[`ruff`] Add support for more `re` patterns (`RUF055`)"
type: pull_request
state: merged
author: Garrett-R
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: garrett/14738/ruff055-more-patterns
created_at: 2025-01-27T05:57:38Z
updated_at: 2025-01-29T19:16:20Z
url: https://github.com/astral-sh/ruff/pull/15764
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff`] Add support for more `re` patterns (`RUF055`)

---

_@Garrett-R_

## Summary
Implements some of #14738, by adding support for 6 new patterns:
```py
re.search("abc", s) is None       # ⇒ "abc" not in s
re.search("abc", s) is not None   # ⇒ "abc" in s

re.match("abc", s) is None       # ⇒ not s.startswith("abc")  
re.match("abc", s) is not None   # ⇒ s.startswith("abc")

re.fullmatch("abc", s) is None       # ⇒ s != "abc"
re.fullmatch("abc", s) is not None   # ⇒ s == "abc"
```


## Test Plan

```shell
cargo nextest run
cargo insta review
```

And ran the fix on my startup's repo.


## Note

One minor limitation here:

```py
if not re.match('abc', s) is None:
    pass
```

will get fixed to this (technically correct, just not nice):
```py
if not not s.startswith('abc'):
    pass
```

This seems fine given that Ruff has this covered: the initial code should be caught by [E714](https://docs.astral.sh/ruff/rules/not-is-test/) and the fixed code should be caught by [SIM208](https://docs.astral.sh/ruff/rules/double-negation/).

---

_@Garrett-R reviewed on 2025-01-27 05:57_

---

_Review comment by @Garrett-R on `crates/ruff_linter/resources/test/fixtures/ruff/RUF055_0.py`:5 on 2025-01-27 05:57_

(found this more readable)

---

_@Garrett-R reviewed on 2025-01-27 06:02_

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:233 on 2025-01-27 06:02_

(this got renamed to `get_call_replacement`)

---

_Comment by @github-actions[bot] on 2025-01-27 06:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +2 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/697a272532a0ddd158b60e6100ea2a393c8d422b/ibis/backends/tests/test_client.py#L1080'>ibis/backends/tests/test_client.py:1080:9:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/ibis-project/ibis/blob/697a272532a0ddd158b60e6100ea2a393c8d422b/ibis/backends/tests/test_client.py#L1101'>ibis/backends/tests/test_client.py:1101:9:</a> RUF055 [*] Plain string pattern passed to `re` function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/f6bce0502bf514d73f0ba93a2877b871836703c7/astropy/coordinates/tests/test_frames.py#L352'>astropy/coordinates/tests/test_frames.py:352:11:</a> RUF055 Plain string pattern passed to `re` function
+ <a href='https://github.com/astropy/astropy/blob/f6bce0502bf514d73f0ba93a2877b871836703c7/astropy/coordinates/tests/test_frames.py#L352'>astropy/coordinates/tests/test_frames.py:352:11:</a> RUF055 [*] Plain string pattern passed to `re` function
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF055 | 4 | 2 | 0 | 2 | 0 |

</p>
</details>




---

_Marked ready for review by @Garrett-R on 2025-01-27 06:30_

---

_Review requested from @ntBre by @AlexWaygood on 2025-01-27 07:45_

---

_Label `rule` added by @AlexWaygood on 2025-01-27 07:46_

---

_Label `preview` added by @AlexWaygood on 2025-01-27 07:46_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF055_2.py`:10 on 2025-01-27 15:15_

```suggestion
# this should be replaced with `"abc" in s`
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:127 on 2025-01-27 15:43_

Can this be combined with the `None` match arm? It looks like it could to me, and then the variables wouldn't have to be `mut`. Something like this:

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
index 894935012..1b17ae897 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
@@ -116,15 +116,10 @@ pub(crate) fn unnecessary_regular_expression(checker: &mut Checker, call: &ExprC
     //
     // We first check the cases where we replace the parent expression rather than just the call.
     //    Example: `re.search("abc", s) is None`  => `"abc" not in s`
-    let (mut new_expr, mut call_range) = match re_func.get_parent_replacement(semantic) {
+    let (new_expr, call_range) = match re_func.get_parent_replacement(semantic) {
         Some((expr, range)) => (Some(expr), range),
-        None => (None, TextRange::default()),
+        None => (re_func.get_call_replacement(), call.range),
     };
-    // Second, we check the case where only the call needs replacing
-    if new_expr.is_none() {
-        new_expr = re_func.get_call_replacement();
-        call_range = call.range;
-    }

     let repl = new_expr.map(|expr| checker.generator().expr(&expr));
     let mut diagnostic = Diagnostic::new(
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:225 on 2025-01-27 15:53_

Is `get_comparison_to_none` always used in the same place as `in_if_context`? If so, I'd be tempted to move the `get_comparison_to_none` call into the `in_if_context` binding and then rename it. For example,

```rust
let in_if_context = semantic.in_boolean_test() || get_comparison_to_none(semantic);
```

Not sure on the name, though. Maybe something like `in_truthy_context` or `in_nonvalue_context`? 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:357 on 2025-01-27 17:38_

I don't feel *too* strongly about this, but I'd prefer something closer to this:

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
index 894935012..ecb970b91 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
@@ -7,7 +7,7 @@ use ruff_python_ast::{
 };
 use ruff_python_semantic::analyze::typing::find_binding_value;
 use ruff_python_semantic::{Modules, SemanticModel};
-use ruff_text_size::{Ranged, TextRange};
+use ruff_text_size::TextRange;

 use crate::checkers::ast::Checker;

@@ -378,22 +378,22 @@ fn get_comparison_to_none(semantic: &SemanticModel) -> Option<(ComparisonToNone,
     let parent_expr = semantic.current_expression_parent()?;

     let Expr::Compare(ExprCompare {
-        ops, comparators, ..
+        ops,
+        comparators,
+        range,
+        ..
     }) = parent_expr
     else {
         return None;
     };

-    if ops.len() != 1 {
+    let Some(Expr::NoneLiteral(_)) = comparators.first() else {
         return None;
+    };
+
+    match ops.as_ref() {
+        [CmpOp::Is] => Some((ComparisonToNone::Is, *range)),
+        [CmpOp::IsNot] => Some((ComparisonToNone::IsNot, *range)),
+        _ => None,
     }
-    let right = comparators.first()?;
-    if right.is_none_literal_expr() {
-        return match ops[0] {
-            CmpOp::Is => Some((ComparisonToNone::Is, parent_expr.range())),
-            CmpOp::IsNot => Some((ComparisonToNone::IsNot, parent_expr.range())),
-            _ => None,
-        };
-    }
-    None
```

I think we should definitely use the `range` from the `parent_expr` destructuring above, but I also like how this flattens out the final `match` and return.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:303 on 2025-01-27 18:45_

I'm not sure about these changes. I think I'd prefer just moving this to a free function with the signature `fn compare_expr(left: Expr, op: CmpOp, right: Expr) -> Expr` and calling it explicitly with `left` and `right` each time instead of this implicit ordering behavior.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:321 on 2025-01-27 19:24_

Similarly, I'd probably just inline the negation in the one place this is called with `negate` as `true`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:127 on 2025-01-27 19:34_

Actually on second thought, I think we can fuse `get_call_replacement` and `get_parent_replacement` back together by storing the `range` and `comparison_to_none` on the `ReFunc` itself. This has the added benefit of not having to call `get_comparison_to_none` twice. I was a bit on the fence about `match (&self.kind, self.comparison_to_none)` compared to first matching on `self.kind` and then on `self.comparison_to_none`, but I like having this logic back together in any case. What do you think?

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
index 28465fbb6..8df60ce5d 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs
@@ -113,25 +113,19 @@ pub(crate) fn unnecessary_regular_expression(checker: &mut Checker, call: &ExprC
 
     // Now we know the pattern is a string literal with no metacharacters, so
     // we can proceed with the str method replacement.
-    //
-    // We first check the cases where we replace the parent expression rather than just the call.
-    //    Example: `re.search("abc", s) is None`  => `"abc" not in s`
-    let (new_expr, call_range) = match re_func.get_parent_replacement(semantic) {
-        Some((expr, range)) => (Some(expr), range),
-        None => (re_func.get_call_replacement(), call.range),
-    };
+    let new_expr = re_func.replacement();
 
     let repl = new_expr.map(|expr| checker.generator().expr(&expr));
     let mut diagnostic = Diagnostic::new(
         UnnecessaryRegularExpression {
             replacement: repl.clone(),
         },
-        call_range,
+        re_func.range,
     );
 
     if let Some(repl) = repl {
         diagnostic.set_fix(Fix::applicable_edit(
-            Edit::range_replacement(repl, call_range),
+            Edit::range_replacement(repl, re_func.range),
             if checker
                 .comment_ranges()
                 .has_comments(call, checker.source())
@@ -162,6 +156,8 @@ struct ReFunc<'a> {
     kind: ReFuncKind<'a>,
     pattern: &'a Expr,
     string: &'a Expr,
+    comparison_to_none: Option<ComparisonToNone>,
+    range: TextRange,
 }
 
 impl<'a> ReFunc<'a> {
@@ -172,8 +168,13 @@ impl<'a> ReFunc<'a> {
     ) -> Option<Self> {
         // the proposed fixes for match, search, and fullmatch rely on the
         // return value only being used for its truth value
-        let in_truthy_context =
-            semantic.in_boolean_test() || get_comparison_to_none(semantic).is_some();
+        let comparison_to_none = get_comparison_to_none(semantic);
+        let in_truthy_context = semantic.in_boolean_test() || comparison_to_none.is_some();
+
+        let (comparison_to_none, range) = match comparison_to_none {
+            Some((cmp, range)) => (Some(cmp), range),
+            None => (None, call.range),
+        };
 
         match (func_name, call.arguments.len()) {
             // `split` is the safest of these to fix, as long as metacharacters
@@ -182,6 +183,8 @@ impl<'a> ReFunc<'a> {
                 kind: ReFuncKind::Split,
                 pattern: call.arguments.find_argument_value("pattern", 0)?,
                 string: call.arguments.find_argument_value("string", 1)?,
+                comparison_to_none,
+                range,
             }),
             // `sub` is only safe to fix if `repl` is a string. `re.sub` also
             // allows it to be a function, which will *not* work in the str
@@ -216,22 +219,30 @@ impl<'a> ReFunc<'a> {
                     },
                     pattern: call.arguments.find_argument_value("pattern", 0)?,
                     string: call.arguments.find_argument_value("string", 2)?,
+                    comparison_to_none,
+                    range,
                 })
             }
             ("match", 2) if in_truthy_context => Some(ReFunc {
                 kind: ReFuncKind::Match,
                 pattern: call.arguments.find_argument_value("pattern", 0)?,
                 string: call.arguments.find_argument_value("string", 1)?,
+                comparison_to_none,
+                range,
             }),
             ("search", 2) if in_truthy_context => Some(ReFunc {
                 kind: ReFuncKind::Search,
                 pattern: call.arguments.find_argument_value("pattern", 0)?,
                 string: call.arguments.find_argument_value("string", 1)?,
+                comparison_to_none,
+                range,
             }),
             ("fullmatch", 2) if in_truthy_context => Some(ReFunc {
                 kind: ReFuncKind::Fullmatch,
                 pattern: call.arguments.find_argument_value("pattern", 0)?,
                 string: call.arguments.find_argument_value("string", 1)?,
+                comparison_to_none,
+                range,
             }),
             _ => None,
         }
@@ -239,50 +250,48 @@ impl<'a> ReFunc<'a> {
 
     /// Get replacement for the parent expression.
     ///     Example: `re.search("abc", s) is None` => `"abc" not in s`
-    fn get_parent_replacement(&self, semantic: &'a SemanticModel) -> Option<(Expr, TextRange)> {
-        let (comparison, range) = get_comparison_to_none(semantic)?;
-        match self.kind {
-            // pattern in string / pattern not in string
-            ReFuncKind::Search => match comparison {
-                ComparisonToNone::Is => Some((self.compare_expr(CmpOp::NotIn), range)),
-                ComparisonToNone::IsNot => Some((self.compare_expr(CmpOp::In), range)),
-            },
-            // string.startswith(pattern) / not string.startswith(pattern)
-            ReFuncKind::Match => match comparison {
-                ComparisonToNone::Is => Some((
-                    self.method_expr("startswith", vec![self.pattern.clone()], true),
-                    range,
-                )),
-                ComparisonToNone::IsNot => Some((
-                    self.method_expr("startswith", vec![self.pattern.clone()], false),
-                    range,
-                )),
-            },
-            // string == pattern / string != pattern
-            ReFuncKind::Fullmatch => match comparison {
-                ComparisonToNone::Is => Some((self.compare_expr(CmpOp::NotEq), range)),
-                ComparisonToNone::IsNot => Some((self.compare_expr(CmpOp::Eq), range)),
-            },
-            _ => None,
-        }
-    }
-
-    fn get_call_replacement(&self) -> Option<Expr> {
-        match self.kind {
+    fn replacement(&self) -> Option<Expr> {
+        match (&self.kind, self.comparison_to_none) {
             // string.replace(pattern, repl)
-            ReFuncKind::Sub { repl } => repl
+            (ReFuncKind::Sub { repl }, _) => repl
                 .cloned()
                 .map(|repl| self.method_expr("replace", vec![self.pattern.clone(), repl], false)),
+            // string.split(pattern)
+            (ReFuncKind::Split, _) => {
+                Some(self.method_expr("split", vec![self.pattern.clone()], false))
+            }
+            // string.startswith(pattern)
+            (ReFuncKind::Match, None) => {
+                Some(self.method_expr("startswith", vec![self.pattern.clone()], false))
+            }
+            // not string.startswith(pattern)
+            (ReFuncKind::Match, Some(ComparisonToNone::Is)) => {
+                Some(self.method_expr("startswith", vec![self.pattern.clone()], true))
+            }
             // string.startswith(pattern)
-            ReFuncKind::Match => {
+            (ReFuncKind::Match, Some(ComparisonToNone::IsNot)) => {
                 Some(self.method_expr("startswith", vec![self.pattern.clone()], false))
             }
             // pattern in string
-            ReFuncKind::Search => Some(self.compare_expr(CmpOp::In)),
+            (ReFuncKind::Search, None) => Some(self.compare_expr(CmpOp::In)),
+            // pattern not in string
+            (ReFuncKind::Search, Some(ComparisonToNone::Is)) => {
+                Some(self.compare_expr(CmpOp::NotIn))
+            }
+            // pattern in string
+            (ReFuncKind::Search, Some(ComparisonToNone::IsNot)) => {
+                Some(self.compare_expr(CmpOp::In))
+            }
             // string == pattern
-            ReFuncKind::Fullmatch => Some(self.compare_expr(CmpOp::Eq)),
-            // string.split(pattern)
-            ReFuncKind::Split => Some(self.method_expr("split", vec![self.pattern.clone()], false)),
+            (ReFuncKind::Fullmatch, None) => Some(self.compare_expr(CmpOp::Eq)),
+            // string != pattern
+            (ReFuncKind::Fullmatch, Some(ComparisonToNone::Is)) => {
+                Some(self.compare_expr(CmpOp::NotEq))
+            }
+            // string == pattern
+            (ReFuncKind::Fullmatch, Some(ComparisonToNone::IsNot)) => {
+                Some(self.compare_expr(CmpOp::Eq))
+            }
         }
     }
 
@@ -357,6 +366,7 @@ fn resolve_string_literal<'a>(
     None
 }
 
+#[derive(Clone, Copy, Debug)]
 enum ComparisonToNone {
     Is,
     IsNot,
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:142 on 2025-01-27 19:53_

This is not covered in my patch above, but I think there's a potential issue here with the `has_comments` check. It only checks the `call` for comments, but in the case of a comparison to `None`, it would really need to check the `parent_expr`. Adding a test like below gives a safe fix, when it should be unsafe since it deletes the comment.

```python
if (
    re.fullmatch(
        "a really really really really long string",
        a_long_variable_name,
    )
    # with a comment here
    is None
):
    pass
```

---

_@ntBre requested changes on 2025-01-27 19:53_

Thanks for doing this! I suggested some changes, but I think the overall approach is spot-on.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:127 on 2025-01-28 06:22_

Ah yeah, good call!  That's definitely cleaner.  Applied that.


---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:225 on 2025-01-28 06:30_

Ah right, I had a feeling I might be prematurely optimizing there.  Doing this way saved having to call `get_comparison_to_none(semantic)` in situations where an above case matched, but yeah, I'm sure it makes no noticeable difference.

Yeah, concise naming is tough here.  I don't think `in_if_context` works since the code might look like:
```py
x = re.search('abc', s)
```
which has nothing to do with `if`.

> `in_truthy_context`

Yeah, I can't think of anything closer without having a very long var name.   I understand `in_nonvalue_context` because I know what we're describing, but I don't think I'd get it if reading code for first time.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:303 on 2025-01-28 06:31_

Yeah, that makes sense.  My thinking was - if this function can already know what to do, then it might as well do it's thing and simplify the signature (knowing that `s == 'abc'` looks better than `'abc' == s`), but OTOH, it's more magical / opaque, so definitely not opposed to making more transparent.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:303 on 2025-01-28 06:37_

Ok, yeah seeing it, it's definitely faster to read these new calls.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:321 on 2025-01-28 06:42_

Ah yup, good call, hadn't realized I only ended up using it once

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:357 on 2025-01-28 06:48_

Ah yes, nice trick, TIL!  Much cleaner

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:142 on 2025-01-28 07:53_

Ah whoops, thanks for catching that! 

Ok, I got a solution.  Gets the job, but it's not the cleanest.  You can see the commit here: https://github.com/astral-sh/ruff/pull/15764/commits/3d2591852fa4eea528dca6aae7db848eb395cb14

An alternative option is storing another field on `ReFunc` called `comparison_to_none_expr` with type `Option<&'a Expr>`.  I'm thinking that's perhaps cleaner.  Do you have an opinion there?

(EDIT: good pattern was suggest and applied -- resolving this one)

---

_@Garrett-R reviewed on 2025-01-28 07:55_

Thanks for the thorough review!  I've applied all suggestions in this commit:  https://github.com/astral-sh/ruff/pull/15764/commits/92e84ee8dfdba0e1eb60baeca34c6a67f0b3ab58

Then I came up with solution to the unsafe fix issue you found in this commit:  https://github.com/astral-sh/ruff/pull/15764/commits/3d2591852fa4eea528dca6aae7db848eb395cb14  

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:184 on 2025-01-28 13:56_

I think this was from my suggestion, but I always forget that you can just use the `call.range` field instead of calling `call.range()` here. Then we can also remove the `ruff_text_size::Ranged` import.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:285 on 2025-01-28 14:02_

One last thing I thought of last night: the `None` and `Some(IsNot)` cases are actually the same, and I think this holds for each of the `ReFuncKind`s with `None` comparison logic. I wasn't sure we'd want to handle this with a placeholder `_` since I thought it would be less readable, but you can actually nest the pattern like this:

```rust
            // pattern in string
            (ReFuncKind::Search, None | Some(ComparisonToNone::IsNot)) => {
                Some(ReFunc::compare_expr(self.pattern, CmpOp::In, self.string))
            }
```

which I think *is* nicer than the separate arms.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:137 on 2025-01-28 14:52_

I think it should be safe to replace the old code from `has_comments(...)` to `intersects(re_func.range)`. Then we can just reuse the range we already have for both cases. The only potentially tricky point is that `has_comments` does some additional checking for "leading" and "trailing" content. So we may also want to add a test like this:

```python
if (  # leading
    re.fullmatch(
        "a really really really really long string",
        s,
    )
    is None  # trailing
):
    pass
```

Your current code marks this unsafe, which follows the original code, while my suggestion marks it safe, which I think is actually okay since neither of the comments is deleted by the fix.

Actually the original code marks more basic examples like `re.split("abc", s) # trailing` as unsafe too, so `intersects` might have been the way to go from the beginning.

---

_@ntBre reviewed on 2025-01-28 14:53_

Thanks, this looks great! Just a couple more much smaller changes, and I think this is good to merge.

---

_@ntBre reviewed on 2025-01-28 14:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:137 on 2025-01-28 14:56_

To be more concrete on the code suggestion:

```rust
    if let Some(repl) = repl {
        diagnostic.set_fix(Fix::applicable_edit(
            Edit::range_replacement(repl, call.range),
            if checker.comment_ranges().intersects(re_func.range) // new
            {
                Applicability::Unsafe
            } else {
                Applicability::Safe
            },
        ));
    }
```

---

_@Garrett-R reviewed on 2025-01-29 02:22_

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:184 on 2025-01-29 02:22_

Ah good to know!  Applied.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:285 on 2025-01-29 02:27_

Ah cool, didn't think of that, but yeah, definitely nicer!   Consolidated to this in the 3 relevant places.

---

_Review comment by @Garrett-R on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:137 on 2025-01-29 02:57_

Ah cool!  Didn't realize that was an option, updated to that and added that new test case.  You can see commit here: https://github.com/astral-sh/ruff/pull/15764/commits/b5ebcc24c89f07a58eb6420b3e4fd78796d1665e

Lmk if I should squash the commits into 1.  They can stand on their own, but not sure if Ruff prefers keeping git history more concise.

---

_@Garrett-R reviewed on 2025-01-29 03:09_

Thanks, good ideas!  Implemented all those :sunglasses: 

---

_@ntBre approved on 2025-01-29 15:09_

This looks great. Thanks again for your work on this!

---

_@ntBre reviewed on 2025-01-29 15:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:137 on 2025-01-29 15:11_

Perfect, thanks! And no worries about squashing, we always squash and merge through GitHub.

---

_Merged by @ntBre on 2025-01-29 15:14_

---

_Closed by @ntBre on 2025-01-29 15:14_

---

_Comment by @ntBre on 2025-01-29 15:18_

Oh and just to acknowledge the `not not` case, I agree that it's unfortunate, but your observation that the original code should be caught by E714, which I believe is enabled by default, is good enough for me. I think we'd have to look at the grandparent expression to detect this anyway, and I think it's reasonable to draw the line at the first parent, especially in light of the other rules.

---

_Comment by @Garrett-R on 2025-01-29 19:16_

For sure, thanks for the super helpful review, learned a bunch!  :sunglasses: 

---

_Branch deleted on 2025-01-29 19:16_

---
