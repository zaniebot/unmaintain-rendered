```yaml
number: 15348
title: "[`flake8-builtins`] Rename `A005` and improve its error message"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: alex/flake8-builtins-message
created_at: 2025-01-08T12:32:16Z
updated_at: 2025-01-09T08:19:59Z
url: https://github.com/astral-sh/ruff/pull/15348
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-builtins`] Rename `A005` and improve its error message

---

_@AlexWaygood_

## Summary

This rule is called `builtin-module-shadowing` and the error message is:

> Module `abc` is shadowing a Python builtin module

But that's not a good description of the rule. There's only one `builtins` module in Python, but many modules in the Python standard library. This rule complains if you shadow any Python _standard-library_ module, so a better name is `stdlib-module-shadowing`, and a better error message would be

> Module `abc` shadows a Python standard-library module

This PR makes that change. We'll need to remember to add a redirect rule to the website, or existing links to the rule's documentation (e.g. in our changelog or blogposts) will break. 

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `rule` added by @AlexWaygood on 2025-01-08 12:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-08 12:32_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-08 12:32_

---

_Comment by @MichaReiser on 2025-01-08 12:34_

>  so a better name is builtin-module-shadowing, and a better error message would be

So the name is already perfect?  ;)

---

_@MichaReiser approved on 2025-01-08 12:35_

---

_Comment by @AlexWaygood on 2025-01-08 12:36_

> > so a better name is builtin-module-shadowing, and a better error message would be
> 
> So the name is already perfect? ;)

Oops. Edited my PR description 😇

---

_Merged by @AlexWaygood on 2025-01-08 12:38_

---

_Closed by @AlexWaygood on 2025-01-08 12:38_

---

_Branch deleted on 2025-01-08 12:38_

---

_Comment by @github-actions[bot] on 2025-01-08 12:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+81 -81 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/_plugins/cortex/types.py#L1'>src/snowflake/cli/_plugins/cortex/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/_plugins/cortex/types.py#L1'>src/snowflake/cli/_plugins/cortex/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/_plugins/notebook/types.py#L1'>src/snowflake/cli/_plugins/notebook/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/_plugins/notebook/types.py#L1'>src/snowflake/cli/_plugins/notebook/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/console/abc.py#L1'>src/snowflake/cli/api/console/abc.py:1:1:</a> A005 Module `abc` is shadowing a Python builtin module
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/console/abc.py#L1'>src/snowflake/cli/api/console/abc.py:1:1:</a> A005 Module `abc` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/console/enum.py#L1'>src/snowflake/cli/api/console/enum.py:1:1:</a> A005 Module `enum` is shadowing a Python builtin module
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/console/enum.py#L1'>src/snowflake/cli/api/console/enum.py:1:1:</a> A005 Module `enum` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/errno.py#L1'>src/snowflake/cli/api/errno.py:1:1:</a> A005 Module `errno` is shadowing a Python builtin module
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/9f47aa3e86e3206d35a75ea46fbc7cbc49dc88e0/src/snowflake/cli/api/errno.py#L1'>src/snowflake/cli/api/errno.py:1:1:</a> A005 Module `errno` shadows a Python standard-library module
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+23 -23 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_connexion/types.py#L1'>airflow/api_connexion/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_connexion/types.py#L1'>airflow/api_connexion/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_fastapi/common/types.py#L1'>airflow/api_fastapi/common/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_fastapi/common/types.py#L1'>airflow/api_fastapi/common/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_fastapi/execution_api/datamodels/token.py#L1'>airflow/api_fastapi/execution_api/datamodels/token.py:1:1:</a> A005 Module `token` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/api_fastapi/execution_api/datamodels/token.py#L1'>airflow/api_fastapi/execution_api/datamodels/token.py:1:1:</a> A005 Module `token` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/io/utils/stat.py#L1'>airflow/io/utils/stat.py:1:1:</a> A005 Module `stat` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/io/utils/stat.py#L1'>airflow/io/utils/stat.py:1:1:</a> A005 Module `stat` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/models/operator.py#L1'>airflow/models/operator.py:1:1:</a> A005 Module `operator` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/models/operator.py#L1'>airflow/models/operator.py:1:1:</a> A005 Module `operator` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/operators/email.py#L1'>airflow/operators/email.py:1:1:</a> A005 Module `email` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/operators/email.py#L1'>airflow/operators/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/serialization/serializers/datetime.py#L1'>airflow/serialization/serializers/datetime.py:1:1:</a> A005 Module `datetime` is shadowing a Python builtin module
+ <a href='https://github.com/apache/airflow/blob/c300e0e7c6b9b74b1171cce9dbc6f683f721c0cc/airflow/serialization/serializers/datetime.py#L1'>airflow/serialization/serializers/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/advanced_data_type/types.py#L1'>superset/advanced_data_type/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/advanced_data_type/types.py#L1'>superset/advanced_data_type/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/commands/dashboard/copy.py#L1'>superset/commands/dashboard/copy.py:1:1:</a> A005 Module `copy` is shadowing a Python builtin module
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/commands/dashboard/copy.py#L1'>superset/commands/dashboard/copy.py:1:1:</a> A005 Module `copy` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/dashboards/permalink/types.py#L1'>superset/dashboards/permalink/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/dashboards/permalink/types.py#L1'>superset/dashboards/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/databases/types.py#L1'>superset/databases/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/databases/types.py#L1'>superset/databases/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/distributed_lock/types.py#L1'>superset/distributed_lock/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/distributed_lock/types.py#L1'>superset/distributed_lock/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L1'>src/bokeh/application/handlers/code.py:1:1:</a> A005 Module `code` is shadowing a Python builtin module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L1'>src/bokeh/application/handlers/code.py:1:1:</a> A005 Module `code` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/json.py#L1'>src/bokeh/command/subcommands/json.py:1:1:</a> A005 Module `json` is shadowing a Python builtin module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/json.py#L1'>src/bokeh/command/subcommands/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/datetime.py#L1'>src/bokeh/core/property/datetime.py:1:1:</a> A005 Module `datetime` is shadowing a Python builtin module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/datetime.py#L1'>src/bokeh/core/property/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L1'>src/bokeh/core/property/enum.py:1:1:</a> A005 Module `enum` is shadowing a Python builtin module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L1'>src/bokeh/core/property/enum.py:1:1:</a> A005 Module `enum` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/json.py#L1'>src/bokeh/core/property/json.py:1:1:</a> A005 Module `json` is shadowing a Python builtin module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/json.py#L1'>src/bokeh/core/property/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/functions/secrets.py#L1'>src/latch/functions/secrets.py:1:1:</a> A005 Module `secrets` is shadowing a Python builtin module
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/functions/secrets.py#L1'>src/latch/functions/secrets.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/types.py#L1'>src/latch/registry/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/types.py#L1'>src/latch/registry/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/upstream_types/types.py#L1'>src/latch/registry/upstream_types/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/upstream_types/types.py#L1'>src/latch/registry/upstream_types/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/glob.py#L1'>src/latch/types/glob.py:1:1:</a> A005 Module `glob` is shadowing a Python builtin module
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/glob.py#L1'>src/latch/types/glob.py:1:1:</a> A005 Module `glob` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/json.py#L1'>src/latch/types/json.py:1:1:</a> A005 Module `json` is shadowing a Python builtin module
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/json.py#L1'>src/latch/types/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/1b555d3b9dbb87e18464f9bf1863e660e1cd4a06/pymilvus/client/types.py#L1'>pymilvus/client/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/milvus-io/pymilvus/blob/1b555d3b9dbb87e18464f9bf1863e660e1cd4a06/pymilvus/client/types.py#L1'>pymilvus/client/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/milvus-io/pymilvus/blob/1b555d3b9dbb87e18464f9bf1863e660e1cd4a06/pymilvus/orm/types.py#L1'>pymilvus/orm/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/milvus-io/pymilvus/blob/1b555d3b9dbb87e18464f9bf1863e660e1cd4a06/pymilvus/orm/types.py#L1'>pymilvus/orm/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/compression/gzip.py#L1'>ch_backup/compression/gzip.py:1:1:</a> A005 Module `gzip` is shadowing a Python builtin module
+ <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/compression/gzip.py#L1'>ch_backup/compression/gzip.py:1:1:</a> A005 Module `gzip` shadows a Python standard-library module
- <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/logging.py#L1'>ch_backup/logging.py:1:1:</a> A005 Module `logging` is shadowing a Python builtin module
+ <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/logging.py#L1'>ch_backup/logging.py:1:1:</a> A005 Module `logging` shadows a Python standard-library module
- <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/profile.py#L1'>ch_backup/profile.py:1:1:</a> A005 Module `profile` is shadowing a Python builtin module
+ <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/profile.py#L1'>ch_backup/profile.py:1:1:</a> A005 Module `profile` shadows a Python standard-library module
- <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/storage/async_pipeline/stages/types.py#L1'>ch_backup/storage/async_pipeline/stages/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/ch_backup/storage/async_pipeline/stages/types.py#L1'>ch_backup/storage/async_pipeline/stages/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/tests/integration/modules/datetime.py#L1'>tests/integration/modules/datetime.py:1:1:</a> A005 Module `datetime` is shadowing a Python builtin module
+ <a href='https://github.com/yandex/ch-backup/blob/d5cb497c1202f4fb53c57d2cf23462b113f36179/tests/integration/modules/datetime.py#L1'>tests/integration/modules/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/actions/typing.py#L1'>zerver/actions/typing.py:1:1:</a> A005 Module `typing` is shadowing a Python builtin module
+ <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/actions/typing.py#L1'>zerver/actions/typing.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/profile.py#L1'>zerver/lib/profile.py:1:1:</a> A005 Module `profile` is shadowing a Python builtin module
+ <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/profile.py#L1'>zerver/lib/profile.py:1:1:</a> A005 Module `profile` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/queue.py#L1'>zerver/lib/queue.py:1:1:</a> A005 Module `queue` is shadowing a Python builtin module
+ <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/queue.py#L1'>zerver/lib/queue.py:1:1:</a> A005 Module `queue` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/types.py#L1'>zerver/lib/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/types.py#L1'>zerver/lib/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/url_preview/types.py#L1'>zerver/lib/url_preview/types.py:1:1:</a> A005 Module `types` is shadowing a Python builtin module
+ <a href='https://github.com/zulip/zulip/blob/f1dd8d88c9ac2f6771d92bcc7d0a3260cf91f5bd/zerver/lib/url_preview/types.py#L1'>zerver/lib/url_preview/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A005 | 162 | 81 | 81 | 0 | 0 |

</p>
</details>




---

_@dhruvmanila reviewed on 2025-01-09 04:03_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:24 on 2025-01-09 04:03_

Do we need to update the settings name as well? (`lint.flake8-builtins.builtins-allowed-modules`) I think not but thought raise it regardless. Aside, the double "builtins" is a bit unfortunate.

---

_@MichaReiser reviewed on 2025-01-09 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:24 on 2025-01-09 07:42_

Hmm, good catch. I think we should

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:24 on 2025-01-09 08:17_

I'd suggest to avoid the rename here and directly rename it to remove the "builtins" prefix from both options. I'll open a tracking issue but happy to hear any suggestions if you think otherwise.

---

_@dhruvmanila reviewed on 2025-01-09 08:17_

---

_@dhruvmanila reviewed on 2025-01-09 08:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:24 on 2025-01-09 08:19_

https://github.com/astral-sh/ruff/issues/15368

---
