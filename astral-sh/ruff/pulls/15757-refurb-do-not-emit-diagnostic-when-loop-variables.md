```yaml
number: 15757
title: "[`refurb`] Do not emit diagnostic when loop variables are used outside loop body (`FURB122`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: FURB122
created_at: 2025-01-27T01:42:03Z
updated_at: 2025-01-30T18:17:53Z
url: https://github.com/astral-sh/ruff/pull/15757
synced_at: 2026-01-12T15:55:52Z
```

# [`refurb`] Do not emit diagnostic when loop variables are used outside loop body (`FURB122`)

---

_@InSyncWithFoo_

## Summary

Resolves #15725.

Previously, `FURB122` analyzes `for` loops only at AST level, resulting in false positives, as shown in the linked issue.

With this change, now `FURB122` will also analyze `for` loops at binding level. If a loop variable is used outside of the loop or shadows another variable, no diagnostic will be emitted.

Two-step analysis is necessary since it's possible for `for` loops not to have any bindings:

```python
with open('file.txt', 'w') as f:
	for () in range(10):
		f.write('line')
```

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-27 01:43_

As the rule is now binding-sensitive, existing tests need to be rescoped. This leads to a rather big change, which I have isolated to the first commit.

---

_Comment by @github-actions[bot] on 2025-01-27 01:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/15d2e200d8651cf5dbc55d5f06bc01f5457fe9a9/docs/exts/extra_files_with_substitutions.py#L51'>docs/exts/extra_files_with_substitutions.py:51:13:</a> FURB122 [*] Use of `output_file.write` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch_cli/services/get_params.py#L180'>src/latch_cli/services/get_params.py:180:9:</a> FURB122 [*] Use of `f.write` in a for loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB122 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Renamed from "[`refurb`] Do not emit diagnostic when loop variables are used outside of the loop (`FURB122`)" to "[`refurb`] Do not emit diagnostic when loop variables are used outside loop body (`FURB122`)" by @InSyncWithFoo on 2025-01-27 02:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:65 on 2025-01-27 14:06_

```suggestion
    let for_stmt = binding.statement(semantic)?.as_for_stmt()?;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:116 on 2025-01-27 14:16_

```suggestion
            Expr::List(ExprList { elts, .. }) | Expr::Tuple(ExprTuple { elts, .. }) => elts
                .iter()
                .for_each(|element| collect_names(element, names)),
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:122 on 2025-01-27 15:04_

Since this now returns `None`, you can simplify several things in the body of this function using the `?` operator (the diff is relative to your branch):

```diff
diff --git a/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs b/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
index a1308bbc0..03c84dd21 100644
--- a/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
+++ b/crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs
@@ -136,12 +136,8 @@ fn for_loop_writes(
     let [Stmt::Expr(stmt_expr)] = for_stmt.body.as_slice() else {
         return None;
     };
-    let Expr::Call(call_expr) = stmt_expr.value.as_ref() else {
-        return None;
-    };
-    let Expr::Attribute(expr_attr) = call_expr.func.as_ref() else {
-        return None;
-    };
+    let call_expr = stmt_expr.value.as_ref().as_call_expr()?;
+    let expr_attr = call_expr.func.as_ref().as_attribute_expr()?;
     if expr_attr.attr.as_str() != "write" {
         return None;
     }
@@ -151,19 +147,13 @@ fn for_loop_writes(
     let [write_arg] = call_expr.arguments.args.as_ref() else {
         return None;
     };
-
-    let Expr::Name(io_object_name) = expr_attr.value.as_ref() else {
-        return None;
-    };
+    let io_object_name = expr_attr.value.as_ref().as_name_expr()?;
 
     let semantic = checker.semantic();
 
     // Determine whether `f` in `f.write()` was bound to a file object.
-    if !semantic
-        .resolve_name(io_object_name)
-        .map(|id| semantic.binding(id))
-        .is_some_and(|binding| typing::is_io_base(binding, semantic))
-    {
+    let binding = semantic.binding(semantic.resolve_name(io_object_name)?);
+    if !typing::is_io_base(binding, semantic) {
         return None;
     }
```

---

_@AlexWaygood reviewed on 2025-01-27 15:05_

Not a full review yet, just a couple of style nitpicks :-)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:57 on 2025-01-28 10:42_

```suggestion
    if !binding.kind.is_loop_var() {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:215 on 2025-01-28 10:55_

Could you add some comments here that explain what these steps do. I think I understand what's going on, but it's not trivial to understand for a new reader of the code

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:104 on 2025-01-28 10:55_

```suggestion
            Expr::Starred(starred) => collect_names(&starred.value, names),
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:133 on 2025-01-28 10:56_

```suggestion
    let call_expr = stmt_expr.value.as_call_expr()?;
    let expr_attr = call_expr.func.as_attribute_expr()?;
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:135 on 2025-01-28 10:56_

```suggestion
    if &expr_attr.attr != "write" {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/for_loop_writes.rs`:146 on 2025-01-28 10:56_

```suggestion
    let io_object_name = expr_attr.value.as_name_expr()?;
```

---

_@AlexWaygood reviewed on 2025-01-28 10:57_

This looks good, thanks. A couple more points below, but nothing major

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-28 10:57_

---

_@AlexWaygood approved on 2025-01-28 19:10_

Thanks!

---

_Merged by @AlexWaygood on 2025-01-28 19:16_

---

_Closed by @AlexWaygood on 2025-01-28 19:16_

---

_Branch deleted on 2025-01-28 21:12_

---
