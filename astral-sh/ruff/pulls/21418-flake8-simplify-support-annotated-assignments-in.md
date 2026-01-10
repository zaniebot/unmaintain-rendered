```yaml
number: 21418
title: "[`flake8-simplify`] Support annotated assignments in if block (`SIM108`)"
type: pull_request
state: closed
author: danparizher
labels:
  - rule
  - preview
assignees: []
base: main
head: fix-21406
created_at: 2025-11-13T04:22:59Z
updated_at: 2025-12-30T17:29:16Z
url: https://github.com/astral-sh/ruff/pull/21418
synced_at: 2026-01-10T16:36:18Z
```

# [`flake8-simplify`] Support annotated assignments in if block (`SIM108`)

---

_Pull request opened by @danparizher on 2025-11-13 04:22_

## Summary

Fixes SIM108 to apply when the if block contains an annotated assignment (e.g., `y: int = 1`) and the else block contains a regular assignment (e.g., `y = 2`), converting it to `y: int = 1 if x else 2`.

Fixes #21406.

## Problem

SIM108 only matched `Stmt::Assign` patterns in both if and else blocks, so it didn't trigger when the if block used `Stmt::AnnAssign` with a value.

## Approach

Updated pattern matching to accept both `Stmt::Assign` and `Stmt::AnnAssign` (with value) in the if block, extracted the annotation when present, and modified `assignment_ternary`, `assignment_binary_or`, and `assignment_binary_and` to accept an optional annotation and create `StmtAnnAssign` nodes when an annotation is provided.

## Test Plan

Added a test case to `SIM108.py` for the annotated assignment scenario and updated the snapshot.

---

_Comment by @danparizher on 2025-11-13 04:28_

Hi @dylwil3, I reread the updated version of `CONTRIBUTING.md` to make sure I'm aligning closer to PR expectations. I did this one manually; would you say this is closer to what you are looking for? Any specific feedback for how I created this one? Thanks!

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 04:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+11 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/291ef943e39e80e6fee1f4a009b62b9392b01827/providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py#L192'>providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py:192:13:</a> SIM108 Use ternary operator `token_arg: dict[str, str] = {"nextToken": next_token} if next_token is not None else {}` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/291ef943e39e80e6fee1f4a009b62b9392b01827/providers/amazon/src/airflow/providers/amazon/aws/triggers/ecs.py#L207'>providers/amazon/src/airflow/providers/amazon/aws/triggers/ecs.py:207:13:</a> SIM108 Use ternary operator `token_arg: dict[str, str] = {"nextToken": next_token} if next_token is not None else {}` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/291ef943e39e80e6fee1f4a009b62b9392b01827/providers/google/src/airflow/providers/google/cloud/triggers/bigquery.py#L479'>providers/google/src/airflow/providers/google/cloud/triggers/bigquery.py:479:21:</a> SIM108 Use ternary operator `first_job_row: str | None = None if not first_records else first_records.pop(0)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/291ef943e39e80e6fee1f4a009b62b9392b01827/providers/google/src/airflow/providers/google/cloud/triggers/bigquery.py#L486'>providers/google/src/airflow/providers/google/cloud/triggers/bigquery.py:486:21:</a> SIM108 Use ternary operator `second_job_row: str | None = None if not second_records else second_records.pop(0)` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L148'>src/bokeh/command/subcommands/file_output.py:148:9:</a> SIM108 Use ternary operator `outputs: list[str] = [] if args.output is None else list(args.output)` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/525d5c0169225f4a56d732be0dad8ad5c78b7265/libs/standard-tests/langchain_tests/integration_tests/chat_models.py#L1553'>libs/standard-tests/langchain_tests/integration_tests/chat_models.py:1553:9:</a> SIM108 Use ternary operator `tool_choice: str | None = "any" if self.has_tool_choice else None` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/c314f9f07bb53eb9dd2d9cd6bc9572045bfc344b/cibuildwheel/oci_container.py#L473'>cibuildwheel/oci_container.py:473:9:</a> SIM108 Use ternary operator `output_io: IO[bytes] = io.BytesIO() if capture_output else sys.stdout.buffer` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/09f194b6172c5d881ed09d9eec689f477a8ffbc8/zerver/actions/bots.py#L113'>zerver/actions/bots.py:113:9:</a> SIM108 Use ternary operator `stream_name: str | None = stream.name if stream else None` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/09f194b6172c5d881ed09d9eec689f477a8ffbc8/zerver/actions/bots.py#L154'>zerver/actions/bots.py:154:9:</a> SIM108 Use ternary operator `stream_name: str | None = stream.name if stream else None` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/09f194b6172c5d881ed09d9eec689f477a8ffbc8/zerver/lib/narrow.py#L348'>zerver/lib/narrow.py:348:9:</a> SIM108 Use ternary operator `maybe_negate: ConditionTransform = not_ if negated else lambda cond: cond` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/09f194b6172c5d881ed09d9eec689f477a8ffbc8/zilencer/management/commands/calculate_first_visible_message_id.py#L31'>zilencer/management/commands/calculate_first_visible_message_id.py:31:9:</a> SIM108 Use ternary operator `realms: Iterable[Realm] = Realm.objects.all() if target_realm is None else [target_realm]` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 11 | 11 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @dylwil3 on 2025-11-14 21:54_

Thanks this is definitely easier for me to read! Could you look through the ecosystem report and add some more test coverage? For example, it would be good to have cases where both branches have the annotation (including a case where the annotations do not match). Thank you!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:358 on 2025-11-17 23:18_

There's a lot of duplication here between `assignment_binary_or` and `assignment_binary_and`. In fact, with a bit of rearranging, I'm pretty sure the only difference on main is the `BoolOp` variant:

```rust
fn assignment_binary_and(target_var: &Expr, left_value: &Expr, right_value: &Expr) -> Stmt {
    let node = ast::ExprBoolOp {
        op: BoolOp::And,
        values: vec![left_value.clone(), right_value.clone()],
        range: TextRange::default(),
        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
    };
    let node1 = ast::StmtAssign {
        targets: vec![target_var.clone()],
        value: Box::new(node.into()),
        range: TextRange::default(),
        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
    };
    node1.into()
}

fn assignment_binary_or(target_var: &Expr, left_value: &Expr, right_value: &Expr) -> Stmt {
    let node = ast::ExprBoolOp {
        op: BoolOp::Or,
        values: vec![left_value.clone(), right_value.clone()],
        range: TextRange::default(),
        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
    };
    let node1 = ast::StmtAssign {
        targets: vec![target_var.clone()],
        value: Box::new(node.into()),
        range: TextRange::default(),
        node_index: ruff_python_ast::AtomicNodeIndex::NONE,
    };
    node1.into()
}
```

I think it would make sense to factor out the commonality and just pass in the `BoolOp`, especially now that we have to consider the annotated case.

It seems a little harder to avoid in the ternary case.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM108.py`:246 on 2025-11-17 23:23_

Is there something special about these different annotation types? I think just covering the cases where both branches have annotations, one matching and one not, is probably sufficient since we don't really inspect the annotations, as far as I can tell.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:115 on 2025-11-17 23:25_

tiny nit: I don't think this comment is very helpful. The match seems straightforward enough.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:218 on 2025-11-17 23:27_

Do we need these `as_ref` calls on `else_value`? It doesn't look like the type of `else_value` has changed, so I think it should still be okay to omit them.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_if_exp.rs`:136 on 2025-11-17 23:29_

I think we should preview-gate this, and this would be a good place to do so:


```suggestion
        ] if is_new_preview_function_enabled(checker.settings()) => (
```

---

_@ntBre requested changes on 2025-11-17 23:31_

Thanks! This looks good to me overall, just a few suggestions for simplification, and I think we should make this a preview change since the rule is stable and had some ecosystem hits.

---

_Label `rule` added by @ntBre on 2025-11-17 23:31_

---

_Label `preview` added by @ntBre on 2025-11-17 23:31_

---

_Review requested from @ntBre by @amyreese on 2025-11-26 02:38_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM108.py`:246 on 2025-12-01 14:03_

This case is actually not triggering SIM108, but the unannotated version does in the test and in the playground: https://play.ruff.rs/ad80f8bf-776b-4243-87a9-054c21207ab2.

Also, all  of the new tests use a ternary replacement. It would be good to include tests that exercise the `or` and `and` fixes too.

---

_@ntBre requested changes on 2025-12-01 14:17_

Thanks! The code looks good to me now, but I think something is going wrong in one of the test cases.

---

_Comment by @ntBre on 2025-12-30 16:51_

Just commented on the issue, but I didn't realize that the issue was a duplicate of an older issue (#15554), which also had an older PR (#15665) with a `needs-decision` label. I'll close this for now, but hopefully we can use some of the code from both PRs, if we decide to make this change. Thanks for your work here!

---

_Closed by @ntBre on 2025-12-30 16:51_

---

_Branch deleted on 2025-12-30 17:29_

---
