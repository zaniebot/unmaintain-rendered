```yaml
number: 15876
title: "[`flake8-comprehensions`] Report and fix even when there are multiple iterables (`C417`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
base: main
head: C417
created_at: 2025-02-02T00:47:48Z
updated_at: 2025-03-17T08:03:20Z
url: https://github.com/astral-sh/ruff/pull/15876
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-comprehensions`] Report and fix even when there are multiple iterables (`C417`)

---

_@InSyncWithFoo_

## Summary

Resolves #15809.

Calls with multiple iterables are now reported and fixed in preview mode. A new generator-based fix function is added. According to test results, it is compatible with the old one, now only used in non-preview.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-02-02 00:50_

The first commit should only contain bugfix-related changes. The PR might still be too big, though; let me know if it needs splitting.

---

_Comment by @github-actions[bot] on 2025-02-02 00:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/5d9cf431f7b774a6724b1dd4c5e6f6fe95647aff/pandas/tests/io/test_html.py#L57'>pandas/tests/io/test_html.py:57:9:</a> C417 Unnecessary `map()` usage (rewrite using a generator expression)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C417 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:854 on 2025-02-03 12:02_

theoretically we could try to import `builtins` and then use the fully qualified `builtins.zip` here if `zip` is shadowed. I don't think it's something for this PR, though, and it may not be worth the complexity to ever do it. Just a thought.

Also, a nitpick: I'd probably just say "shadowed" rather than "overshadowed"

```suggestion
    } else {
        bail!("Cannot create fix: `zip` is shadowed locally");
    };
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:869 on 2025-02-03 12:04_

```suggestion
        let mut elements: Vec<Expr> = parameters
            .posonlyargs
            .iter()
            .chain(&parameters.args)
            .map(|parameter| {
                Expr::Name(ast::ExprName {
                    id: parameter.name().id.clone(),
                    ctx: ExprContext::Store,
                    range: TextRange::default(),
                })
            })
            .collect();
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:941 on 2025-02-03 12:13_

if we take the first branch here then we end up having to do two allocations: one to convert the argument to a `String` before passing it into the function, and another allocation in the `format!()` call. If we have the argument take `&str` rather than `String`, we only have to do one allocation no matter which branch is taken:

```diff
--- a/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs
+++ b/crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs
@@ -933,11 +933,11 @@ pub(crate) fn fix_unnecessary_map_preview(
 
     let replacements: Vec<String> = match object_type {
         ObjectType::Dict => {
-            fn parenthesized_if_necessary(expr: &Expr, as_str: String) -> String {
+            fn parenthesized_if_necessary(expr: &Expr, as_str: &str) -> String {
                 if expr.is_named_expr() {
                     format!("({as_str})")
                 } else {
-                    as_str
+                    as_str.to_string()
                 }
             }
 
@@ -957,8 +957,8 @@ pub(crate) fn fix_unnecessary_map_preview(
             let original_value = original_expr_in_source(value.into(), body.into());
 
             vec![
-                parenthesized_if_necessary(key, original_key.to_string()),
-                parenthesized_if_necessary(value, original_value.to_string()),
+                parenthesized_if_necessary(key, original_key),
+                parenthesized_if_necessary(value, original_value),
             ]
         }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:978 on 2025-02-03 12:15_

Do we also need to check whether the grandparent expression is an f-string? And the great-grandparent? And the great-great-grandparent? (Etc.)

Iterating over `semantic.current_expressions()` and checking whether any of them are f-string expressions might be more robust, if so

https://github.com/astral-sh/ruff/blob/638186afbd571c1a618bbcf356dd0f50785370ca/crates/ruff_python_semantic/src/model.rs#L1273-L1280

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:984 on 2025-02-03 12:17_

I don't feel like I fully understand why we need a regex here rather than using the `str::replacen` method https://doc.rust-lang.org/std/primitive.str.html#method.replacen

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:183 on 2025-02-03 12:19_

This seems inefficient, because we have to resolve the qualified name multiple times. Something like this might be cheaper:

```suggestion
    semantic
        .resolve_qualified_name(func)
        .is_some_and(|qualified_name| {
            matches!(
                qualified_name.segments(),
                ["" | "buitins", "list" | "set" | "dict"]
            )
        })
```

---

_@AlexWaygood reviewed on 2025-02-03 12:19_

Some comments from briefly skimming through

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:984 on 2025-02-03 15:38_

The replacements are different depending on the indices. Replacing twice also doesn't work, as the replacement might also contain `...`. There's a testcase for this.

---

_@InSyncWithFoo reviewed on 2025-02-03 15:38_

---

_@AlexWaygood reviewed on 2025-02-03 15:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/fixes.rs`:984 on 2025-02-03 15:45_

Could you add a comment to the source code explaining why `replacen` doesn't work here? I think other people reading this code in the future might have the same question I did :-)

---

_Comment by @MichaReiser on 2025-02-04 10:28_

@AlexWaygood already started reviewing so I assume he's fine leaving the PR as is? 

I'm slightly leaning towards splitting the PR into a) bug fixes and b) preview behavior change. It has the advantage that it results in nicer changelog entries (and might be easier to review)

---

_Comment by @AlexWaygood on 2025-02-04 10:33_

> @AlexWaygood already started reviewing so I assume he's fine leaving the PR as is?
> I'm slightly leaning towards splitting the PR into a) bug fixes and b) preview behavior change. It has the advantage that it results in nicer changelog entries (and might be easier to review)

I only really skimmed; I haven't done a proper review. Splitting it up sounds sensible to me too 👍

---

_Comment by @InSyncWithFoo on 2025-02-05 01:02_

Submitted #15955. I'll rebase this one once that gets merged.

---

_Converted to draft by @MichaReiser on 2025-02-05 08:03_

---

_Comment by @MichaReiser on 2025-02-05 08:04_

Thanks. I'll convert this PR to draft in the meantime.

---

_Renamed from "[`flake8-comprehensions`] Detect overshadowed `list`/`set`/`dict`, ignore variadics and named expressions as well as report and fix even when there are multiple iterables (`C417`)" to "[`flake8-comprehensions`] Report and fix even when there are multiple iterables (`C417`)" by @InSyncWithFoo on 2025-02-07 21:23_

---

_Marked ready for review by @InSyncWithFoo on 2025-02-07 21:23_

---

_Label `rule` added by @MichaReiser on 2025-02-14 07:45_

---

_Label `preview` added by @MichaReiser on 2025-02-14 07:45_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-03-11 08:52_

---

_Closed by @MichaReiser on 2025-03-17 07:50_

---

_Reopened by @MichaReiser on 2025-03-17 07:50_

---
