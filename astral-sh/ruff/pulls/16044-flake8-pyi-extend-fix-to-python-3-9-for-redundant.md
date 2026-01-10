```yaml
number: 16044
title: "[`flake8-pyi`] Extend fix to Python <= 3.9 for `redundant-none-literal` (`PYI061`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: extra-none
created_at: 2025-02-08T21:20:46Z
updated_at: 2025-02-09T15:58:53Z
url: https://github.com/astral-sh/ruff/pull/16044
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-pyi`] Extend fix to Python <= 3.9 for `redundant-none-literal` (`PYI061`)

---

_Pull request opened by @dylwil3 on 2025-02-08 21:20_

This PR extends the fix offered for [redundant-none-literal (PYI061)](https://docs.astral.sh/ruff/rules/redundant-none-literal/#redundant-none-literal-pyi061) to include Python versions <= 3.9 by using `typing.Optional` instead of the operator `|`. We also offer the fix with `|` for any target version on stub files.

Closes #15795

------

For review: 

- I recommend reading the extended commit message for the second commit (where the fix logic is implemented), which hopefully explains the perhaps larger than expected diff.
- Note that the snapshots can still have bit-or operators `|` since we will not remove them if they are already present, e.g. `Literal[None] | int` simplifies to `None | int` regardless of Python version or source type.

---

_Label `fixes` added by @dylwil3 on 2025-02-08 21:20_

---

_Label `preview` added by @dylwil3 on 2025-02-08 21:20_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-02-08 21:20_

---

_Comment by @github-actions[bot] on 2025-02-08 21:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -15 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6f1c5b75cea28ed3687cc3cfa6cd65954dc26247/airflow/models/dag.py#L1016'>airflow/models/dag.py:1016:36:</a> PYI061 [*] Use `Optional[Literal[...]]` rather than `Literal[None, ...]` 
- <a href='https://github.com/apache/airflow/blob/6f1c5b75cea28ed3687cc3cfa6cd65954dc26247/airflow/models/dag.py#L1016'>airflow/models/dag.py:1016:36:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 [*] Use `Optional[Literal[...]]` rather than `Literal[None, ...]` 
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/core/groupby/groupby.py#L4181'>pandas/core/groupby/groupby.py:4181:39:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/core/groupby/groupby.py#L4181'>pandas/core/groupby/groupby.py:4181:39:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/io/html.py#L1045'>pandas/io/html.py:1045:28:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/io/html.py#L1045'>pandas/io/html.py:1045:28:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/pandas-dev/pandas/blob/e557039fda7d4325184cf76520892b3a635ec2dd/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/billing_page.py#L328'>corporate/views/billing_page.py:328:24:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/billing_page.py#L328'>corporate/views/billing_page.py:328:24:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L158'>corporate/views/remote_billing_page.py:158:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L159'>corporate/views/remote_billing_page.py:159:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L160'>corporate/views/remote_billing_page.py:160:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:26:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L679'>corporate/views/remote_billing_page.py:679:26:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:42:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L680'>corporate/views/remote_billing_page.py:680:42:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L681'>corporate/views/remote_billing_page.py:681:48:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L681'>corporate/views/remote_billing_page.py:681:48:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 [*] Use `Literal[...] | None` rather than `Literal[None, ...]` 
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:5:</a> PYI061 [*] `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/event_types.py#L785'>zerver/lib/event_types.py:785:67:</a> PYI061 [*] Use `None` rather than `Literal[None]`
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/event_types.py#L785'>zerver/lib/event_types.py:785:67:</a> PYI061 [*] `Literal[None]` can be replaced with `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI061 | 30 | 15 | 15 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2025-02-08 22:11_

Ecosystem changes as expected - both repos have `target-version` set to `py39`.

---

_Comment by @AlexWaygood on 2025-02-08 23:23_

> + [airflow/models/dag.py:1016:36:](https://github.com/apache/airflow/blob/fd606b1d6e1469ebf977e327c947a1d288b5f0d2/airflow/models/dag.py#L1016) PYI061 [*] `Literal[None, ...]` can be replaced with `Union[Literal[...], None]`

Hmmm... wouldn't it be better to suggest `Optional[Literal[...]]` here rather than `Union[Literal[...], None]`? I think most people would consider that more idiomatic on Python <=3.9

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:241 on 2025-02-08 23:27_

nit: it would be better to return `anyhow::Result<Fix>` or `anyhow::Result<Option<Fix>>` from this function, do this here, and use `Diagnostic::try_set_fix` or `Diagnostic::try_set_optional_fix` to add the fix to the diagnostic:

```suggestion
            let (import_edit, bound_name) = checker
                .importer()
                .get_or_import_symbol(
                    &ImportRequest::import_from("typing", "Union"),
                    literal_expr.start(),
                    checker.semantic(),
                )?;
```

The advantage is that if you use `try_set_fix` or `try_set_optional_fix`, Ruff will print an error message to the terminal (if you run it with enough `-v` flags) with information from the `ResolutionError` here when this function returns an error. That makes it easier for users or us to debug why Ruff was unable to create an autofix in a specific situation.

---

_@AlexWaygood reviewed on 2025-02-08 23:27_

---

_Comment by @dylwil3 on 2025-02-09 01:58_

> Hmmm... wouldn't it be better to suggest Optional[Literal[...]]

🤦 of course! much better

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:66 on 2025-02-09 12:07_

While we're here, it might be nice to change these so they're a little more imperative:

```suggestion
            UnionKind::NoUnion => "Use `None` rather than `Literal[None]`".to_string(),
            UnionKind::TypingOptional => {
                "Use `Optional[Literal[...]]` rather than `Literal[None, ...]`".to_string()
            }
            UnionKind::BitOr => {
                "Use `Literal[...] | None` rather than `Literal[None, ...]`".to_string()
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:228 on 2025-02-09 12:08_

ignorable nit (just my stylistic preference!) -- you could reduce some duplication between the branches here:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
index 2f5e892d0..ddaa51ac8 100644
--- a/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
+++ b/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs
@@ -225,7 +225,7 @@ fn create_fix(
         }),
     });
 
-    match union_kind {
+    let fix = match union_kind {
         UnionKind::TypingOptional => {
             let (import_edit, bound_name) = checker.importer().get_or_import_symbol(
                 &ImportRequest::import_from("typing", "Optional"),
@@ -235,11 +235,7 @@ fn create_fix(
             let optional_expr = typing_optional(new_literal_expr, Name::from(bound_name));
             let content = checker.generator().expr(&optional_expr);
             let optional_edit = Edit::range_replacement(content, literal_expr.range());
-            Ok(Some(Fix::applicable_edits(
-                import_edit,
-                [optional_edit],
-                applicability,
-            )))
+            Fix::applicable_edits(import_edit, [optional_edit], applicability)
         }
         UnionKind::BitOr => {
             let none_expr = Expr::NoneLiteral(ExprNoneLiteral {
@@ -248,13 +244,15 @@ fn create_fix(
             let union_expr = pep_604_union(&[new_literal_expr, none_expr]);
             let content = checker.generator().expr(&union_expr);
             let union_edit = Edit::range_replacement(content, literal_expr.range());
-            Ok(Some(Fix::applicable_edit(union_edit, applicability)))
+            Fix::applicable_edit(union_edit, applicability)
         }
         // We dealt with this case earlier to avoid allocating `lhs` and `rhs`
         UnionKind::NoUnion => {
             unreachable!()
         }
-    }
+    };
+
+    Ok(Some(fix))
 }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_none_literal.rs`:260 on 2025-02-09 12:09_

I tend to add these to simple enums like this, because you never know when they might be useful in tests or debug prints

```suggestion
#[derive(Copy, Clone, Debug, PartialEq, Eq)]
```

---

_@AlexWaygood approved on 2025-02-09 12:10_

Excellent, thank you!

---

_Merged by @dylwil3 on 2025-02-09 15:58_

---

_Closed by @dylwil3 on 2025-02-09 15:58_

---
