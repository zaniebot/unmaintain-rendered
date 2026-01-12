```yaml
number: 21278
title: "[`refurb`] Auto-fix annotated assignments (`FURB101`)"
type: pull_request
state: merged
author: danparizher
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: fix-21274
created_at: 2025-11-05T06:02:55Z
updated_at: 2025-11-08T00:05:44Z
url: https://github.com/astral-sh/ruff/pull/21278
synced_at: 2026-01-12T15:57:20Z
```

# [`refurb`] Auto-fix annotated assignments (`FURB101`)

---

_@danparizher_

## Summary

Fixed FURB101 (`read-whole-file`) to handle annotated assignments. Previously, the rule would detect violations in code like `contents: str = f.read()` but fail to generate a fix. Now it correctly generates fixes that preserve type annotations (e.g., `contents: str = Path("file.txt").read_text(encoding="utf-8")`).

Fixes #21274

## Problem Analysis

The FURB101 rule was only checking for `Stmt::Assign` statements when determining whether a fix could be applied. When encountering annotated assignments (`Stmt::AnnAssign`) like `contents: str = f.read()`, the rule would:

1. Correctly detect the violation (the diagnostic was reported)
2. Fail to generate a fix because:
   - The `visit_expr` method only matched `Stmt::Assign`, not `Stmt::AnnAssign`
   - The `generate_fix` function only accepted `Stmt::Assign` in its body validation
   - The replacement code generation didn't account for type annotations

This occurred because Python's AST represents annotated assignments as a different node type (`StmtAnnAssign`) with separate fields for the target, annotation, and value, unlike regular assignments which use a list of targets.

## Approach

The fix extends the rule to handle both assignment types:

1. **Updated `visit_expr` method**: Now matches both `Stmt::Assign` and `Stmt::AnnAssign`, extracting:
   - Variable name from the target expression
   - Type annotation code (when present) using the code generator

2. **Updated `generate_fix` function**:
   - Added `annotation: Option<String>` parameter to accept annotation code
   - Updated body validation to accept both `Stmt::Assign` and `Stmt::AnnAssign`
   - Modified replacement code generation to preserve annotations: `{var}: {annotation} = {binding}({filename_code}).{suggestion}`

3. **Added test case**: Added an annotated assignment test case to verify the fix works correctly.

The implementation maintains backward compatibility with regular assignments while adding support for annotated assignments, ensuring type annotations are preserved in the generated fixes.

---

_Comment by @github-actions[bot] on 2025-11-05 06:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +14 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a630926a18730f3adcfccb619a5904509c17d6bc/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L1005'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:1005:18:</a> FURB101 [*] `open` and `read` should be replaced by `Path(self.waiter_path).read_text()`
- <a href='https://github.com/apache/airflow/blob/a630926a18730f3adcfccb619a5904509c17d6bc/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L1005'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:1005:18:</a> FURB101 `open` and `read` should be replaced by `Path(self.waiter_path).read_text()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/export.py#L1732'>zerver/lib/export.py:1732:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(input_path).read_bytes()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/export.py#L1732'>zerver/lib/export.py:1732:10:</a> FURB101 `open` and `read` should be replaced by `Path(input_path).read_bytes()`
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/import_realm.py#L1205'>zerver/lib/import_realm.py:1205:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(migration_status_filename).read_text()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/import_realm.py#L1205'>zerver/lib/import_realm.py:1205:10:</a> FURB101 `open` and `read` should be replaced by `Path(migration_status_filename).read_text()`
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/import_realm.py#L996'>zerver/lib/import_realm.py:996:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(records_filename).read_bytes()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/import_realm.py#L996'>zerver/lib/import_realm.py:996:10:</a> FURB101 `open` and `read` should be replaced by `Path(records_filename).read_bytes()`
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/management.py#L282'>zerver/lib/management.py:282:18:</a> FURB101 [*] `open` and `read` should be replaced by `Path(options["password_file"]).read_text()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/lib/management.py#L282'>zerver/lib/management.py:282:18:</a> FURB101 `open` and `read` should be replaced by `Path(options["password_file"]).read_text()`
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/management/commands/send_custom_email.py#L118'>zerver/management/commands/send_custom_email.py:118:22:</a> FURB101 [*] `open` and `read` should be replaced by `Path(options["json_file"]).read_text()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/management/commands/send_custom_email.py#L118'>zerver/management/commands/send_custom_email.py:118:22:</a> FURB101 `open` and `read` should be replaced by `Path(options["json_file"]).read_text()`
+ <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/management/commands/send_custom_email.py#L233'>zerver/management/commands/send_custom_email.py:233:18:</a> FURB101 [*] `open` and `read` should be replaced by `Path(options["json_file"]).read_text()`
- <a href='https://github.com/zulip/zulip/blob/663dfc3ff99a4f65d8105c04c7ae5588fc23161c/zerver/management/commands/send_custom_email.py#L233'>zerver/management/commands/send_custom_email.py:233:18:</a> FURB101 `open` and `read` should be replaced by `Path(options["json_file"]).read_text()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB101 | 14 | 0 | 0 | 14 | 0 |

</p>
</details>




---

_Label `fixes` added by @ntBre on 2025-11-06 18:40_

---

_Label `preview` added by @ntBre on 2025-11-06 18:40_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:132 on 2025-11-06 18:55_

I should have noticed this in the earlier PR, but this actually looks quite repetitive with the check inside of `generate_fix`. I think we can combine this check and this check:

```rust
matches!(with_stmt.body.as_slice(), [Stmt::Assign(_)]))
```

`generate_fix` seems like the right place to do that to me, which should allow us to decrease the number of arguments since we're already passing the `with_stmt` itself.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:152 on 2025-11-06 18:59_

Do we need to use the generator here, or can we just slice the `&str` out of the input?


```suggestion
                                let annotation_code = self
                                    .checker
                                    .locator()
                                    .slice(ann_assign.annotation.range());
```

(untested but that's what I had in mind)

---

_@ntBre reviewed on 2025-11-06 19:02_

Thanks, the results look good here, including the ecosystem check. I just had a couple of suggestions for simplification.

---

_Review requested from @ntBre by @danparizher on 2025-11-06 21:48_

---

_Comment by @ntBre on 2025-11-08 00:03_

I merged the conflicting test changes I made in another PR, simplified the implementation a bit further, and then fixed a couple of preexisting bugs I noticed. The ecosystem comment doesn't seem to be updating, but I downloaded the artifact and checked that instead. It's showing -188 fixes, and from the ones I've clicked on this seems correct. The two bugs I fixed were that the `assign.targets.first()` check wasn't exactly right because it could drop other elements in an assignment like:

```py
content, x = f.read(), 2
```

and the `contains_range` checks also weren't correct. `contains_range` is true for an expression like:

```py
content = process_contents(f.read())
```

but we were then replacing the entire assignment statement and discarding the `process_contents` call. All of the ecosystem changes I've seen so far fit into this latter pattern.

The fix is now fairly restricted. We may want to consider expanding it in the future by doing a more targeted replacement of the `read` call expression itself instead of re-building the whole assignment as a string. But I think the current state of the PR is a substantial enough improvement to merge as-is.

---

_@ntBre approved on 2025-11-08 00:03_

---

_Renamed from "[`refurb`] Fix annotated assignments blocking FURB101 fix (`FURB101`)" to "[`refurb`] Auto-fix annotated assignments (`FURB101`)" by @ntBre on 2025-11-08 00:04_

---

_Merged by @ntBre on 2025-11-08 00:04_

---

_Closed by @ntBre on 2025-11-08 00:04_

---

_Branch deleted on 2025-11-08 00:05_

---
