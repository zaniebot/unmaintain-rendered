```yaml
number: 15343
title: "[`flake8-pyi`] Stabilize autofix for `redundant-numeric-union` (`PYI041`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - fixes
assignees: []
merged: true
base: ruff-0.9
head: micha/pyi041-fix
created_at: 2025-01-08T09:53:38Z
updated_at: 2025-01-08T11:08:56Z
url: https://github.com/astral-sh/ruff/pull/15343
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-pyi`] Stabilize autofix for `redundant-numeric-union` (`PYI041`)

---

_Pull request opened by @MichaReiser on 2025-01-08 09:53_

## Summary

Stabilizes the autofix introduced in https://github.com/astral-sh/ruff/pull/14273

## Test Plan

There are no open issues concerning this fix. ~~There are two open issues related to the rule:~~

* https://github.com/astral-sh/ruff/issues/9810 asks to extend the rule to include `bool`. This should not block stabilization because it's an extension of the rule
* https://github.com/astral-sh/ruff/issues/14185 is the issue that requested adding this specific fix. This should definitely not block stabilization ;)


---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 09:53_

---

_Label `fixes` added by @MichaReiser on 2025-01-08 09:56_

---

_Comment by @github-actions[bot] on 2025-01-08 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +80 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +62 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/exceptions.py#L419'>airflow/exceptions.py:419:18:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/exceptions.py#L419'>airflow/exceptions.py:419:18:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/jobs/job.py#L296'>airflow/jobs/job.py:296:39:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/jobs/job.py#L296'>airflow/jobs/job.py:296:39:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/datadog_logger.py#L103'>airflow/metrics/datadog_logger.py:103:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/datadog_logger.py#L103'>airflow/metrics/datadog_logger.py:103:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/otel_logger.py#L238'>airflow/metrics/otel_logger.py:238:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/otel_logger.py#L238'>airflow/metrics/otel_logger.py:238:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/models/baseoperator.py#L955'>airflow/models/baseoperator.py:955:18:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/models/baseoperator.py#L955'>airflow/models/baseoperator.py:955:18:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 [*] Use `float` instead of `int | float`
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/daos/query.py#L49'>superset/daos/query.py:49:52:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/daos/query.py#L49'>superset/daos/query.py:49:52:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/decorators.py#L163'>superset/utils/decorators.py:163:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/utils/decorators.py#L163'>superset/utils/decorators.py:163:24:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/statistics.pyi#L160'>stdlib/statistics.pyi:160:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/statistics.pyi#L160'>stdlib/statistics.pyi:160:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/turtle.pyi#L454'>stdlib/turtle.pyi:454:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/turtle.pyi#L454'>stdlib/turtle.pyi:454:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/turtle.pyi#L455'>stdlib/turtle.pyi:455:17:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/dc1ce5f5679359e53dcb84073312cd472840eb98/stdlib/turtle.pyi#L455'>stdlib/turtle.pyi:455:17:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 80 | 0 | 0 | 80 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.9` by @MichaReiser on 2025-01-08 10:29_

---

_Comment by @AlexWaygood on 2025-01-08 10:36_

>     * [Should `PYI041` consider `bool` to be redundant with `int`? #9810](https://github.com/astral-sh/ruff/issues/9810) asks to extend the rule to include `bool`. This should not block stabilization because it's an extension of the rule

I just closed this as rejected!

---

_@AlexWaygood approved on 2025-01-08 10:42_

It's weird that we didn't add a test for the preview-only behaviour in https://github.com/astral-sh/ruff/pull/14273. But this all LGTM!

---

_Comment by @MichaReiser on 2025-01-08 10:49_

> It's weird that we didn't add a test for the preview-only behaviour in #14273. But this all LGTM!

Agree. I think we did, but then gated the fix behind preview without creating a dedicated preview test.

---

_Merged by @MichaReiser on 2025-01-08 11:08_

---

_Closed by @MichaReiser on 2025-01-08 11:08_

---

_Branch deleted on 2025-01-08 11:08_

---
