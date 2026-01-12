```yaml
number: 9000
title: "Ignore `@overrides` and `@overloads` for `too-many-positional`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/override
created_at: 2023-12-04T23:32:41Z
updated_at: 2023-12-04T23:45:13Z
url: https://github.com/astral-sh/ruff/pull/9000
synced_at: 2026-01-10T23:40:55Z
```

# Ignore `@overrides` and `@overloads` for `too-many-positional`

---

_Pull request opened by @charliermarsh on 2023-12-04 23:32_

Same as `too-many-arguments`.

---

_Marked ready for review by @charliermarsh on 2023-12-04 23:32_

---

_Label `bug` added by @charliermarsh on 2023-12-04 23:32_

---

_Merged by @charliermarsh on 2023-12-04 23:38_

---

_Closed by @charliermarsh on 2023-12-04 23:38_

---

_Branch deleted on 2023-12-04 23:38_

---

_Comment by @github-actions[bot] on 2023-12-04 23:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -59 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/decorators/task_group.py#L179'>airflow/decorators/task_group.py:179:5:</a> PLR0917 Too many positional arguments: (9/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/models/xcom.py#L170'>airflow/models/xcom.py:170:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/models/xcom.py#L357'>airflow/models/xcom.py:357:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/models/xcom.py#L467'>airflow/models/xcom.py:467:9:</a> PLR0917 Too many positional arguments: (8/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/common/sql/hooks/sql.py#L288'>airflow/providers/common/sql/hooks/sql.py:288:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/common/sql/hooks/sql.py#L300'>airflow/providers/common/sql/hooks/sql.py:300:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/databricks/hooks/databricks_sql.py#L149'>airflow/providers/databricks/hooks/databricks_sql.py:149:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/databricks/hooks/databricks_sql.py#L161'>airflow/providers/databricks/hooks/databricks_sql.py:161:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/exasol/hooks/exasol.py#L166'>airflow/providers/exasol/hooks/exasol.py:166:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/exasol/hooks/exasol.py#L178'>airflow/providers/exasol/hooks/exasol.py:178:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/mongo/hooks/mongo.py#L146'>airflow/providers/mongo/hooks/mongo.py:146:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/mongo/hooks/mongo.py#L158'>airflow/providers/mongo/hooks/mongo.py:158:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/apache/airflow/blob/7ececfdb2183516d9a30195ffcd76632167119c5/airflow/providers/snowflake/hooks/snowflake.py#L304'>airflow/providers/snowflake/hooks/snowflake.py:304:9:</a> PLR0917 Too many positional arguments: (8/5)
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/fa4dcbc646b1e33591ba9df61bf0d16807196ace/samcli/cli/global_config.py#L150'>samcli/cli/global_config.py:150:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/fa4dcbc646b1e33591ba9df61bf0d16807196ace/samcli/cli/global_config.py#L162'>samcli/cli/global_config.py:162:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/fa4dcbc646b1e33591ba9df61bf0d16807196ace/samcli/cli/global_config.py#L174'>samcli/cli/global_config.py:174:9:</a> PLR0917 Too many positional arguments: (6/5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/11e03fa7003ff05bfc243a7cd16b0f791b488737/ibis/expr/api.py#L636'>ibis/expr/api.py:636:5:</a> PLR0917 Too many positional arguments: (7/5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -26 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L11921'>pandas/core/frame.py:11921:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L11932'>pandas/core/frame.py:11932:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L11943'>pandas/core/frame.py:11943:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L1211'>pandas/core/frame.py:1211:9:</a> PLR0917 Too many positional arguments: (20/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L1236'>pandas/core/frame.py:1236:9:</a> PLR0917 Too many positional arguments: (20/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L2944'>pandas/core/frame.py:2944:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L2957'>pandas/core/frame.py:2957:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L3171'>pandas/core/frame.py:3171:9:</a> PLR0917 Too many positional arguments: (24/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/frame.py#L3200'>pandas/core/frame.py:3200:9:</a> PLR0917 Too many positional arguments: (24/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/generic.py#L3332'>pandas/core/generic.py:3332:9:</a> PLR0917 Too many positional arguments: (22/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/generic.py#L3359'>pandas/core/generic.py:3359:9:</a> PLR0917 Too many positional arguments: (22/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/generic.py#L3751'>pandas/core/generic.py:3751:9:</a> PLR0917 Too many positional arguments: (22/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/generic.py#L3778'>pandas/core/generic.py:3778:9:</a> PLR0917 Too many positional arguments: (22/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/series.py#L1755'>pandas/core/series.py:1755:9:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/series.py#L1771'>pandas/core/series.py:1771:9:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/tools/datetimes.py#L624'>pandas/core/tools/datetimes.py:624:5:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/tools/datetimes.py#L641'>pandas/core/tools/datetimes.py:641:5:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/core/tools/datetimes.py#L658'>pandas/core/tools/datetimes.py:658:5:</a> PLR0917 Too many positional arguments: (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/io/json/_json.py#L100'>pandas/io/json/_json.py:100:5:</a> PLR0917 Too many positional arguments: (14/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/io/json/_json.py#L120'>pandas/io/json/_json.py:120:5:</a> PLR0917 Too many positional arguments: (14/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/io/sql.py#L265'>pandas/io/sql.py:265:5:</a> PLR0917 Too many positional arguments: (9/5)
- <a href='https://github.com/pandas-dev/pandas/blob/daa9cdb1c70932afd6d5146c98b48bd95b1c5dd1/pandas/io/sql.py#L280'>pandas/io/sql.py:280:5:</a> PLR0917 Too many positional arguments: (9/5)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/accounting/cost_basis/base.py#L627'>rotkehlchen/accounting/cost_basis/base.py:627:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/accounting/cost_basis/base.py#L639'>rotkehlchen/accounting/cost_basis/base.py:639:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/accounting/cost_basis/base.py#L651'>rotkehlchen/accounting/cost_basis/base.py:651:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/db/history_events.py#L443'>rotkehlchen/db/history_events.py:443:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/db/history_events.py#L454'>rotkehlchen/db/history_events.py:454:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/rotki/rotki/blob/0eb42c47e4579217582757c751972e6aca8fd7aa/rotkehlchen/db/history_events.py#L465'>rotkehlchen/db/history_events.py:465:9:</a> PLR0917 Too many positional arguments: (6/5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/forms.py#L355'>zerver/forms.py:355:9:</a> PLR0917 Too many positional arguments: (10/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/lib/test_runner.py#L359'>zerver/lib/test_runner.py:359:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/lib/upload/local.py#L87'>zerver/lib/upload/local.py:87:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/lib/upload/s3.py#L220'>zerver/lib/upload/s3.py:220:9:</a> PLR0917 Too many positional arguments: (7/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/tests/test_auth_backends.py#L3467'>zerver/tests/test_auth_backends.py:3467:9:</a> PLR0917 Too many positional arguments: (10/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/tests/test_auth_backends.py#L3918'>zerver/tests/test_auth_backends.py:3918:9:</a> PLR0917 Too many positional arguments: (6/5)
- <a href='https://github.com/zulip/zulip/blob/f020f9eee0706e8bbf050e1189eadf579c761da4/zerver/tornado/django_api.py#L36'>zerver/tornado/django_api.py:36:9:</a> PLR0917 Too many positional arguments: (7/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 59 | 0 | 59 | 0 | 0 |

</p>
</details>




---
