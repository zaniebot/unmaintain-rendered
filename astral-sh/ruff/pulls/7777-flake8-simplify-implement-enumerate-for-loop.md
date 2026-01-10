```yaml
number: 7777
title: "[`flake8-simplify`] Implement `enumerate-for-loop` (`SIM113`)"
type: pull_request
state: merged
author: chammika-become
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: simplify-SIM113
created_at: 2023-10-03T12:38:27Z
updated_at: 2024-01-14T16:00:59Z
url: https://github.com/astral-sh/ruff/pull/7777
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-simplify`] Implement `enumerate-for-loop` (`SIM113`)

---

_Pull request opened by @chammika-become on 2023-10-03 12:38_

Implements SIM113 from #998

Added tests
Limitations 
   - No fix yet
   - Only flag cases where index variable immediately precede `for` loop

@charliermarsh please review and let me know any improvements


---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM113.py`:1 on 2023-10-09 08:26_

good test cases!

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:24 on 2023-10-09 08:30_

```suggestion
///    idx += 1
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:31 on 2023-10-09 08:30_

```suggestion
///     sum += func(item, idx)
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:69 on 2023-10-09 08:38_

```suggestion
        let diagnostic = Diagnostic::new(
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:80 on 2023-10-09 08:40_

I think you need to use `StatementVisitor` here instead. Using the visitor means that all the different cases are handled in one central place (so you don't need to test them here) and it also makes updating to new python versions which introduce new statements (or changes to our AST independent of python) much easier.

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:142 on 2023-10-09 08:52_

You should be able to directly match on `Constant::Int(Int::ZERO)`

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:157 on 2023-10-09 08:54_

Does 
```suggestion
        target, op: Operator::Add, value, ..
```
work?

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:127 on 2023-10-09 09:12_

This way does work, but i think it's better to look the binding of the name up in the semantic model and then check if it's in the same suite as the for and set to zero.

---

_@konstin reviewed on 2023-10-09 09:12_

---

_@ncoish reviewed on 2023-11-20 15:54_

---

_Review comment by @ncoish on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for.rs`:45 on 2023-11-20 15:54_

Small typo in the output recommendation, but an important one since it's the name of the replacement function.

The snapshots need this change too.

```suggestion
        format!("Use enumerate() for index variable `{index_var}` in for loop")
```

---

_Comment by @danieleades on 2024-01-13 04:22_

any progress on this?

---

_Comment by @charliermarsh on 2024-01-14 02:50_

@danieleades - Na, but I can take it over.

---

_Comment by @codspeed-hq[bot] on 2024-01-14 03:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/chammika-become:simplify-SIM113)

### Merging #7777 will **degrade performances by 4.35%**

<sub>Comparing <code>chammika-become:simplify-SIM113</code> (5c25d68) with <code>main</code> (b6ce4f5)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/chammika-become:simplify-SIM113)._

### Benchmarks breakdown

|     | Benchmark | `main` | `chammika-become:simplify-SIM113` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-14 03:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+14 -0 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1455a3babb1bf4b890562a65610b33c0db206f69/airflow/cli/commands/webserver_command.py#L171'>airflow/cli/commands/webserver_command.py:171:13:</a> SIM113 Use `enumerate()` for index variable `excess` in `for` loop
+ <a href='https://github.com/apache/airflow/blob/1455a3babb1bf4b890562a65610b33c0db206f69/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L180'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:180:13:</a> SIM113 Use `enumerate()` for index variable `counter` in `for` loop
+ <a href='https://github.com/apache/airflow/blob/1455a3babb1bf4b890562a65610b33c0db206f69/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L202'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:202:13:</a> SIM113 Use `enumerate()` for index variable `counter` in `for` loop
+ <a href='https://github.com/apache/airflow/blob/1455a3babb1bf4b890562a65610b33c0db206f69/scripts/ci/pre_commit/pre_commit_update_providers_dependencies.py#L317'>scripts/ci/pre_commit/pre_commit_update_providers_dependencies.py:317:9:</a> SIM113 Use `enumerate()` for index variable `line_count` in `for` loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/import_util.py#L643'>zerver/data_import/import_util.py:643:13:</a> SIM113 Use `enumerate()` for index variable `count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L634'>zerver/data_import/slack.py:634:9:</a> SIM113 Use `enumerate()` for index variable `recipient_id_count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L635'>zerver/data_import/slack.py:635:9:</a> SIM113 Use `enumerate()` for index variable `subscription_id_count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/data_import/slack.py#L745'>zerver/data_import/slack.py:745:13:</a> SIM113 Use `enumerate()` for index variable `_counter` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/lib/export.py#L1744'>zerver/lib/export.py:1744:9:</a> SIM113 Use `enumerate()` for index variable `count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/lib/export.py#L1865'>zerver/lib/export.py:1865:9:</a> SIM113 Use `enumerate()` for index variable `count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/lib/import_realm.py#L791'>zerver/lib/import_realm.py:791:9:</a> SIM113 Use `enumerate()` for index variable `count` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/migrations/0177_user_message_add_and_index_is_private_flag.py#L42'>zerver/migrations/0177_user_message_add_and_index_is_private_flag.py:42:9:</a> SIM113 Use `enumerate()` for index variable `i` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zerver/tests/test_message_edit.py#L1012'>zerver/tests/test_message_edit.py:1012:13:</a> SIM113 Use `enumerate()` for index variable `i` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/a4fad5dda116048e22c3d559d370367e4e1c9b9a/zilencer/management/commands/populate_db.py#L705'>zilencer/management/commands/populate_db.py:705:17:</a> SIM113 Use `enumerate()` for index variable `i` in `for` loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM113 | 14 | 14 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-01-14 03:42_

(Taking over.)

---

_Renamed from "flake8_simplify : SIM113" to "[`flake8-simplify`] Implement `enumerate-for-loop` (`SIM113`)" by @charliermarsh on 2024-01-14 04:16_

---

_Label `rule` added by @charliermarsh on 2024-01-14 04:16_

---

_Label `preview` added by @charliermarsh on 2024-01-14 04:16_

---

_Comment by @danieleades on 2024-01-14 04:20_

> @danieleades - Na, but I can take it over.

I only ask because I think this lint is the last hold-out for deprecating flake8 linting from the sphinx project!

---

_Comment by @charliermarsh on 2024-01-14 04:21_

😲 😲 😲 

---

_Comment by @charliermarsh on 2024-01-14 04:23_

Should be good to go, also addressed some false positives that exist in the upstream implementation.

---

_Comment by @charliermarsh on 2024-01-14 13:55_

Looking at the ecosystem checks, there are a few more false positives we can fix here.

---

_Comment by @charliermarsh on 2024-01-14 13:57_

Namely:

- Ensure that the initialization and the `for` loop have the same parent (not just the same branch).
- If the variable is used _after_ the loop, consider allowing it? As in:

```python
count = 0
for entry in result:
    assert 'thing' in entry[0]
    count += 1
assert count == 9, 'only 9 query ranges should remain in the DB'
```

There's a lot of this in tests, which seems ok? I could go either way. You can also write as:

```python
for count, entry in enumerate(result):
    assert 'thing' in entry[0]
assert count == 9, 'only 9 query ranges should remain in the DB'
```

---

_Merged by @charliermarsh on 2024-01-14 16:00_

---

_Closed by @charliermarsh on 2024-01-14 16:00_

---
