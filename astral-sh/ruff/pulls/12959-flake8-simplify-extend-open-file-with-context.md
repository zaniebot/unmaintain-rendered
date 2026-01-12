```yaml
number: 12959
title: "[`flake8-simplify`] Extend open-file-with-context-handler to work with other standard-library IO modules (`SIM115`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-SIM115-to-tempfile
created_at: 2024-08-18T00:04:50Z
updated_at: 2024-08-22T13:18:56Z
url: https://github.com/astral-sh/ruff/pull/12959
synced_at: 2026-01-12T15:55:42Z
```

# [`flake8-simplify`] Extend open-file-with-context-handler to work with other standard-library IO modules (`SIM115`)

---

_@diceroll123_

## Summary

Closes #7313 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-08-18 00:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+39 -39 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use context handler for opening files
+ dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use a context manager for opening files
- dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/docs/exts/operators_and_hooks_ref.py#L181'>docs/exts/operators_and_hooks_ref.py:181:25:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/docs/exts/operators_and_hooks_ref.py#L181'>docs/exts/operators_and_hooks_ref.py:181:25:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/scripts/ci/pre_commit/check_deferrable_default.py#L66'>scripts/ci/pre_commit/check_deferrable_default.py:66:25:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/scripts/ci/pre_commit/check_deferrable_default.py#L66'>scripts/ci/pre_commit/check_deferrable_default.py:66:25:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/scripts/ci/pre_commit/validate_operators_init.py#L227'>scripts/ci/pre_commit/validate_operators_init.py:227:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/scripts/ci/pre_commit/validate_operators_init.py#L227'>scripts/ci/pre_commit/validate_operators_init.py:227:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/amazon/aws/example_local_to_s3.py#L40'>tests/system/providers/amazon/aws/example_local_to_s3.py:40:12:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/amazon/aws/example_local_to_s3.py#L40'>tests/system/providers/amazon/aws/example_local_to_s3.py:40:12:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_cohere.py#L57'>tests/system/providers/weaviate/example_weaviate_cohere.py:57:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_cohere.py#L57'>tests/system/providers/weaviate/example_weaviate_cohere.py:57:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_cohere.py#L72'>tests/system/providers/weaviate/example_weaviate_cohere.py:72:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_cohere.py#L72'>tests/system/providers/weaviate/example_weaviate_cohere.py:72:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py#L56'>tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py:56:27:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py#L56'>tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py:56:27:</a> SIM115 Use context handler for opening files
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use context handler for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L32'>examples/server/app/spectrogram/main.py:32:17:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L32'>examples/server/app/spectrogram/main.py:32:17:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L157'>release/build.py:157:29:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L157'>release/build.py:157:29:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L31'>release/remote.py:31:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L31'>release/remote.py:31:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L33'>release/remote.py:33:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L33'>release/remote.py:33:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L318'>scripts/milestone.py:318:11:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L318'>scripts/milestone.py:318:11:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L38'>scripts/sri.py:38:21:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L38'>scripts/sri.py:38:21:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L801'>src/bokeh/settings.py:801:47:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L801'>src/bokeh/settings.py:801:47:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L820'>src/bokeh/settings.py:820:34:</a> SIM115 Use a context manager for opening files
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM115 | 78 | 39 | 39 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+68 -39 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+36 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/hooks/s3.py#L1453'>airflow/providers/amazon/aws/hooks/s3.py:1453:20:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L247'>airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:247:29:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/docker/operators/docker.py#L487'>airflow/providers/docker/operators/docker.py:487:19:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/hooks/cloud_sql.py#L934'>airflow/providers/google/cloud/hooks/cloud_sql.py:934:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L195'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:195:27:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L212'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:212:35:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L228'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:228:34:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L354'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:354:27:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L465'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:465:34:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/utils/file.py#L159'>airflow/utils/file.py:159:30:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L185'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:185:24:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/breeze/src/airflow_breeze/utils/parallel.py#L62'>dev/breeze/src/airflow_breeze/utils/parallel.py:62:12:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/00e73e6089f2d54a38944ec47303aa00f9d211d7/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use context handler for opening files
+ dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use a context manager for opening files
- dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use context handler for opening files
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/superset/commands/dataset/importers/v1/utils.py#L198'>superset/commands/dataset/importers/v1/utils.py:198:16:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/tests/integration_tests/email_tests.py#L160'>tests/integration_tests/email_tests.py:160:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/tests/integration_tests/email_tests.py#L44'>tests/integration_tests/email_tests.py:44:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/tests/integration_tests/email_tests.py#L64'>tests/integration_tests/email_tests.py:64:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/1a1548da3bb7104214d55d12f0d54ab4c2832239/tests/integration_tests/email_tests.py#L95'>tests/integration_tests/email_tests.py:95:22:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+17 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L32'>examples/server/app/spectrogram/main.py:32:17:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L32'>examples/server/app/spectrogram/main.py:32:17:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L157'>release/build.py:157:29:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L157'>release/build.py:157:29:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L31'>release/remote.py:31:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L31'>release/remote.py:31:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L33'>release/remote.py:33:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/remote.py#L33'>release/remote.py:33:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L318'>scripts/milestone.py:318:11:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L318'>scripts/milestone.py:318:11:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L38'>scripts/sri.py:38:21:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L38'>scripts/sri.py:38:21:</a> SIM115 Use context handler for opening files
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L278'>src/poetry/inspection/lazy_wheel.py:278:41:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/13a6d538a93856a181b63c3fe356d866317044ba/reflex/utils/prerequisites.py#L678'>reflex/utils/prerequisites.py:678:14:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/9cad9644e7a93c3a00f7423124fda9a4bc554735/zerver/management/commands/backup.py#L124'>zerver/management/commands/backup.py:124:36:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/events/export.py#L149'>indico/modules/events/export.py:149:24:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/events/export.py#L413'>indico/modules/events/export.py:413:24:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/events/static/offline.py#L91'>indico/modules/events/static/offline.py:91:21:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/events/util.py#L369'>indico/modules/events/util.py:369:17:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/events/util.py#L710'>indico/modules/events/util.py:710:21:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/indico/indico/blob/086194f81a0329e56cf81ec8caa24a01e39d5c15/indico/modules/users/export.py#L79'>indico/modules/users/export.py:79:17:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM115 | 107 | 68 | 39 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:6 on 2024-08-18 15:33_

This is a bit of a nitpick: adding this import at the top means there's a really big diff in the snapshot, because the line numbers change for all the violations. Could you move all the additions to the snapshot to the bottom of the file? It'll make it much easier to review the changes to the snapshot (and easier for git archaeologists in the future to see what changed in this commit)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:129 on 2024-08-18 15:39_

There are loads of other standard-library functions and methods that we could also add:
- `tarfile.open()`
- `tarfile.TarFile.open()`
- `zipfile.ZipFile.open()`
- `zipfile.Path.open()`
- `io.open()`
- `io.open_code()`
- `codecs.open()`
- `bz2.open()`
- `gzip.open()`
- `dbm.open()`
- `dbm.gnu.open()`
- `dbm.ndbm.open()`
- `dbm.dumb.open()`
- `lzma.open()`
- `shelve.open()`
- `tokenize.open()`
- `wave.open()`

---

_@AlexWaygood requested changes on 2024-08-18 16:04_

Thanks, this overall looks reasonable! I think it might make sense for it to be a preview-only change first, though, since it increases the scope of the rule. We also need to update the docs for the rule, which currently just says that the rule

> Checks for uses of the builtin open() function without an associated context manager.

---

_@AlexWaygood reviewed on 2024-08-18 16:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:119 on 2024-08-18 16:08_

The change you've made to this function means that the rule no longer detects `pathlib.Path("foo").open("bar)"` calls, because the qualified name of `pathlib.Path("foo")` is not `Some(["pathlib", "Path"])`, it's `None`. The fully qualified name of `pathlib.Path` (an attribute expression) is `Some(["pathlib", "Path"])`, but `pathlib.Path("foo")` is a call expression. This is why a number of hits are going away in the ecosystem report, and also why one of the hits is going away in the snapshot.

---

_Label `rule` added by @MichaReiser on 2024-08-19 09:12_

---

_Label `preview` added by @MichaReiser on 2024-08-19 09:12_

---

_Converted to draft by @diceroll123 on 2024-08-21 00:34_

---

_@diceroll123 reviewed on 2024-08-21 00:34_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:129 on 2024-08-21 00:34_

- `tarfile.open()` ✅ 
- `tarfile.TarFile.open()` ✅ 
- `zipfile.ZipFile.open()` ✅ 
- `zipfile.Path.open()` ❌ zipfile.Path does not support context manager protocol
- `io.open()` ✅
- `io.open_code()` ✅ 
- `codecs.open()` ✅
- `bz2.open()` ✅
- `gzip.open()` ✅
- `dbm.open()` ✅
- `dbm.gnu.open()` ✅ 
- `dbm.ndbm.open()` ✅ 
- `dbm.dumb.open()` ✅ 
- `lzma.open()` ✅
- `shelve.open()` ✅
- `tokenize.open()` ✅
- `wave.open()` ✅

Also added a few others, gonna look around for more additions that could be made

---

_Marked ready for review by @diceroll123 on 2024-08-22 09:27_

---

_Review requested from @AlexWaygood by @diceroll123 on 2024-08-22 09:34_

---

_@AlexWaygood reviewed on 2024-08-22 10:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:129 on 2024-08-22 10:41_

> `zipfile.Path.open()` ❌ zipfile.Path does not support context manager protocol

`zipfile.Path` doesn't... but I think if you create a `zipfile.Path` object and then call `.open()` on that instance, it should work pretty similarly to to `pathlib.Path(...).open()`... let me just find a zipfile to test with 😄

---

_@AlexWaygood reviewed on 2024-08-22 10:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:129 on 2024-08-22 10:45_

Yeah, confirmed: if I have a zip archive at `foo.zip`, where `hello.py` is a file in that zip archive, I can do this:

```pycon
>>> import zipfile
>>> p = zipfile.Path('foo.zip') / 'hello.py'
>>> f = p.open()
>>> f.close()
>>>
```

---

_@AlexWaygood reviewed on 2024-08-22 10:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:129 on 2024-08-22 10:46_

Hmm, but I suppose that kind of thing is too complicated for this rule to catch really; we'd need type inference. I guess you're right; we can leave this out for now. Thanks!

---

_Comment by @AlexWaygood on 2024-08-22 10:53_

Looking at the ecosystem results: a _lot_ of them seem to be regarding `io.StringIO()` and `io.BytesIO()`. Those two weren't ones that I suggested in https://github.com/astral-sh/ruff/pull/12959#discussion_r1721005148, and I'm inclined to think that they probably shouldn't be part of this rule: although they can be used as context managers, they're _in-memory_ buffers rather than functions or classes that manipulate files on disk in some way. So it's not nearly as much of an issue if you forget to close them after you're done with them -- there's a small risk of a memory leak, but the in-memory buffer will be discarded after the function scope ends if it's only assigned to a local variable.

(I'll push some fixes to address this)

---

_@AlexWaygood approved on 2024-08-22 10:59_

Thanks @diceroll123! I'll wait to check the final ecosystem report before merging, but this LGTM now.

---

_Renamed from "[`flake8-simplify`] - extend open-file-with-context-handler to work with `tempfile` (`SIM115`)" to "[`flake8-simplify`] Extend open-file-with-context-handler to work with other standard-library IO modules (`SIM115`)" by @AlexWaygood on 2024-08-22 13:18_

---

_Merged by @AlexWaygood on 2024-08-22 13:18_

---

_Closed by @AlexWaygood on 2024-08-22 13:18_

---
