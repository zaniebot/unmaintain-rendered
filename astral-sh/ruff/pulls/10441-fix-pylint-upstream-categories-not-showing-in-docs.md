```yaml
number: 10441
title: Fix pylint upstream categories not showing in docs
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-pylint-upstream-category
created_at: 2024-03-18T01:01:16Z
updated_at: 2024-03-18T01:38:26Z
url: https://github.com/astral-sh/ruff/pull/10441
synced_at: 2026-01-10T22:47:02Z
```

# Fix pylint upstream categories not showing in docs

---

_Pull request opened by @augustelalande on 2024-03-18 01:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The upstream category check here
https://github.com/astral-sh/ruff/blob/fd26b299865997fc9a89608d5e090f9c05360997/crates/ruff_linter/src/upstream_categories.rs#L54-L65

was not working because the code is actually "E0001" not "PLE0001", I changed it so it will detect the upstream category correctly.

I also sorted the upstream categories alphabetically, so that the document generation will be deterministic.

## Test Plan

I compared the diff before and after the change.

```
➜  git diff
warning: in the working copy of 'docs/rules.md', CRLF will be replaced by LF the next time Git touches it
diff --git a/crates/ruff_dev/src/generate_rules_table.rs b/crates/ruff_dev/src/generate_rules_table.rs
index c167c018d..414b3df18 100644
--- a/crates/ruff_dev/src/generate_rules_table.rs
+++ b/crates/ruff_dev/src/generate_rules_table.rs
@@ -180,8 +180,17 @@ pub(crate) fn generate() -> String {
             .map(|rule| (rule.upstream_category(&linter), rule))
             .into_group_map();

+        let mut rules_by_upstream_category: Vec<_> = rules_by_upstream_category.iter().collect();
+
+        // Sort the upstream categories alphabetically by prefix.
+        rules_by_upstream_category.sort_by(|a, b| {
+            a.0.as_ref()
+                .map_or("", |x| &x.prefix)
+                .cmp(&b.0.as_ref().map_or("", |x| &x.prefix))
+        });
+
         if rules_by_upstream_category.len() > 1 {
-            for (opt, rules) in &rules_by_upstream_category {
+            for (opt, rules) in rules_by_upstream_category {
                 if opt.is_some() {
                     let UpstreamCategoryAndPrefix { category, prefix } = opt.unwrap();
                     table_out.push_str(&format!("#### {category} ({prefix})"));
diff --git a/crates/ruff_linter/src/upstream_categories.rs b/crates/ruff_linter/src/upstream_categories.rs
index 905cf8b72..9f94759e2 100644
--- a/crates/ruff_linter/src/upstream_categories.rs
+++ b/crates/ruff_linter/src/upstream_categories.rs
@@ -41,19 +41,19 @@ impl Rule {
                     None
                 }
             }
-            // Linter::Pylint => {
-            //     if code.starts_with('C') {
-            //         Some(C)
-            //     } else if code.starts_with('E') {
-            //         Some(E)
-            //     } else if code.starts_with('R') {
-            //         Some(R)
-            //     } else if code.starts_with('W') {
-            //         Some(W)
-            //     } else {
-            //         None
-            //     }
-            // }
+            Linter::Pylint => {
+                if code.starts_with('C') {
+                    Some(C)
+                } else if code.starts_with('E') {
+                    Some(E)
+                } else if code.starts_with('R') {
+                    Some(R)
+                } else if code.starts_with('W') {
+                    Some(W)
+                } else {
+                    None
+                }
+            }
             _ => None,
         }
     }
diff --git a/docs/rules.md b/docs/rules.md
index 3ece0d0f4..1ec9a4842 100644
--- a/docs/rules.md
+++ b/docs/rules.md
@@ -67,18 +67,6 @@ For more, see [Pyflakes](https://pypi.org/project/pyflakes/) on PyPI.

 For more, see [pycodestyle](https://pypi.org/project/pycodestyle/) on PyPI.

-### Warning (W)
-
-| Code | Name | Message | |
-| ---- | ---- | ------- | ------: |
-| W191 { #W191 } | [tab-indentation](rules/tab-indentation.md) | Indentation contains tabs | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity 0.1' aria-hidden='true'>🛠️</span> |
-| W291 { #W291 } | [trailing-whitespace](rules/trailing-whitespace.md) | Trailing whitespace | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
-| W292 { #W292 } | [missing-newline-at-end-of-file](rules/missing-newline-at-end-of-file.md) | No newline at end of file | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix
available'>🛠️</span> |
-| W293 { #W293 } | [blank-line-with-whitespace](rules/blank-line-with-whitespace.md) | Blank line contains whitespace | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix avilable'>🛠️</span> |
-| W391 { #W391 } | [too-many-newlines-at-end-of-file](rules/too-many-newlines-at-end-of-file.md) | Too many newlines at end of file | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
-| W505 { #W505 } | [doc-line-too-long](rules/doc-line-too-long.md) | Doc line too long ({width} > {limit}) | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
-| W605 { #W605 } | [invalid-escape-sequence](rules/invalid-escape-sequence.md) | Invalid escape sequence: `\{ch}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix availale'>🛠️</span> |
le='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
-| W292 { #W292 } | [missing-newline-at-end-of-file](rules/missing-newline-at-end-of-file.md) | No newline at end of file | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix vailable'>🛠️</span> |
-| W293 { #W293 } | [blank-line-with-whitespace](rules/blank-line-with-whitespace.md) | Blank line contains whitespace | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix avalable'>🛠️</span> |
-| W391 { #W391 } | [too-many-newlines-at-end-of-file](rules/too-many-newlines-at-end-of-file.md) | Too many newlines at end of file | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
-| W505 { #W505 } | [doc-line-too-long](rules/doc-line-too-long.md) | Doc line too long ({width} > {limit}) | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available'style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
-| W605 { #W605 } | [invalid-escape-sequence](rules/invalid-escape-sequence.md) | Invalid escape sequence: `\{ch}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix availabe'>🛠️</span> |
-
 ### Error (E)

 | Code | Name | Message | |
@@ -143,6 +131,18 @@ For more, see [pycodestyle](https://pypi.org/project/pycodestyle/) on PyPI.
 | E902 { #E902 } | [io-error](rules/io-error.md) | {message\} | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️<pan> |
 | E999 { #E999 } | [syntax-error](rules/syntax-error.md) | SyntaxError: {message\} | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity: 0.1' aia-hidden='true'>🛠️</span> |

+### Warning (W)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
+| W191 { #W191 } | [tab-indentation](rules/tab-indentation.md) | Indentation contains tabs | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity 0.1' aria-hidden='true'>🛠️</span> |
+| W291 { #W291 } | [trailing-whitespace](rules/trailing-whitespace.md) | Trailing whitespace | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
+| W292 { #W292 } | [missing-newline-at-end-of-file](rules/missing-newline-at-end-of-file.md) | No newline at end of file | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix
available'>🛠️</span> |
+| W293 { #W293 } | [blank-line-with-whitespace](rules/blank-line-with-whitespace.md) | Blank line contains whitespace | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix avilable'>🛠️</span> |
+| W391 { #W391 } | [too-many-newlines-at-end-of-file](rules/too-many-newlines-at-end-of-file.md) | Too many newlines at end of file | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
+| W505 { #W505 } | [doc-line-too-long](rules/doc-line-too-long.md) | Doc line too long ({width} > {limit}) | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
+| W605 { #W605 } | [invalid-escape-sequence](rules/invalid-escape-sequence.md) | Invalid escape sequence: `\{ch}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix availale'>🛠️</span> |
+
 ## mccabe (C90)

 For more, see [mccabe](https://pypi.org/project/mccabe/) on PyPI.
@@ -999,6 +999,8 @@ For more, see [pygrep-hooks](https://github.com/pre-commit/pygrep-hooks) on GitH

 For more, see [Pylint](https://pypi.org/project/pylint/) on PyPI.

+### Convention (C)
+
 | Code | Name | Message | |
 | ---- | ---- | ------- | ------: |
 | PLC0105 { #PLC0105 } | [type-name-incorrect-variance](rules/type-name-incorrect-variance.md) | `{kind}` name "{param_name}" does not reflect its {variance}; consider renaming it to "{replacement_name}" | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style=opacity: 0.1' aria-hidden='true'>🛠️</span> |
@@ -1014,6 +1016,11 @@ For more, see [Pylint](https://pypi.org/project/pylint/) on PyPI.
 | PLC2701 { #PLC2701 } | [import-private-name](rules/import-private-name.md) | Private name import `{name}` from external module `{module}` | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLC2801 { #PLC2801 } | [unnecessary-dunder-call](rules/unnecessary-dunder-call.md) | Unnecessary dunder call to `{method}`. {replacement}. | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
 | PLC3002 { #PLC3002 } | [unnecessary-direct-lambda-call](rules/unnecessary-direct-lambda-call.md) | Lambda expression called directly. Execute the expression inline instead. | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hidden='tue'>🛠️</span> |
+
+### Error (E)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLE0100 { #PLE0100 } | [yield-in-init](rules/yield-in-init.md) | `__init__` method is a generator | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style'opacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLE0101 { #PLE0101 } | [return-in-init](rules/return-in-init.md) | Explicit return in `__init__` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style=opacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLE0116 { #PLE0116 } | [continue-in-finally](rules/continue-in-finally.md) | `continue` not supported inside `finally` clause | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automaic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
@@ -1046,6 +1053,11 @@ For more, see [Pylint](https://pypi.org/project/pylint/) on PyPI.
 | PLE2513 { #PLE2513 } | [invalid-character-esc](rules/invalid-character-esc.md) | Invalid unescaped character ESC, use "\x1B" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title=Automatic fix available'>🛠️</span> |
 | PLE2514 { #PLE2514 } | [invalid-character-nul](rules/invalid-character-nul.md) | Invalid unescaped character NUL, use "\0" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Atomatic fix available'>🛠️</span> |
 | PLE2515 { #PLE2515 } | [invalid-character-zero-width-space](rules/invalid-character-zero-width-space.md) | Invalid unescaped character zero-width-space, use "\u200B" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
+
+### Refactor (R)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLR0124 { #PLR0124 } | [comparison-with-itself](rules/comparison-with-itself.md) | Name compared with itself, consider replacing `{actual}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <spantitle='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLR0124 { #PLR0124 } | [comparison-with-itself](rules/comparison-with-itself.md) | Name compared with itself, consider replacing `{actual}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span itle='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
le='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hidden='tre'>🛠️</span> |
+
+### Error (E)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLE0100 { #PLE0100 } | [yield-in-init](rules/yield-in-init.md) | `__init__` method is a generator | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style=opacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLE0101 { #PLE0101 } | [return-in-init](rules/return-in-init.md) | Explicit return in `__init__` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='pacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLE0116 { #PLE0116 } | [continue-in-finally](rules/continue-in-finally.md) | `continue` not supported inside `finally` clause | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatc fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
@@ -1046,6 +1053,11 @@ For more, see [Pylint](https://pypi.org/project/pylint/) on PyPI.
 | PLE2513 { #PLE2513 } | [invalid-character-esc](rules/invalid-character-esc.md) | Invalid unescaped character ESC, use "\x1B" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='utomatic fix available'>🛠️</span> |
 | PLE2514 { #PLE2514 } | [invalid-character-nul](rules/invalid-character-nul.md) | Invalid unescaped character NUL, use "\0" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Auomatic fix available'>🛠️</span> |
 | PLE2515 { #PLE2515 } | [invalid-character-zero-width-space](rules/invalid-character-zero-width-space.md) | Invalid unescaped character zero-width-space, use "\u200B" instead | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
+
+### Refactor (R)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLR0124 { #PLR0124 } | [comparison-with-itself](rules/comparison-with-itself.md) | Name compared with itself, consider replacing `{actual}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <spantitle='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
 | PLR0133 { #PLR0133 } | [comparison-of-constant](rules/comparison-of-constant.md) | Two constants compared in a comparison, consider replacing `{left_constant} {} {right_constant}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hiden='true'>🛠️</span> |
 | PLR0202 { #PLR0202 } | [no-classmethod-decorator](rules/no-classmethod-decorator.md) | Class method defined without decorator | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
@@ -1074,6 +1086,11 @@ For more, see [Pylint](https://pypi.org/project/pylint/) on PyPI.
 | PLR5501 { #PLR5501 } | [collapsible-else-if](rules/collapsible-else-if.md) | Use `elif` instead of `else` then `if`, to reduce indentation | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span itle='Automatic fix available'>🛠️</span> |
 | PLR6201 { #PLR6201 } | [literal-membership](rules/literal-membership.md) | Use a `set` literal when testing for membership | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠</span> |
 | PLR6301 { #PLR6301 } | [no-self-use](rules/no-self-use.md) | Method `{method_name}` could be a function, class method, or static method | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
+
+### Warning (W)
+
+| Code | Name | Message | |
+| ---- | ---- | ------- | ------: |
 | PLW0108 { #PLW0108 } | [unnecessary-lambda](rules/unnecessary-lambda.md) | Lambda may be unnecessary; consider inlining inner function | <span title='Rule is in preview'>🧪</span> <span title='Automatic fix available'>🛠️</span> |
 | PLW0120 { #PLW0120 } | [useless-else-on-loop](rules/useless-else-on-loop.md) | `else` clause on loop without a `break` statement; remove the `else` and dedent its contents | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic fix available'>🛠️</span> |
 | PLW0127 { #PLW0127 } | [self-assigning-variable](rules/self-assigning-variable.md) | Self-assignment of variable `{name}` | <span title='Rule is stable' style='opacity: 0.6'>✔️</span> <span title='Automatic ix not available' style='opacity: 0.1' aria-hidden='true'>🛠️</span> |
```


---

_@charliermarsh approved on 2024-03-18 01:18_

Thank you!

---

_Label `bug` added by @charliermarsh on 2024-03-18 01:19_

---

_Merged by @charliermarsh on 2024-03-18 01:27_

---

_Closed by @charliermarsh on 2024-03-18 01:27_

---

_Branch deleted on 2024-03-18 01:30_

---

_Comment by @github-actions[bot] on 2024-03-18 01:38_

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
