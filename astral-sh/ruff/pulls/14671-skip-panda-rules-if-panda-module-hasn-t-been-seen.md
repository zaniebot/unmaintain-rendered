```yaml
number: 14671
title: "Skip panda rules if panda module hasn't been seen"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: micha/disable-pd-rules-for-non-pd-code
created_at: 2024-11-29T07:33:31Z
updated_at: 2024-11-29T21:32:53Z
url: https://github.com/astral-sh/ruff/pull/14671
synced_at: 2026-01-10T20:50:57Z
```

# Skip panda rules if panda module hasn't been seen

---

_Pull request opened by @MichaReiser on 2024-11-29 07:33_

## Summary

Issues about `PD011` and other panda rules triggering on non-panda code are frequent: 

* https://github.com/astral-sh/ruff/issues/14669
* https://github.com/astral-sh/ruff/issues/14508
* https://github.com/astral-sh/ruff/issues/6432
* https://github.com/astral-sh/ruff/issues/11235
* https://github.com/astral-sh/ruff/issues/8846
* https://github.com/astral-sh/ruff/issues/11858
* https://github.com/astral-sh/ruff/issues/13495
* https://github.com/astral-sh/ruff/issues/3807
* https://github.com/astral-sh/ruff/issues/14301

There are probably more. At this point, I consider the rule harmful to the majority of users. 

This PR adds an `seen_module` check for all panda rules. This likely results in more false negatives, but I consider
this is the better outcome, considering how many users the rules confused in the past. 

A proper fix requires type inference


## Test Plan

I verified that the following code snippet doesn't trigger a pandas violation anymore, unless I change the import to `pandas`

```py
import polars as pl

pl_df = pl.DataFrame([{"a": 1, "b": 2}, {"a": 3, "b": 4}])
pl_df.pivot(on="a", values="b")
```

---

_Label `rule` added by @MichaReiser on 2024-11-29 07:33_

---

_Label `bug` added by @MichaReiser on 2024-11-29 07:36_

---

_Comment by @github-actions[bot] on 2024-11-29 07:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -98 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py#L192'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py:192:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py#L348'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py:348:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/drill/hooks/test_drill.py#L103'>providers/tests/apache/drill/hooks/test_drill.py:103:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/druid/hooks/test_druid.py#L456'>providers/tests/apache/druid/hooks/test_druid.py:456:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L120'>providers/tests/apache/impala/hooks/test_impala.py:120:33:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L121'>providers/tests/apache/impala/hooks/test_impala.py:121:33:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/pinot/hooks/test_pinot.py#L274'>providers/tests/apache/pinot/hooks/test_pinot.py:274:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L118'>providers/tests/elasticsearch/hooks/test_elasticsearch.py:118:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L119'>providers/tests/elasticsearch/hooks/test_elasticsearch.py:119:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L112'>providers/tests/google/suite/hooks/test_sheets.py:112:28:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L134'>providers/tests/google/suite/hooks/test_sheets.py:134:25:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L162'>providers/tests/google/suite/hooks/test_sheets.py:162:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L192'>providers/tests/google/suite/hooks/test_sheets.py:192:31:</a> PD011 Use `.to_numpy()` instead of `.values`
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/superset/db_engine_specs/clickhouse.py#L174'>superset/db_engine_specs/clickhouse.py:174:23:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L363'>tests/integration_tests/model_tests.py:363:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L366'>tests/integration_tests/model_tests.py:366:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L373'>tests/integration_tests/model_tests.py:373:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L376'>tests/integration_tests/model_tests.py:376:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:22:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L493'>tests/integration_tests/model_tests.py:493:22:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/columnar_reader_test.py#L123'>tests/unit_tests/commands/databases/columnar_reader_test.py:123:21:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/columnar_reader_test.py#L196'>tests/unit_tests/commands/databases/columnar_reader_test.py:196:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/csv_reader_test.py#L255'>tests/unit_tests/commands/databases/csv_reader_test.py:255:21:</a> PD011 Use `.to_numpy()` instead of `.values`
... 2 additional changes omitted for rule PD011
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -56 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/periodic_shells.py#L46'>examples/plotting/periodic_shells.py:46:29:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/density.py#L29'>examples/topics/stats/density.py:29:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L517'>tests/unit/bokeh/test_client_server.py:517:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L518'>tests/unit/bokeh/test_client_server.py:518:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L519'>tests/unit/bokeh/test_client_server.py:519:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L520'>tests/unit/bokeh/test_client_server.py:520:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L521'>tests/unit/bokeh/test_client_server.py:521:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L533'>tests/unit/bokeh/test_client_server.py:533:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L544'>tests/unit/bokeh/test_client_server.py:544:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L545'>tests/unit/bokeh/test_client_server.py:545:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L546'>tests/unit/bokeh/test_client_server.py:546:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L547'>tests/unit/bokeh/test_client_server.py:547:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L548'>tests/unit/bokeh/test_client_server.py:548:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L565'>tests/unit/bokeh/test_client_server.py:565:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L585'>tests/unit/bokeh/test_client_server.py:585:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L586'>tests/unit/bokeh/test_client_server.py:586:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L587'>tests/unit/bokeh/test_client_server.py:587:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L588'>tests/unit/bokeh/test_client_server.py:588:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L589'>tests/unit/bokeh/test_client_server.py:589:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L601'>tests/unit/bokeh/test_client_server.py:601:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L617'>tests/unit/bokeh/test_client_server.py:617:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L618'>tests/unit/bokeh/test_client_server.py:618:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L619'>tests/unit/bokeh/test_client_server.py:619:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L620'>tests/unit/bokeh/test_client_server.py:620:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L621'>tests/unit/bokeh/test_client_server.py:621:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L633'>tests/unit/bokeh/test_client_server.py:633:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L644'>tests/unit/bokeh/test_client_server.py:644:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L645'>tests/unit/bokeh/test_client_server.py:645:17:</a> PD011 Use `.to_numpy()` instead of `.values`
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/record.py#L306'>src/latch/registry/record.py:306:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/record.py#L309'>src/latch/registry/record.py:309:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/utils.py#L438'>src/latch/registry/utils.py:438:19:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch_cli/services/get_params.py#L330'>src/latch_cli/services/get_params.py:330:37:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD011 | 94 | 0 | 94 | 0 | 0 |
| PD009 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -98 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py#L192'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py:192:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py#L348'>providers/src/airflow/providers/google/cloud/hooks/vertex_ai/generative_model.py:348:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/drill/hooks/test_drill.py#L103'>providers/tests/apache/drill/hooks/test_drill.py:103:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/druid/hooks/test_druid.py#L456'>providers/tests/apache/druid/hooks/test_druid.py:456:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L120'>providers/tests/apache/impala/hooks/test_impala.py:120:33:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L121'>providers/tests/apache/impala/hooks/test_impala.py:121:33:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/pinot/hooks/test_pinot.py#L274'>providers/tests/apache/pinot/hooks/test_pinot.py:274:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L118'>providers/tests/elasticsearch/hooks/test_elasticsearch.py:118:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L119'>providers/tests/elasticsearch/hooks/test_elasticsearch.py:119:37:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L112'>providers/tests/google/suite/hooks/test_sheets.py:112:28:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L134'>providers/tests/google/suite/hooks/test_sheets.py:134:25:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L162'>providers/tests/google/suite/hooks/test_sheets.py:162:31:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/google/suite/hooks/test_sheets.py#L192'>providers/tests/google/suite/hooks/test_sheets.py:192:31:</a> PD011 Use `.to_numpy()` instead of `.values`
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/superset/db_engine_specs/clickhouse.py#L174'>superset/db_engine_specs/clickhouse.py:174:23:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L363'>tests/integration_tests/model_tests.py:363:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L366'>tests/integration_tests/model_tests.py:366:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L373'>tests/integration_tests/model_tests.py:373:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L376'>tests/integration_tests/model_tests.py:376:20:</a> PD009 Use `.iloc` instead of `.iat`. If speed is important, use NumPy.
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:22:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/integration_tests/model_tests.py#L493'>tests/integration_tests/model_tests.py:493:22:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/columnar_reader_test.py#L123'>tests/unit_tests/commands/databases/columnar_reader_test.py:123:21:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/columnar_reader_test.py#L196'>tests/unit_tests/commands/databases/columnar_reader_test.py:196:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/apache/superset/blob/dbcb473040b7e8ef1d41a81ba594387b66c04ebe/tests/unit_tests/commands/databases/csv_reader_test.py#L255'>tests/unit_tests/commands/databases/csv_reader_test.py:255:21:</a> PD011 Use `.to_numpy()` instead of `.values`
... 2 additional changes omitted for rule PD011
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -56 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/periodic_shells.py#L46'>examples/plotting/periodic_shells.py:46:29:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/stats/density.py#L29'>examples/topics/stats/density.py:29:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L517'>tests/unit/bokeh/test_client_server.py:517:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L518'>tests/unit/bokeh/test_client_server.py:518:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L519'>tests/unit/bokeh/test_client_server.py:519:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L520'>tests/unit/bokeh/test_client_server.py:520:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L521'>tests/unit/bokeh/test_client_server.py:521:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L533'>tests/unit/bokeh/test_client_server.py:533:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L544'>tests/unit/bokeh/test_client_server.py:544:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L545'>tests/unit/bokeh/test_client_server.py:545:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L546'>tests/unit/bokeh/test_client_server.py:546:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L547'>tests/unit/bokeh/test_client_server.py:547:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L548'>tests/unit/bokeh/test_client_server.py:548:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L565'>tests/unit/bokeh/test_client_server.py:565:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L585'>tests/unit/bokeh/test_client_server.py:585:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L586'>tests/unit/bokeh/test_client_server.py:586:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L587'>tests/unit/bokeh/test_client_server.py:587:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L588'>tests/unit/bokeh/test_client_server.py:588:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L589'>tests/unit/bokeh/test_client_server.py:589:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L601'>tests/unit/bokeh/test_client_server.py:601:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L617'>tests/unit/bokeh/test_client_server.py:617:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L618'>tests/unit/bokeh/test_client_server.py:618:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L619'>tests/unit/bokeh/test_client_server.py:619:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L620'>tests/unit/bokeh/test_client_server.py:620:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L621'>tests/unit/bokeh/test_client_server.py:621:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L633'>tests/unit/bokeh/test_client_server.py:633:53:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L644'>tests/unit/bokeh/test_client_server.py:644:17:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_client_server.py#L645'>tests/unit/bokeh/test_client_server.py:645:17:</a> PD011 Use `.to_numpy()` instead of `.values`
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/record.py#L306'>src/latch/registry/record.py:306:12:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/record.py#L309'>src/latch/registry/record.py:309:16:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch/registry/utils.py#L438'>src/latch/registry/utils.py:438:19:</a> PD011 Use `.to_numpy()` instead of `.values`
- <a href='https://github.com/latchbio/latch/blob/91eb9e17e4c745e4702a676b365a96f73eb09f5a/src/latch_cli/services/get_params.py#L330'>src/latch_cli/services/get_params.py:330:37:</a> PD011 Use `.to_numpy()` instead of `.values`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD011 | 94 | 0 | 94 | 0 | 0 |
| PD009 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-11-29 07:48_

As expected, this increases false negatives but also reduces false positives. The airflow and bokeh are a mix of new false negatives and fewer false positives. Superset is almost exclusively more false-negatives. 

---

_Review requested from @charliermarsh by @MichaReiser on 2024-11-29 07:48_

---

_Comment by @sbrugman on 2024-11-29 11:51_

As someone who works a lot with pandas, spark, polars etc. I think this is the right trade-off for now. 

False positives are a reason to disable the rules completely.

False negatives are mostly in test files or "glue code" where type annotations are missing. In production code it's more common to write pure functions that take DataFrames as input and output, and if these functions are typed then the rules apply.

The rules can be expanded again when inference of DataFrame and Series types is possible.

---

_Comment by @MichaReiser on 2024-11-29 11:54_

> False negatives are mostly in test files or "glue code" where type annotations are missing. In production code it's more common to write pure functions that take DataFrames as input and output, and if these functions are typed then the rules apply.

I'm just pointing out that this is currently not the case. It would require adding type inference support for panda data frames. 

---

_Comment by @sbrugman on 2024-11-29 12:26_

Ah sorry, I meant that when functions are typed, `pandas` is also imported and thus the "has seen module" logic applies.

---

_Comment by @AlexWaygood on 2024-11-29 12:42_

It's possible to think of heuristics we could use to avoid some of the new false negatives. For example, [this function](https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L108-L121) is pretty clearly using pandas because the function name has `pandas` in it. Using heuristics like that has the downside that it makes the rule harder to understand for users, however.

---

_Comment by @MichaReiser on 2024-11-29 12:48_

@AlexWaygood I don't think this is worth special casing because it only helps in a few limited cases. 

---

_Comment by @AlexWaygood on 2024-11-29 12:58_

> @AlexWaygood I don't think this is worth special casing because it only helps in a few limited cases.

It would actually get rid of seven new false negatives on `airflow` highlighted by the ecosystem check:

- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/drill/hooks/test_drill.py#L103
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/druid/hooks/test_druid.py#L456
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L120
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/impala/hooks/test_impala.py#L121
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/apache/pinot/hooks/test_pinot.py#L274
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L118
- https://github.com/apache/airflow/blob/fdd353a03e4b058fff834b611880c2475abfac61/providers/tests/elasticsearch/hooks/test_elasticsearch.py#L119

But no others. So, agreed that it probably isn't worth special-casing.

---

_@AlexWaygood approved on 2024-11-29 13:00_

This change makes sense to me.

There are also some other libraries that the pandas docs specifically calls out as either being `pandas` plugins/extensions, or as making heavy use of `pandas`. Possibly we could also emit these lints if one of those has been imported: https://pandas.pydata.org/community/ecosystem.html

---

_Comment by @MichaReiser on 2024-11-29 21:32_

I like the idea but I'll leave it as is for now. I'm not sure how I feel about adding that many modules. Let's see if it comes up in user reports.

---

_Merged by @MichaReiser on 2024-11-29 21:32_

---

_Closed by @MichaReiser on 2024-11-29 21:32_

---

_Branch deleted on 2024-11-29 21:32_

---
