```yaml
number: 8954
title: "Ignore `@overload` and `@override` methods for too-many-arguments checks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/override
created_at: 2023-12-01T18:16:35Z
updated_at: 2023-12-01T18:28:41Z
url: https://github.com/astral-sh/ruff/pull/8954
synced_at: 2026-01-12T15:55:27Z
```

# Ignore `@overload` and `@override` methods for too-many-arguments checks

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/8945.

---

_Label `bug` added by @charliermarsh on 2023-12-01 18:16_

---

_Marked ready for review by @charliermarsh on 2023-12-01 18:16_

---

_Merged by @charliermarsh on 2023-12-01 18:22_

---

_Closed by @charliermarsh on 2023-12-01 18:22_

---

_Branch deleted on 2023-12-01 18:22_

---

_Comment by @github-actions[bot] on 2023-12-01 18:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -41 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L104'>airflow/decorators/__init__.pyi:104:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L202'>airflow/decorators/__init__.pyi:202:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L583'>airflow/decorators/__init__.pyi:583:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/task_group.py#L179'>airflow/decorators/task_group.py:179:5:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/dag.py#L1721'>airflow/models/dag.py:1721:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/dag.py#L1739'>airflow/models/dag.py:1739:9:</a> PLR0913 Too many arguments in function definition (16 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L141'>airflow/models/xcom.py:141:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L170'>airflow/models/xcom.py:170:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L314'>airflow/models/xcom.py:314:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L357'>airflow/models/xcom.py:357:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L428'>airflow/models/xcom.py:428:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L467'>airflow/models/xcom.py:467:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/common/sql/hooks/sql.py#L288'>airflow/providers/common/sql/hooks/sql.py:288:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/common/sql/hooks/sql.py#L300'>airflow/providers/common/sql/hooks/sql.py:300:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/databricks/hooks/databricks_sql.py#L149'>airflow/providers/databricks/hooks/databricks_sql.py:149:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/databricks/hooks/databricks_sql.py#L161'>airflow/providers/databricks/hooks/databricks_sql.py:161:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/exasol/hooks/exasol.py#L166'>airflow/providers/exasol/hooks/exasol.py:166:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/exasol/hooks/exasol.py#L178'>airflow/providers/exasol/hooks/exasol.py:178:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/google/cloud/hooks/gcs.py#L271'>airflow/providers/google/cloud/hooks/gcs.py:271:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/google/cloud/hooks/gcs.py#L284'>airflow/providers/google/cloud/hooks/gcs.py:284:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/mongo/hooks/mongo.py#L146'>airflow/providers/mongo/hooks/mongo.py:146:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/mongo/hooks/mongo.py#L158'>airflow/providers/mongo/hooks/mongo.py:158:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/snowflake/hooks/snowflake.py#L304'>airflow/providers/snowflake/hooks/snowflake.py:304:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/snowflake/hooks/snowflake.py#L317'>airflow/providers/snowflake/hooks/snowflake.py:317:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/vertica/hooks/vertica.py#L136'>airflow/providers/vertica/hooks/vertica.py:136:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/vertica/hooks/vertica.py#L148'>airflow/providers/vertica/hooks/vertica.py:148:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/forms.py#L355'>zerver/forms.py:355:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L215'>zerver/lib/request.py:215:5:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L231'>zerver/lib/request.py:231:5:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L247'>zerver/lib/request.py:247:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L262'>zerver/lib/request.py:262:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L277'>zerver/lib/request.py:277:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/test_runner.py#L359'>zerver/lib/test_runner.py:359:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/upload/local.py#L87'>zerver/lib/upload/local.py:87:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/upload/s3.py#L220'>zerver/lib/upload/s3.py:220:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L1955'>zerver/tests/test_auth_backends.py:1955:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3467'>zerver/tests/test_auth_backends.py:3467:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3509'>zerver/tests/test_auth_backends.py:3509:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3918'>zerver/tests/test_auth_backends.py:3918:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tornado/django_api.py#L36'>zerver/tornado/django_api.py:36:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0913 | 41 | 0 | 41 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -40 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L104'>airflow/decorators/__init__.pyi:104:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L202'>airflow/decorators/__init__.pyi:202:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/__init__.pyi#L583'>airflow/decorators/__init__.pyi:583:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/decorators/task_group.py#L179'>airflow/decorators/task_group.py:179:5:</a> PLR0913 Too many arguments in function definition (9 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/dag.py#L1721'>airflow/models/dag.py:1721:9:</a> PLR0913 Too many arguments in function definition (12 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/dag.py#L1739'>airflow/models/dag.py:1739:9:</a> PLR0913 Too many arguments in function definition (16 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L141'>airflow/models/xcom.py:141:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L170'>airflow/models/xcom.py:170:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L314'>airflow/models/xcom.py:314:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L357'>airflow/models/xcom.py:357:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L428'>airflow/models/xcom.py:428:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/models/xcom.py#L467'>airflow/models/xcom.py:467:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/common/sql/hooks/sql.py#L288'>airflow/providers/common/sql/hooks/sql.py:288:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/common/sql/hooks/sql.py#L300'>airflow/providers/common/sql/hooks/sql.py:300:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/databricks/hooks/databricks_sql.py#L149'>airflow/providers/databricks/hooks/databricks_sql.py:149:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/databricks/hooks/databricks_sql.py#L161'>airflow/providers/databricks/hooks/databricks_sql.py:161:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/exasol/hooks/exasol.py#L166'>airflow/providers/exasol/hooks/exasol.py:166:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/exasol/hooks/exasol.py#L178'>airflow/providers/exasol/hooks/exasol.py:178:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/mongo/hooks/mongo.py#L146'>airflow/providers/mongo/hooks/mongo.py:146:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/mongo/hooks/mongo.py#L158'>airflow/providers/mongo/hooks/mongo.py:158:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/snowflake/hooks/snowflake.py#L304'>airflow/providers/snowflake/hooks/snowflake.py:304:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/snowflake/hooks/snowflake.py#L317'>airflow/providers/snowflake/hooks/snowflake.py:317:9:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/vertica/hooks/vertica.py#L136'>airflow/providers/vertica/hooks/vertica.py:136:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/apache/airflow/blob/8d0229b8f635cb87ba1c747b619019d4536ffd5a/airflow/providers/vertica/hooks/vertica.py#L148'>airflow/providers/vertica/hooks/vertica.py:148:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/core/common.py#L237'>pandas/core/common.py:237:46:</a> PLR6201 Use a `set` literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/forms.py#L355'>zerver/forms.py:355:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L215'>zerver/lib/request.py:215:5:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L231'>zerver/lib/request.py:231:5:</a> PLR0913 Too many arguments in function definition (8 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L247'>zerver/lib/request.py:247:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L262'>zerver/lib/request.py:262:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/request.py#L277'>zerver/lib/request.py:277:5:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/test_runner.py#L359'>zerver/lib/test_runner.py:359:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/upload/local.py#L87'>zerver/lib/upload/local.py:87:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/lib/upload/s3.py#L220'>zerver/lib/upload/s3.py:220:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L1955'>zerver/tests/test_auth_backends.py:1955:9:</a> PLR0913 Too many arguments in function definition (10 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3467'>zerver/tests/test_auth_backends.py:3467:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3509'>zerver/tests/test_auth_backends.py:3509:9:</a> PLR0913 Too many arguments in function definition (11 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tests/test_auth_backends.py#L3918'>zerver/tests/test_auth_backends.py:3918:9:</a> PLR0913 Too many arguments in function definition (6 > 5)
- <a href='https://github.com/zulip/zulip/blob/5d49e54d33014b9ef5a515df2483b87d2b1d4aeb/zerver/tornado/django_api.py#L36'>zerver/tornado/django_api.py:36:9:</a> PLR0913 Too many arguments in function definition (7 > 5)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0913 | 39 | 0 | 39 | 0 | 0 |
| PLR6201 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---
