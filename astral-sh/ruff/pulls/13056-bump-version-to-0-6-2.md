```yaml
number: 13056
title: Bump version to 0.6.2
type: pull_request
state: merged
author: dhruvmanila
labels:
  - release
assignees: []
merged: true
base: main
head: dhruv/bump
created_at: 2024-08-22T13:21:09Z
updated_at: 2024-08-22T13:41:54Z
url: https://github.com/astral-sh/ruff/pull/13056
synced_at: 2026-01-10T21:38:32Z
```

# Bump version to 0.6.2

---

_Pull request opened by @dhruvmanila on 2024-08-22 13:21_

_No description provided._

---

_Label `release` added by @dhruvmanila on 2024-08-22 13:21_

---

_Merged by @dhruvmanila on 2024-08-22 13:29_

---

_Closed by @dhruvmanila on 2024-08-22 13:29_

---

_Branch deleted on 2024-08-22 13:29_

---

_Comment by @github-actions[bot] on 2024-08-22 13:41_

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
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use context handler for opening files
+ dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use a context manager for opening files
- dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/docs/exts/operators_and_hooks_ref.py#L181'>docs/exts/operators_and_hooks_ref.py:181:25:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/docs/exts/operators_and_hooks_ref.py#L181'>docs/exts/operators_and_hooks_ref.py:181:25:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/scripts/ci/pre_commit/check_deferrable_default.py#L66'>scripts/ci/pre_commit/check_deferrable_default.py:66:25:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/scripts/ci/pre_commit/check_deferrable_default.py#L66'>scripts/ci/pre_commit/check_deferrable_default.py:66:25:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/scripts/ci/pre_commit/validate_operators_init.py#L227'>scripts/ci/pre_commit/validate_operators_init.py:227:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/scripts/ci/pre_commit/validate_operators_init.py#L227'>scripts/ci/pre_commit/validate_operators_init.py:227:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/amazon/aws/example_local_to_s3.py#L40'>tests/system/providers/amazon/aws/example_local_to_s3.py:40:12:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/amazon/aws/example_local_to_s3.py#L40'>tests/system/providers/amazon/aws/example_local_to_s3.py:40:12:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_cohere.py#L57'>tests/system/providers/weaviate/example_weaviate_cohere.py:57:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_cohere.py#L57'>tests/system/providers/weaviate/example_weaviate_cohere.py:57:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_cohere.py#L72'>tests/system/providers/weaviate/example_weaviate_cohere.py:72:26:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_cohere.py#L72'>tests/system/providers/weaviate/example_weaviate_cohere.py:72:26:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py#L56'>tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py:56:27:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py#L56'>tests/system/providers/weaviate/example_weaviate_dynamic_mapping_dag.py:56:27:</a> SIM115 Use context handler for opening files
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
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use context handler for opening files
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
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/models/dag.py#L793'>airflow/models/dag.py:793:24:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/hooks/s3.py#L1451'>airflow/providers/amazon/aws/hooks/s3.py:1451:20:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/hooks/s3.py#L1453'>airflow/providers/amazon/aws/hooks/s3.py:1453:20:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py#L247'>airflow/providers/amazon/aws/transfers/dynamodb_to_s3.py:247:29:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/docker/operators/docker.py#L487'>airflow/providers/docker/operators/docker.py:487:19:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L183'>airflow/providers/ftp/hooks/ftp.py:183:33:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/ftp/hooks/ftp.py#L216'>airflow/providers/ftp/hooks/ftp.py:216:28:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/hooks/cloud_sql.py#L934'>airflow/providers/google/cloud/hooks/cloud_sql.py:934:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L195'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:195:27:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L212'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:212:35:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/transfers/cassandra_to_gcs.py#L228'>airflow/providers/google/cloud/transfers/cassandra_to_gcs.py:228:34:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L354'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:354:27:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/providers/google/cloud/transfers/sql_to_gcs.py#L465'>airflow/providers/google/cloud/transfers/sql_to_gcs.py:465:34:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/utils/file.py#L159'>airflow/utils/file.py:159:30:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/airflow/utils/file.py#L161'>airflow/utils/file.py:161:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/console.py#L86'>dev/breeze/src/airflow_breeze/utils/console.py:86:16:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L185'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:185:24:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/breeze/src/airflow_breeze/utils/parallel.py#L62'>dev/breeze/src/airflow_breeze/utils/parallel.py:62:12:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/airflow/blob/63f9c9f2ae10184cbba57302653253f34db1dddc/dev/send_email.py#L79'>dev/send_email.py:79:32:</a> SIM115 Use context handler for opening files
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
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use a context manager for opening files
- <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> SIM115 Use context handler for opening files
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/superset/commands/dataset/importers/v1/utils.py#L198'>superset/commands/dataset/importers/v1/utils.py:198:16:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/tests/integration_tests/email_tests.py#L160'>tests/integration_tests/email_tests.py:160:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/tests/integration_tests/email_tests.py#L44'>tests/integration_tests/email_tests.py:44:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/tests/integration_tests/email_tests.py#L64'>tests/integration_tests/email_tests.py:64:22:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/apache/superset/blob/c049771a7f8ccc048aea4ea298759615a56f3538/tests/integration_tests/email_tests.py#L95'>tests/integration_tests/email_tests.py:95:22:</a> SIM115 Use a context manager for opening files
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

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
