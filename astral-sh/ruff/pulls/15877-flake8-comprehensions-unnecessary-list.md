```yaml
number: 15877
title: "[`flake8-comprehensions`] Unnecessary `list` comprehension (rewrite as a `set` comprehension) (`C403`) - Handle extraneous parentheses around list comprehension"
type: pull_request
state: merged
author: jbramley
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: main
created_at: 2025-02-02T14:05:38Z
updated_at: 2025-02-03T18:26:03Z
url: https://github.com/astral-sh/ruff/pull/15877
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-comprehensions`] Unnecessary `list` comprehension (rewrite as a `set` comprehension) (`C403`) - Handle extraneous parentheses around list comprehension

---

_Pull request opened by @jbramley on 2025-02-02 14:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Given the following code:

```python
set(([x for x in range(5)]))
```

the current implementation of C403 results in

```python
{(x for x in range(5))}
```

which is a set containing a generator rather than the result of the generator.

This change removes the extraneous parentheses so that the resulting code is:

```python
{x for x in range(5)}
```


## Test Plan

`cargo nextest run` and `cargo insta test`
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-02-02 14:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:87 on 2025-02-02 16:16_

It might be nice to avoid using the `generator` here to preserve comments in the comprehension like the non-parenthesized code. For example,

```python
s = set(# outer set comment
    ( # inner paren comment - not preserved
        (([ # comment in the comprehension
            x for x in range(3)]))))
```

We'll still lose the comment in the inner parentheses, but that seems like the weirdest place to put a comment of the three. It might be good to add a test like this with the comments and also one with multiple levels of parentheses like:

```python
s = set(((([x for x in range(3)]))))
```

I started to suggest an approach that would *not* have worked with multiple parentheses, but your approach handles that nicely, so it would be good to capture that in a test.

Back to avoiding the generator, you can slice the original source text (instead of round-tripping through the generator like `checker.generator().expr(argument)`) with code like `checker.source()[range].to_string()`. After that change, the new code started to look very similar to the old code, so I ended up with something like this. What do you think? 

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs
index c518a8b11..cd7d0901a 100644
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs
@@ -59,44 +59,37 @@ pub(crate) fn unnecessary_list_comprehension_set(checker: &mut Checker, call: &a
     if argument.is_list_comp_expr() {
         let diagnostic = Diagnostic::new(UnnecessaryListComprehensionSet, call.range());
         let fix = {
+            let one = TextSize::from(1);
+
             // Replace `set(` with `{`.
             let call_start = Edit::replacement(
                 pad_start("{", call.range(), checker.locator(), checker.semantic()),
                 call.start(),
-                call.arguments.start() + TextSize::from(1),
+                call.arguments.start() + one,
             );
 
             // Replace `)` with `}`.
             let call_end = Edit::replacement(
                 pad_end("}", call.range(), checker.locator(), checker.semantic()),
-                call.arguments.end() - TextSize::from(1),
+                call.arguments.end() - one,
                 call.end(),
             );
 
-            // If the list comprehension is parenthesized, remove the parentheses in addition to
-            // removing the brackets.
-            if let Some(range) = parenthesized_range(
+            // If the list comprehension is parenthesized, replace the whole parenthesized range.
+            let replacement_range = parenthesized_range(
                 argument.into(),
                 (&call.arguments).into(),
                 checker.comment_ranges(),
                 checker.locator().contents(),
-            ) {
-                // The generator produces the brackets that need to be removed
-                let generator = checker.generator().expr(argument);
-                let generator = generator[1..generator.len() - 1].to_string();
-                let replacement = Edit::range_replacement(generator, range);
-                Fix::unsafe_edits(call_start, [call_end, replacement])
-            } else {
-                // Delete the open bracket (`[`).
-                let argument_start =
-                    Edit::deletion(argument.start(), argument.start() + TextSize::from(1));
+            )
+            .unwrap_or_else(|| argument.range());
 
-                // Delete the close bracket (`]`).
-                let argument_end =
-                    Edit::deletion(argument.end() - TextSize::from(1), argument.end());
+            // remove the leading `[` and trailing `]`
+            let span = argument.range().add_start(one).sub_end(one);
+            let replacement =
+                Edit::range_replacement(checker.source()[span].to_string(), replacement_range);
 
-                Fix::unsafe_edits(call_start, [argument_start, argument_end, call_end])
-            }
+            Fix::unsafe_edits(call_start, [call_end, replacement])
         };
         checker.diagnostics.push(diagnostic.with_fix(fix));
     }

```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:87 on 2025-02-02 17:25_

This isn't related to your changes, but while we're here, I'd probably slightly prefer to make

https://github.com/astral-sh/ruff/blob/d9a1034db05ff6627323339d9395e80e78e8f47a/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs#L58-L59

into an early return to avoid a level of nesting, and similarly move the code out of the `fix = {` block. The last line of the unwrapped block could just be `let fix = Fix::unsafe_edits(call_start, [call_end, replacement]);`.

---

_@ntBre requested changes on 2025-02-02 17:46_

Thanks for working on this! I added some suggestions, but I think the approach with `parenthesized_range` is perfect.

---

_Label `bug` added by @ntBre on 2025-02-02 17:49_

---

_Label `fixes` added by @ntBre on 2025-02-02 17:49_

---

_@jbramley reviewed on 2025-02-02 23:32_

---

_Review comment by @jbramley on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:87 on 2025-02-02 23:32_

Fantastic! Thank you. I didn't know about slicing the source like that. Makes things a lot simpler.

---

_Review requested from @ntBre by @MichaReiser on 2025-02-03 14:55_

---

_@ntBre reviewed on 2025-02-03 18:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_list_comprehension_set.rs`:87 on 2025-02-03 18:16_

No problem, I just learned about that trick myself!

---

_@ntBre approved on 2025-02-03 18:21_

LGTM! Thanks again for your work on this!

---

_Merged by @ntBre on 2025-02-03 18:26_

---

_Closed by @ntBre on 2025-02-03 18:26_

---
