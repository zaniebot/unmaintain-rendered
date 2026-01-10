```yaml
number: 16149
title: "[`flake8-self`] Ignore attribute accesses on instance-like variables (`SLF001`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: SLF001
created_at: 2025-02-14T01:43:10Z
updated_at: 2025-02-23T11:36:42Z
url: https://github.com/astral-sh/ruff/pull/16149
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-self`] Ignore attribute accesses on instance-like variables (`SLF001`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-14 01:43_

## Summary

Resolves #9022.

`SLF001` now recognizes the following as instance-like and will not report them:

* `cls()`/`mcs()`
* `super().__new__()`
* `SameClass()`/`SameClass[int]()`/`Annotated[SameClass[int], ...]()`
* `a: SameClass`/`a: SameClass[int]`/`a: Annotated[SameClass[int], ...]`

The new logic prioritizes avoiding false positives over avoiding false negatives. As the issue is labeled as a bug, the changes are not preview-gated.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-14 01:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -14 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/io/path.py#L365'>airflow/io/path.py:365:17:</a> SLF001 Private member accessed: `_cp_file`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/serialized_dag.py#L218'>airflow/models/serialized_dag.py:218:36:</a> SLF001 Private member accessed: `_data`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/serialized_dag.py#L219'>airflow/models/serialized_dag.py:219:47:</a> SLF001 Private member accessed: `_data_compressed`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/taskinstance.py#L3084'>airflow/models/taskinstance.py:3084:84:</a> SLF001 Private member accessed: `_execute_task`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/providers/databricks/src/airflow/providers/databricks/hooks/databricks.py#L716'>providers/databricks/src/airflow/providers/databricks/hooks/databricks.py:716:13:</a> SLF001 Private member accessed: `_do_api_call`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/task_sdk/src/airflow/sdk/execution_time/supervisor.py#L375'>task_sdk/src/airflow/sdk/execution_time/supervisor.py:375:9:</a> SLF001 Private member accessed: `_register_pipe_readers`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L127'>superset/migrations/shared/migrate_viz/base.py:127:9:</a> SLF001 Private member accessed: `_pre_action`
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L128'>superset/migrations/shared/migrate_viz/base.py:128:9:</a> SLF001 Private member accessed: `_migrate`
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L129'>superset/migrations/shared/migrate_viz/base.py:129:9:</a> SLF001 Private member accessed: `_post_action`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L105'>src/bokeh/model/model.py:105:13:</a> SLF001 Private member accessed: `_id`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L107'>src/bokeh/model/model.py:107:13:</a> SLF001 Private member accessed: `_id`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L200'>src/bokeh/protocol/message.py:200:9:</a> SLF001 Private member accessed: `_header_json`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L201'>src/bokeh/protocol/message.py:201:9:</a> SLF001 Private member accessed: `_metadata_json`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L202'>src/bokeh/protocol/message.py:202:9:</a> SLF001 Private member accessed: `_content_json`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 14 | 0 | 14 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -14 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/io/path.py#L365'>airflow/io/path.py:365:17:</a> SLF001 Private member accessed: `_cp_file`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/serialized_dag.py#L218'>airflow/models/serialized_dag.py:218:36:</a> SLF001 Private member accessed: `_data`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/serialized_dag.py#L219'>airflow/models/serialized_dag.py:219:47:</a> SLF001 Private member accessed: `_data_compressed`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/airflow/models/taskinstance.py#L3084'>airflow/models/taskinstance.py:3084:84:</a> SLF001 Private member accessed: `_execute_task`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/providers/databricks/src/airflow/providers/databricks/hooks/databricks.py#L716'>providers/databricks/src/airflow/providers/databricks/hooks/databricks.py:716:13:</a> SLF001 Private member accessed: `_do_api_call`
- <a href='https://github.com/apache/airflow/blob/946a62aa835ab10bd83343bcdeb8354d7bbfc7c6/task_sdk/src/airflow/sdk/execution_time/supervisor.py#L375'>task_sdk/src/airflow/sdk/execution_time/supervisor.py:375:9:</a> SLF001 Private member accessed: `_register_pipe_readers`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L127'>superset/migrations/shared/migrate_viz/base.py:127:9:</a> SLF001 Private member accessed: `_pre_action`
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L128'>superset/migrations/shared/migrate_viz/base.py:128:9:</a> SLF001 Private member accessed: `_migrate`
- <a href='https://github.com/apache/superset/blob/2c37ddb2f63d216665c8d81232500986281ccfc0/superset/migrations/shared/migrate_viz/base.py#L129'>superset/migrations/shared/migrate_viz/base.py:129:9:</a> SLF001 Private member accessed: `_post_action`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L105'>src/bokeh/model/model.py:105:13:</a> SLF001 Private member accessed: `_id`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L107'>src/bokeh/model/model.py:107:13:</a> SLF001 Private member accessed: `_id`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L200'>src/bokeh/protocol/message.py:200:9:</a> SLF001 Private member accessed: `_header_json`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L201'>src/bokeh/protocol/message.py:201:9:</a> SLF001 Private member accessed: `_metadata_json`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/protocol/message.py#L202'>src/bokeh/protocol/message.py:202:9:</a> SLF001 Private member accessed: `_content_json`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SLF001 | 14 | 0 | 14 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @InSyncWithFoo on 2025-02-14 02:00_

Ecosystem changes are all as expected.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_self/rules/private_member_access.rs`:155 on 2025-02-14 22:32_

This is where the new code starts right? Everything above looked like a (good) refactoring, but I want to make sure I focus on the right parts.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_self/rules/private_member_access.rs`:203 on 2025-02-14 22:49_

This is the first time I've looked at this `TypeChecker` API, but it looks comparable to other uses. It also makes sense to me to make the `check_type` function `pub` instead of putting all of these structs in the `typing` module.

---

_@ntBre reviewed on 2025-02-14 23:04_

Thanks! This looks good to me, but I'd like someone more familiar with the `TypeChecker` API to have a look too. Maybe @AlexWaygood or @MichaReiser?

---

_Label `bug` added by @ntBre on 2025-02-14 23:04_

---

_@InSyncWithFoo reviewed on 2025-02-14 23:55_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_self/rules/private_member_access.rs`:155 on 2025-02-14 23:55_

This and the `if` block on line 141, yes.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_self/rules/private_member_access.rs`:156 on 2025-02-17 09:53_

Would it be possible to unify this method with `is_known_dunder_method`?

---

_@MichaReiser approved on 2025-02-17 10:03_

I think this overall is an improvement. Will this be a deviation from the upstream rule?

---

_@InSyncWithFoo reviewed on 2025-02-17 10:57_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_self/rules/private_member_access.rs`:156 on 2025-02-17 10:57_

No, but similar patterns can be deduplicated. This one only checks for operators specifically, since it is often the case that such methods have an `other` argument of the same type.

---

_Comment by @InSyncWithFoo on 2025-02-17 11:03_

@MichaReiser This would be a deviation, yes, but the rule already contains multiple such deviations. [The upstream rule](https://github.com/Korijn/flake8-self/blob/f5fc507515eaa895f56b0bdabba0041fec8af1a4/flake8_self.py) exempts `self`, `cls`, `mcs` but nothing else.

---

_@MichaReiser approved on 2025-02-23 10:00_

---

_Merged by @MichaReiser on 2025-02-23 10:00_

---

_Closed by @MichaReiser on 2025-02-23 10:00_

---

_Branch deleted on 2025-02-23 11:36_

---
