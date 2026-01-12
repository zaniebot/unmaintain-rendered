```yaml
number: 14273
title: "[`flake8-pyi`] Implement autofix for `redundant-numeric-union` (`PYI041`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pyi041-redundant-numeric-union-autofix
created_at: 2024-11-11T12:35:52Z
updated_at: 2024-11-25T18:20:39Z
url: https://github.com/astral-sh/ruff/pull/14273
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Implement autofix for `redundant-numeric-union` (`PYI041`)

---

_@sbrugman_

## Summary

This PR adds autofix for `redundant-numeric-union` (`PYI041`)

There are some comments below to explain the reasoning behind some choices that might help review.

<!-- What's the purpose of the change? What does it do, and why? -->

Resolves part of https://github.com/astral-sh/ruff/issues/14185.

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 12:35_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:70 on 2024-11-11 12:36_

Emitting multiple messages for the same union, such as "Use `complex` instead of `int | complex`" and  "Use `complex` instead of `float | complex`" gets confusing when there is a fix presented:

```diff
-    def bad4(self, arg: Union[float | complex, int]) -> None: ...  # PYI041
+    def bad4(self, arg: complex) -> None: ...  # PYI041
```

With `IntFloatComplex` we can assign an unique violation for each combination.

---

_@sbrugman reviewed on 2024-11-11 12:36_

---

_@sbrugman reviewed on 2024-11-11 12:36_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:77 on 2024-11-11 12:36_

Replace `has_*` with `bitflags`, as they correspond naturally to the order of Complex > Float > Int.

---

_@sbrugman reviewed on 2024-11-11 12:37_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:102 on 2024-11-11 12:37_

The second loop is only active when there is a violation, instead of collecting all expressions for any Union and filtering them later to avoid overhead in those cases.

---

_@sbrugman reviewed on 2024-11-11 12:38_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:177 on 2024-11-11 12:38_

There must be a more elegant way of doing this, but I haven't found it yet and this works.

---

_@sbrugman reviewed on 2024-11-11 12:38_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:234 on 2024-11-11 12:38_

The code above handles `nodes.len() < 2` separately and thus this function should never encounter these cases. Is `debug_assert!` ok to denote this?

---

_Comment by @github-actions[bot] on 2024-11-11 12:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +66 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +58 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L50'>airflow/metrics/base_stats_logger.py:50:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/base_stats_logger.py#L61'>airflow/metrics/base_stats_logger.py:61:15:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/datadog_logger.py#L103'>airflow/metrics/datadog_logger.py:103:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/datadog_logger.py#L103'>airflow/metrics/datadog_logger.py:103:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/otel_logger.py#L237'>airflow/metrics/otel_logger.py:237:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/otel_logger.py#L237'>airflow/metrics/otel_logger.py:237:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/metrics/statsd_logger.py#L117'>airflow/metrics/statsd_logger.py:117:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/timezone.py#L240'>airflow/utils/timezone.py:240:26:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/airflow/utils/timezone.py#L305'>airflow/utils/timezone.py:305:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L175'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:175:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L337'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:337:31:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L273'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:273:16:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L302'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:302:56:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L325'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:325:57:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:27:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:47:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:72:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L516'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:516:72:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L544'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:544:22:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_client.py#L544'>providers/src/airflow/providers/amazon/aws/hooks/batch_client.py:544:22:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/a85d94e6cdcd09efe93c3acee0b4ce5c9508bc23/providers/src/airflow/providers/amazon/aws/hooks/batch_waiters.py#L195'>providers/src/airflow/providers/amazon/aws/hooks/batch_waiters.py:195:16:</a> PYI041 Use `float` instead of `int | float`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/daos/query.py#L68'>superset/daos/query.py:68:52:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/cache.py#L150'>superset/utils/cache.py:150:14:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/core.py#L349'>superset/utils/core.py:349:24:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/decorators.py#L163'>superset/utils/decorators.py:163:24:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/superset/utils/decorators.py#L163'>superset/utils/decorators.py:163:24:</a> PYI041 [*] Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/turtle.pyi#L443'>stdlib/turtle.pyi:443:16:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/f554f54673122a9ac45625f26805458c9cac2e3a/stdlib/turtle.pyi#L444'>stdlib/turtle.pyi:444:17:</a> PYI041 Use `float` instead of `int | float`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI041 | 70 | 0 | 4 | 66 | 0 |

</p>
</details>




---

_@sbrugman reviewed on 2024-11-11 12:50_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:140 on 2024-11-11 12:50_

TODO(SB):     
```rs
if checker.settings.preview.is_enabled() {
```

---

_Label `rule` added by @AlexWaygood on 2024-11-11 13:56_

---

_Label `fixes` added by @AlexWaygood on 2024-11-11 13:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:49 on 2024-11-12 08:31_

the fix will...?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:52 on 2024-11-12 08:32_

What's the reason that the fix is marked as unsafe when comments are present? Does it produce invalid syntax? Are there cases it can't handle?

I think our general practice has now been to not mark a fix as unsafe just because it removes comments. Instead, we stated that the fix could remove comments.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:177 on 2024-11-12 08:43_

I don't think there is and it's inherent to bitflags where it isn't required that all possible combinations are valid. 

I would write this as

```
        if numeric_flags == NumericFlags::INT | NumericFlags::FLOAT | NumericFlags::COMPLEX {
            Some(Self::IntFloatComplex)
        } else if numeric_flags == NumericFlags::INT | NumericFlags::FLOAT {
            Some(Self::IntComplex)
        } else if numeric_flags == NumericFlags::FLOAT | NumericFlags::COMPLEX {
            Some(Self::FloatComplex)
        } else if numeric_flags == NumericFlags::FLOAT | NumericFlags::INT {
            Some(Self::IntFloat)
        } else {
            None
        }
```



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:148 on 2024-11-12 08:45_

Is there a specific reason for using a generator over a text-based fix? Most fixes are text based to preserve existing formatting (and possibly comments).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:126 on 2024-11-12 08:47_

Isn't this the same as?

```suggestion
            || builtin_type == "complex"
```

Nit: It could also be worth to have a `BuiltInTy` enum with `Float`, `Int`, and `Complex` to reduce some of the string matching

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:133 on 2024-11-12 08:49_

This comment seems a copy-paste leftover from above. We aren't traversing the union to remember which numeric types are found, but to generate the fix.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:285 on 2024-11-12 08:53_

Isn't this branch dead because of the assertion above?

---

_@MichaReiser reviewed on 2024-11-12 08:55_

Thanks. This overall looks great. 

The main open questions are

* should use text based edits to avoid reformatting unchanged code and loosing inner comments. 
* Whether it's possible to always mark the fix as safe, regardless of comments

---

_Comment by @MichaReiser on 2024-11-12 08:56_

Oh, and did you look through the ecosystem changes? If so, could you update the test plan. It's helpful to know if you did because I then don't need to carefully review all of them and can instead just click through some of them.

---

_@sbrugman reviewed on 2024-11-12 15:01_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:126 on 2024-11-12 15:01_

Here I think the `builtin_type` is not guaranteed to be `int`/`float`/`complex`, in which case we cannot replace this condition

---

_@sbrugman reviewed on 2024-11-12 15:01_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:285 on 2024-11-12 15:01_

Good catch!

---

_@sbrugman reviewed on 2024-11-12 15:18_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:177 on 2024-11-12 15:18_

Thanks, yes that looks better.

---

_Comment by @sbrugman on 2024-11-12 15:34_

> Oh, and did you look through the ecosystem changes? If so, could you update the test plan. It's helpful to know if you did because I then don't need to carefully review all of them and can instead just click through some of them.

The ecosystem results look good.
From [this example](https://github.com/apache/superset/blob/0e165c1a21a90098adfea8efa5006e4feb2adf11/superset/utils/core.py#L349) I'm wondering if we shouldn't include return types; it appears to skip that one now.

Typeshed shows negative in the ecosystem results, but I expect that to not be related to the PR.
~~Maybe @AlexWaygood can help out with that.~~
It's probably because the example is Py3.13+ 

---

_@sbrugman reviewed on 2024-11-12 15:44_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:148 on 2024-11-12 15:44_

I've used the generator based on other rules, it could very well be that a text-based fix is more optimal. 

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:148 on 2024-11-12 16:35_

However, redundant expressions are still dropped. If they have the comment, it will be still lost.

---

_@sbrugman reviewed on 2024-11-12 16:35_

---

_Comment by @MichaReiser on 2024-11-13 09:11_

> From [this example](https://github.com/apache/superset/blob/0e165c1a21a90098adfea8efa5006e4feb2adf11/superset/utils/core.py#L349) I'm wondering if we shouldn't include return types; it appears to skip that one now.

Could you expand what you mean by *it appears to skip that now*. Is it not flagging this case or do you think this case shouldn't be flagged?

Is it worth to add a test like that example (or that specific example) to our test suite?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:49 on 2024-11-13 09:14_

Reading the documentation, I get the impression the fix isn't safe because it flattens nested union expressions. But I understand that this doesn't make the fix unsafe, it's just an extension where the fix goes beyond what one might expect.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:80 on 2024-11-13 09:14_

Nit: To align with the terminology used in the documentation

```suggestion
        Some("Remove redundant type".to_string())
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:126 on 2024-11-13 09:16_

Oh, in case it is e.g. `str` or `bool` and I think there are unit tests that exercise this.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:148 on 2024-11-13 09:17_

> I've used the generator based on other rules, it could very well be that a text-based fix is more optimal.

We generally prefer using text-based fixes if they are not too complicated because they preserve more comments and the formatting. But I don't think it's worth rewriting it now, or we can do so as a separate PR .

---

_@MichaReiser approved on 2024-11-13 09:18_

---

_Label `preview` added by @MichaReiser on 2024-11-13 09:18_

---

_Label `rule` removed by @MichaReiser on 2024-11-13 09:18_

---

_Comment by @AlexWaygood on 2024-11-13 11:28_

> Typeshed shows negative in the ecosystem results, but I expect that to not be related to the PR.
> ~Maybe @AlexWaygood can help out with that.~
> It's probably because the example is Py3.13+

The ecosystem reports for typeshed have [often not been reproducible locally](https://github.com/astral-sh/ruff/issues/11305) and nobody's yet had a chance to look into why... if you can't repro the typeshed hits locally, I'd just ignore them!

---

_Converted to draft by @sbrugman on 2024-11-13 14:48_

---

_Converted to draft by @sbrugman on 2024-11-13 14:48_

---

_Comment by @sbrugman on 2024-11-13 14:53_

> Could you expand what you mean by it appears to skip that now. Is it not flagging this case or do you think this case shouldn't be flagged?

After investigating this, it turned out that this rule hooked into `statement.rs` instead of `expression.rs` like all the other rules related to unions do. This caused false negatives on the return types and type aliases. We had no test for this. Fixed it, and added a test.

---

_@sbrugman reviewed on 2024-11-13 15:00_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:49 on 2024-11-13 15:00_

Exactly. I've reused the phrasing that Charlie used for another rule.

The nesting is a feature to allow cases such as these:
```python
ReadOnlyMode         = Literal["r", "r+"]
WriteAndTruncateMode = Literal["w", "w+", "wt", "w+t"]
WriteNoTruncateMode  = Literal["r+", "r+t"]
AppendMode           = Literal["a", "a+", "at", "a+t"]

AllModes = Literal[ReadOnlyMode, WriteAndTruncateMode,
                   WriteNoTruncateMode, AppendMode]
```

As a consequence this redundant nesting is also allowed, but can be safely fixed:

```python
AllModes = Literal[Literal["r", "r+"], Literal["w", "w+", "wt", "w+t"],
                   Literal["r+", "r+t"], Literal["a", "a+", "at", "a+t"]]
```

See https://typing.readthedocs.io/en/latest/spec/literal.html#legal-and-illegal-parameterizations

~~We probably should have~~ I will create a rule for this (for both `union` and `literal`).

---

_Marked ready for review by @sbrugman on 2024-11-13 15:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:102 on 2024-11-14 07:27_

Oh nice find. I think we should move this change out into its own PR so that it gets its own entry in the changelog. I have to think about it if it warrants a breaking change or not.

---

_@MichaReiser reviewed on 2024-11-14 07:27_

---

_@MichaReiser reviewed on 2024-11-14 07:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:49 on 2024-11-14 07:30_

After reading the linked document, I understand that both examples are valid. Can you share an example where flattening a nested union is unsafe?

---

_@MichaReiser approved on 2024-11-14 07:30_

---

_@sbrugman reviewed on 2024-11-14 07:54_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:102 on 2024-11-14 07:54_

I'll move the last commit into it's own PR

---

_@charliermarsh reviewed on 2024-11-16 18:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:49 on 2024-11-16 18:08_

I tweaked the comment a bit on all three rules.

---

_Merged by @charliermarsh on 2024-11-16 18:13_

---

_Closed by @charliermarsh on 2024-11-16 18:13_

---

_Branch deleted on 2024-11-25 18:20_

---
