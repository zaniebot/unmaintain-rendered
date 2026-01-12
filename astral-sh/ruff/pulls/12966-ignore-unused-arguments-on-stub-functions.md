```yaml
number: 12966
title: Ignore unused arguments on stub functions
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/stu
created_at: 2024-08-18T22:55:33Z
updated_at: 2024-08-18T23:21:35Z
url: https://github.com/astral-sh/ruff/pull/12966
synced_at: 2026-01-12T15:55:42Z
```

# Ignore unused arguments on stub functions

---

_@charliermarsh_

## Summary

We already enforce this logic for the other `ARG` rules. I'm guessing this was an oversight.

Closes https://github.com/astral-sh/ruff/issues/12963.


---

_Label `rule` added by @charliermarsh on 2024-08-18 22:55_

---

_Comment by @codspeed-hq[bot] on 2024-08-18 23:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/stu)

### Merging #12966 will **degrade performances by 5.17%**

<sub>Comparing <code>charlie/stu</code> (ce7df06) with <code>main</code> (4881d32)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/stu)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/stu` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 727 µs | 766.6 µs | -5.17% |


---

_Comment by @github-actions[bot] on 2024-08-18 23:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -289 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -177 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L31'>airflow/listeners/spec/dagrun.py:31:24:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L31'>airflow/listeners/spec/dagrun.py:31:41:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L36'>airflow/listeners/spec/dagrun.py:36:24:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L36'>airflow/listeners/spec/dagrun.py:36:41:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L41'>airflow/listeners/spec/dagrun.py:41:23:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L41'>airflow/listeners/spec/dagrun.py:41:40:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dataset.py#L32'>airflow/listeners/spec/dataset.py:32:5:</a> ARG001 Unused function argument: `dataset`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dataset.py#L39'>airflow/listeners/spec/dataset.py:39:5:</a> ARG001 Unused function argument: `dataset`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L26'>airflow/listeners/spec/importerrors.py:26:29:</a> ARG001 Unused function argument: `filename`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L26'>airflow/listeners/spec/importerrors.py:26:39:</a> ARG001 Unused function argument: `stacktrace`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L31'>airflow/listeners/spec/importerrors.py:31:34:</a> ARG001 Unused function argument: `filename`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L31'>airflow/listeners/spec/importerrors.py:31:44:</a> ARG001 Unused function argument: `stacktrace`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/lifecycle.py#L26'>airflow/listeners/spec/lifecycle.py:26:17:</a> ARG001 Unused function argument: `component`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/lifecycle.py#L37'>airflow/listeners/spec/lifecycle.py:37:21:</a> ARG001 Unused function argument: `component`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:47:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:76:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:47:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:76:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L49'>airflow/listeners/spec/taskinstance.py:49:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L50'>airflow/listeners/spec/taskinstance.py:50:5:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L51'>airflow/listeners/spec/taskinstance.py:51:5:</a> ARG001 Unused function argument: `error`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L52'>airflow/listeners/spec/taskinstance.py:52:5:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L109'>airflow/policies.py:109:31:</a> ARG001 Unused function argument: `dag_file_path`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L36'>airflow/policies.py:36:17:</a> ARG001 Unused function argument: `task`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L54'>airflow/policies.py:54:16:</a> ARG001 Unused function argument: `dag`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L72'>airflow/policies.py:72:33:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L83'>airflow/policies.py:83:23:</a> ARG001 Unused function argument: `pod`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L96'>airflow/policies.py:96:30:</a> ARG001 Unused function argument: `context`
... 147 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:27:</a> ARG001 Unused function argument: `col`
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:40:</a> ARG001 Unused function argument: `op`
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:60:</a> ARG001 Unused function argument: `values`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -77 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:29:</a> ARG001 Unused function argument: `config`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:45:</a> ARG001 Unused function argument: `system`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:64:</a> ARG001 Unused function argument: `token`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:31:</a> ARG001 Unused function argument: `config`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:47:</a> ARG001 Unused function argument: `system`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:66:</a> ARG001 Unused function argument: `token`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L135'>src/bokeh/application/handlers/lifecycle.py:135:17:</a> ARG001 Unused function argument: `ignored`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L49'>src/bokeh/core/has_props.py:49:19:</a> ARG001 Unused function argument: `arg`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/example_handler.py#L85'>src/bokeh/sphinxext/example_handler.py:85:20:</a> ARG001 Unused function argument: `args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/example_handler.py#L85'>src/bokeh/sphinxext/example_handler.py:85:28:</a> ARG001 Unused function argument: `kw`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/terminal.py#L72'>src/bokeh/util/terminal.py:72:12:</a> ARG001 Unused function argument: `values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/terminal.py#L72'>src/bokeh/util/terminal.py:72:27:</a> ARG001 Unused function argument: `kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_document_lifecycle.py#L57'>tests/unit/bokeh/application/handlers/test_document_lifecycle.py:57:21:</a> ARG001 Unused function argument: `a`
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/170e7aaeb91807dccbade786f95e8c8e4d2eda31/unit_test/main_tests/conftest.py#L50'>unit_test/main_tests/conftest.py:50:50:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8cbd92f430c20ad5e0d9725198a1397ab599437d/tests/test_get_requires.py#L23'>tests/test_get_requires.py:23:64:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -32 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/markdown/__init__.py#L419'>zerver/lib/markdown/__init__.py:419:22:</a> ARG001 Unused function argument: `tweet_id`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/partial.py#L36'>zerver/lib/partial.py:36:17:</a> ARG001 Unused function argument: `func`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/partial.py#L36'>zerver/lib/partial.py:36:45:</a> ARG001 Unused function argument: `args`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/push_notifications.py#L179'>zerver/lib/push_notifications.py:179:47:</a> ARG001 Unused function argument: `result`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/push_notifications.py#L179'>zerver/lib/push_notifications.py:179:9:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L319'>zerver/tests/test_typed_endpoint.py:319:13:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L321'>zerver/tests/test_typed_endpoint.py:321:13:</a> ARG001 Unused function argument: `path_var_default`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L331'>zerver/tests/test_typed_endpoint.py:331:13:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L333'>zerver/tests/test_typed_endpoint.py:333:13:</a> ARG001 Unused function argument: `foo`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L337'>zerver/tests/test_typed_endpoint.py:337:13:</a> ARG001 Unused function argument: `bar`
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG001 | 289 | 0 | 289 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -289 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -177 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L31'>airflow/listeners/spec/dagrun.py:31:24:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L31'>airflow/listeners/spec/dagrun.py:31:41:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L36'>airflow/listeners/spec/dagrun.py:36:24:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L36'>airflow/listeners/spec/dagrun.py:36:41:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L41'>airflow/listeners/spec/dagrun.py:41:23:</a> ARG001 Unused function argument: `dag_run`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dagrun.py#L41'>airflow/listeners/spec/dagrun.py:41:40:</a> ARG001 Unused function argument: `msg`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dataset.py#L32'>airflow/listeners/spec/dataset.py:32:5:</a> ARG001 Unused function argument: `dataset`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/dataset.py#L39'>airflow/listeners/spec/dataset.py:39:5:</a> ARG001 Unused function argument: `dataset`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L26'>airflow/listeners/spec/importerrors.py:26:29:</a> ARG001 Unused function argument: `filename`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L26'>airflow/listeners/spec/importerrors.py:26:39:</a> ARG001 Unused function argument: `stacktrace`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L31'>airflow/listeners/spec/importerrors.py:31:34:</a> ARG001 Unused function argument: `filename`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/importerrors.py#L31'>airflow/listeners/spec/importerrors.py:31:44:</a> ARG001 Unused function argument: `stacktrace`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/lifecycle.py#L26'>airflow/listeners/spec/lifecycle.py:26:17:</a> ARG001 Unused function argument: `component`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/lifecycle.py#L37'>airflow/listeners/spec/lifecycle.py:37:21:</a> ARG001 Unused function argument: `component`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:47:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L35'>airflow/listeners/spec/taskinstance.py:35:76:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:47:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L42'>airflow/listeners/spec/taskinstance.py:42:76:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L49'>airflow/listeners/spec/taskinstance.py:49:5:</a> ARG001 Unused function argument: `previous_state`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L50'>airflow/listeners/spec/taskinstance.py:50:5:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L51'>airflow/listeners/spec/taskinstance.py:51:5:</a> ARG001 Unused function argument: `error`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/listeners/spec/taskinstance.py#L52'>airflow/listeners/spec/taskinstance.py:52:5:</a> ARG001 Unused function argument: `session`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L109'>airflow/policies.py:109:31:</a> ARG001 Unused function argument: `dag_file_path`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L36'>airflow/policies.py:36:17:</a> ARG001 Unused function argument: `task`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L54'>airflow/policies.py:54:16:</a> ARG001 Unused function argument: `dag`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L72'>airflow/policies.py:72:33:</a> ARG001 Unused function argument: `task_instance`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L83'>airflow/policies.py:83:23:</a> ARG001 Unused function argument: `pod`
- <a href='https://github.com/apache/airflow/blob/b0fac0e09d79e6e44e9f69a7b91010e0c5c30872/airflow/policies.py#L96'>airflow/policies.py:96:30:</a> ARG001 Unused function argument: `context`
... 147 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:27:</a> ARG001 Unused function argument: `col`
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:40:</a> ARG001 Unused function argument: `op`
- <a href='https://github.com/apache/superset/blob/6e1ef193dd788e6847b77c4b725868aafb0b928f/tests/integration_tests/advanced_data_type/api_tests.py#L56'>tests/integration_tests/advanced_data_type/api_tests.py:56:60:</a> ARG001 Unused function argument: `values`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -77 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:29:</a> ARG001 Unused function argument: `config`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:45:</a> ARG001 Unused function argument: `system`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L75'>release/credentials.py:75:64:</a> ARG001 Unused function argument: `token`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:31:</a> ARG001 Unused function argument: `config`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:47:</a> ARG001 Unused function argument: `system`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L82'>release/credentials.py:82:66:</a> ARG001 Unused function argument: `token`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/lifecycle.py#L135'>src/bokeh/application/handlers/lifecycle.py:135:17:</a> ARG001 Unused function argument: `ignored`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L49'>src/bokeh/core/has_props.py:49:19:</a> ARG001 Unused function argument: `arg`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/example_handler.py#L85'>src/bokeh/sphinxext/example_handler.py:85:20:</a> ARG001 Unused function argument: `args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/example_handler.py#L85'>src/bokeh/sphinxext/example_handler.py:85:28:</a> ARG001 Unused function argument: `kw`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/terminal.py#L72'>src/bokeh/util/terminal.py:72:12:</a> ARG001 Unused function argument: `values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/terminal.py#L72'>src/bokeh/util/terminal.py:72:27:</a> ARG001 Unused function argument: `kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_document_lifecycle.py#L57'>tests/unit/bokeh/application/handlers/test_document_lifecycle.py:57:21:</a> ARG001 Unused function argument: `a`
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/170e7aaeb91807dccbade786f95e8c8e4d2eda31/unit_test/main_tests/conftest.py#L50'>unit_test/main_tests/conftest.py:50:50:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8cbd92f430c20ad5e0d9725198a1397ab599437d/tests/test_get_requires.py#L23'>tests/test_get_requires.py:23:64:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -32 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/markdown/__init__.py#L419'>zerver/lib/markdown/__init__.py:419:22:</a> ARG001 Unused function argument: `tweet_id`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/partial.py#L36'>zerver/lib/partial.py:36:17:</a> ARG001 Unused function argument: `func`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/partial.py#L36'>zerver/lib/partial.py:36:45:</a> ARG001 Unused function argument: `args`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/push_notifications.py#L179'>zerver/lib/push_notifications.py:179:47:</a> ARG001 Unused function argument: `result`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/lib/push_notifications.py#L179'>zerver/lib/push_notifications.py:179:9:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L319'>zerver/tests/test_typed_endpoint.py:319:13:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L321'>zerver/tests/test_typed_endpoint.py:321:13:</a> ARG001 Unused function argument: `path_var_default`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L331'>zerver/tests/test_typed_endpoint.py:331:13:</a> ARG001 Unused function argument: `request`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L333'>zerver/tests/test_typed_endpoint.py:333:13:</a> ARG001 Unused function argument: `foo`
- <a href='https://github.com/zulip/zulip/blob/a3806b416534ffa6e8d89b9a3182035d80aa3856/zerver/tests/test_typed_endpoint.py#L337'>zerver/tests/test_typed_endpoint.py:337:13:</a> ARG001 Unused function argument: `bar`
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG001 | 289 | 0 | 289 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-08-18 23:21_

---

_Closed by @charliermarsh on 2024-08-18 23:21_

---

_Branch deleted on 2024-08-18 23:21_

---
