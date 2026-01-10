```yaml
number: 14214
title: "[`flake8-pyi`] Implement autofix and handle nested unions with single element (`PYI041 `, `PYI055`)"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
draft: true
base: main
head: pyi-nested-union-fix
created_at: 2024-11-08T23:01:11Z
updated_at: 2024-11-11T14:00:45Z
url: https://github.com/astral-sh/ruff/pull/14214
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`] Implement autofix and handle nested unions with single element (`PYI041 `, `PYI055`)

---

_Pull request opened by @sbrugman on 2024-11-08 23:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

PR to implement autofix for `redundant-numeric-union` (`PYI041`) and `duplication-union-member` (`PYI016`)
https://github.com/astral-sh/ruff/issues/14185

There appears to be an issue with the nested union detection when the union is unary:

```python-console
>>> from typing import Union
>>> Union[int | int | float]
typing.Union[int, float]

>>> Union[Union[float, complex]]
typing.Union[float, complex]
```

This issue also affects at least `PYI055`.

Edit: the PR grew a bit bigger after finding issues with fix safety. I'll break up the PR into multiple for easier review.

## Test Plan

Extension of the test cases and `cargo test`

---

_@sbrugman reviewed on 2024-11-08 23:05_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:68 on 2024-11-08 23:05_

Emitting multiple messages for the same union, such as "Use `complex` instead of `int | complex`" and  "Use `complex` instead of `float | complex`" gets confusing when there is a fix presented:

```diff
-    def bad4(self, arg: Union[float | complex, int]) -> None: ...  # PYI041
+    def bad4(self, arg: complex) -> None: ...  # PYI041
```

With `IntFloatComplex` we can assign an unique violation for each combination.

---

_@sbrugman reviewed on 2024-11-08 23:07_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:90 on 2024-11-08 23:07_

Instead of `has_*`, I've used `bitflags` (which are used in some other parts of the code already).
(They correspond naturally to the order of Complex > Float > Int)

---

_@sbrugman reviewed on 2024-11-08 23:14_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:106 on 2024-11-08 23:14_

I've chosen to do a second loop only when there is a violation, instead of collecting all expressions for any Union and filtering them later to avoid overhead in those cases.

---

_Comment by @github-actions[bot] on 2024-11-08 23:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +74 -0 fixes in 5 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +58 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/datadog_logger.py#L113'>airflow/metrics/datadog_logger.py:113:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/datadog_logger.py#L113'>airflow/metrics/datadog_logger.py:113:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/otel_logger.py#L246'>airflow/metrics/otel_logger.py:246:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/otel_logger.py#L246'>airflow/metrics/otel_logger.py:246:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:72:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:72:</a> PYI041 [*] Use `float` instead of `int | float`
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 Duplicate union member `str`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 [*] Duplicate union member `str`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 [*] Use `float` instead of `int | float`
... 3 additional changes omitted for rule PYI041
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 Duplicate union member `tuple[str, str]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 [*] Duplicate union member `tuple[str, str]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 Duplicate union member `tp.Sequence[tuple[str, str]]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 [*] Duplicate union member `tp.Sequence[tuple[str, str]]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 Duplicate union member `ContourColor`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 [*] Duplicate union member `ContourColor`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/statistics.pyi#L160'>stdlib/statistics.pyi:160:15:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 67 | 0 | 1 | 66 | 0 |
| PYI016 | 8 | 0 | 0 | 8 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +74 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +58 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/datadog_logger.py#L113'>airflow/metrics/datadog_logger.py:113:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/datadog_logger.py#L113'>airflow/metrics/datadog_logger.py:113:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/otel_logger.py#L246'>airflow/metrics/otel_logger.py:246:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/otel_logger.py#L246'>airflow/metrics/otel_logger.py:246:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/02217ccf5c6a8a38446ad6145bcaccd615712c8f/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 [*] Use `float` instead of `int | float`
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 Duplicate union member `str`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/models/helpers.py#L900'>superset/models/helpers.py:900:39:</a> PYI016 [*] Duplicate union member `str`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/24b8a94c2ce4d21c65caa076bdc54ba61635be85/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 [*] Use `float` instead of `int | float`
... 3 additional changes omitted for rule PYI041
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 Duplicate union member `tuple[str, str]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 [*] Duplicate union member `tuple[str, str]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 Duplicate union member `tp.Sequence[tuple[str, str]]`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 [*] Duplicate union member `tp.Sequence[tuple[str, str]]`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 Duplicate union member `ContourColor`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 [*] Duplicate union member `ContourColor`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/statistics.pyi#L160'>stdlib/statistics.pyi:160:15:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/turtle.pyi#L443'>stdlib/turtle.pyi:443:16:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/turtle.pyi#L444'>stdlib/turtle.pyi:444:17:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 71 | 0 | 5 | 66 | 0 |
| PYI016 | 8 | 0 | 0 | 8 | 0 |

</p>
</details>




---

_@sbrugman reviewed on 2024-11-08 23:15_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:158 on 2024-11-08 23:15_

There must be a more elegant way of doing this, but I haven't found it yet.

---

_@sbrugman reviewed on 2024-11-08 23:17_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:230 on 2024-11-08 23:17_

The code above handles `nodes.len() < 2` separately and thus this function should never encounter these cases. Is `debug_assert!` ok to denote this?

---

_@sbrugman reviewed on 2024-11-08 23:19_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:78 on 2024-11-08 23:19_

Rather than using the same Union syntax as the top-level union, I check if _any_ PEP604-style union is encountered. If so, then we use that. (`a | b` or `typing.Union[a, b]`)

---

_Comment by @sbrugman on 2024-11-11 14:00_

Raises as separate PRs to reduce the review load in #14280, #14275, #14273, #14272, #14270 and #14268.

---

_Closed by @sbrugman on 2024-11-11 14:00_

---
