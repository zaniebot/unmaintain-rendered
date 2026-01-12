```yaml
number: 16681
title: "[`pyupgrade`]: Deprecate `non-pep604-isinstance` (`UP038`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
  - breaking
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/deprecate-up038
created_at: 2025-03-12T15:42:19Z
updated_at: 2025-03-13T07:42:17Z
url: https://github.com/astral-sh/ruff/pull/16681
synced_at: 2026-01-12T15:55:55Z
```

# [`pyupgrade`]: Deprecate `non-pep604-isinstance` (`UP038`)

---

_@MichaReiser_

## Summary

This PR deprecates UP038. Using PEP 604 syntax in `isinstance` and `issubclass` calls isn't a recommended pattern (or community agreed best practice) 
and it negatively impacts performance. 

Resolves https://github.com/astral-sh/ruff/issues/7871

## Test Plan

I tested that selecting `UP038` results in a warning in no-preview mode and an error in preview mode


---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-12 15:42_

---

_Label `rule` added by @MichaReiser on 2025-03-12 15:42_

---

_Label `breaking` added by @MichaReiser on 2025-03-12 15:42_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 15:43_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 15:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fdeprecate-up038)

### Merging #16681 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/deprecate-up038</code> (9511be4) with <code>micha/ruff-0.10</code> (464ea4a)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fdeprecate-up038)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -39 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -39 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/async_events/async_query_manager.py#L269'>superset/async_events/async_query_manager.py:269:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/cli/main.py#L55'>superset/cli/main.py:55:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/commands/dataset/importers/v1/utils.py#L212'>superset/commands/dataset/importers/v1/utils.py:212:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L529'>superset/connectors/sqla/models.py:529:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L562'>superset/connectors/sqla/models.py:562:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L566'>superset/connectors/sqla/models.py:566:35:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L568'>superset/connectors/sqla/models.py:568:37:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/cockroachdb.py#L36'>superset/db_engine_specs/cockroachdb.py:36:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/databend.py#L326'>superset/db_engine_specs/databend.py:326:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/druid.py#L116'>superset/db_engine_specs/druid.py:116:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/duckdb.py#L229'>superset/db_engine_specs/duckdb.py:229:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/dynamodb.py#L64'>superset/db_engine_specs/dynamodb.py:64:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/sqlite.py#L120'>superset/db_engine_specs/sqlite.py:120:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/core.py#L633'>superset/models/core.py:633:21:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1164'>superset/models/helpers.py:1164:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1197'>superset/models/helpers.py:1197:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1201'>superset/models/helpers.py:1201:35:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1203'>superset/models/helpers.py:1203:37:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1833'>superset/models/helpers.py:1833:28:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/sql/parse.py#L365'>superset/sql/parse.py:365:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/sql_parse.py#L472'>superset/sql_parse.py:472:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/core.py#L377'>superset/utils/core.py:377:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/core.py#L412'>superset/utils/core.py:412:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L126'>superset/utils/json.py:126:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L157'>superset/utils/json.py:157:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L94'>superset/utils/json.py:94:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L102'>superset/utils/mock_data.py:102:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L117'>superset/utils/mock_data.py:117:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L140'>superset/utils/mock_data.py:140:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L73'>superset/utils/mock_data.py:73:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L81'>superset/utils/mock_data.py:81:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L89'>superset/utils/mock_data.py:89:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L97'>superset/utils/mock_data.py:97:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L81'>superset/utils/pandas_postprocessing/boxplot.py:81:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L83'>superset/utils/pandas_postprocessing/boxplot.py:83:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L84'>superset/utils/pandas_postprocessing/boxplot.py:84:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/viz.py#L1596'>superset/viz.py:1596:59:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/viz.py#L970'>superset/viz.py:970:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/tests/integration_tests/base_tests.py#L193'>tests/integration_tests/base_tests.py:193:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP038 | 39 | 0 | 39 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -39 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -39 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/async_events/async_query_manager.py#L269'>superset/async_events/async_query_manager.py:269:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/cli/main.py#L55'>superset/cli/main.py:55:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/commands/dataset/importers/v1/utils.py#L212'>superset/commands/dataset/importers/v1/utils.py:212:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L529'>superset/connectors/sqla/models.py:529:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L562'>superset/connectors/sqla/models.py:562:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L566'>superset/connectors/sqla/models.py:566:35:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/connectors/sqla/models.py#L568'>superset/connectors/sqla/models.py:568:37:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/cockroachdb.py#L36'>superset/db_engine_specs/cockroachdb.py:36:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/databend.py#L326'>superset/db_engine_specs/databend.py:326:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/druid.py#L116'>superset/db_engine_specs/druid.py:116:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/duckdb.py#L229'>superset/db_engine_specs/duckdb.py:229:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/dynamodb.py#L64'>superset/db_engine_specs/dynamodb.py:64:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/db_engine_specs/sqlite.py#L120'>superset/db_engine_specs/sqlite.py:120:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/core.py#L633'>superset/models/core.py:633:21:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1164'>superset/models/helpers.py:1164:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1197'>superset/models/helpers.py:1197:12:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1201'>superset/models/helpers.py:1201:35:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1203'>superset/models/helpers.py:1203:37:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/models/helpers.py#L1833'>superset/models/helpers.py:1833:28:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/sql/parse.py#L365'>superset/sql/parse.py:365:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/sql_parse.py#L472'>superset/sql_parse.py:472:16:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/core.py#L377'>superset/utils/core.py:377:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/core.py#L412'>superset/utils/core.py:412:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L126'>superset/utils/json.py:126:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L157'>superset/utils/json.py:157:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/json.py#L94'>superset/utils/json.py:94:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L102'>superset/utils/mock_data.py:102:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L117'>superset/utils/mock_data.py:117:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L140'>superset/utils/mock_data.py:140:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L73'>superset/utils/mock_data.py:73:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L81'>superset/utils/mock_data.py:81:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L89'>superset/utils/mock_data.py:89:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/mock_data.py#L97'>superset/utils/mock_data.py:97:8:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L81'>superset/utils/pandas_postprocessing/boxplot.py:81:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L83'>superset/utils/pandas_postprocessing/boxplot.py:83:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/utils/pandas_postprocessing/boxplot.py#L84'>superset/utils/pandas_postprocessing/boxplot.py:84:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/viz.py#L1596'>superset/viz.py:1596:59:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/viz.py#L970'>superset/viz.py:970:17:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/tests/integration_tests/base_tests.py#L193'>tests/integration_tests/base_tests.py:193:20:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP038 | 39 | 0 | 39 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 16:58_

---

_@ntBre approved on 2025-03-12 18:13_

---

_Merged by @MichaReiser on 2025-03-13 07:42_

---

_Closed by @MichaReiser on 2025-03-13 07:42_

---

_Branch deleted on 2025-03-13 07:42_

---
