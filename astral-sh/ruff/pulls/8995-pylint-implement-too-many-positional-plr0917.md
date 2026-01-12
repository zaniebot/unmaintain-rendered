```yaml
number: 8995
title: "[`pylint`] Implement `too-many-positional` (`PLR0917`)"
type: pull_request
state: merged
author: flying-sheep
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-too-many-positional
created_at: 2023-12-04T10:53:46Z
updated_at: 2023-12-05T08:03:57Z
url: https://github.com/astral-sh/ruff/pull/8995
synced_at: 2026-01-12T15:55:27Z
```

# [`pylint`] Implement `too-many-positional` (`PLR0917`)

---

_@flying-sheep_

## Summary

Adds a rule that bans too many positional (i.e. not keyword-only) parameters in function definitions.

Fixes https://github.com/astral-sh/ruff/issues/8946

Rule ID code taken from https://github.com/pylint-dev/pylint/pull/9278

## Test Plan
1. fixtures file checking multiple OKs/fails
2. parametrized test file


---

_Review requested from @charliermarsh by @charliermarsh on 2023-12-04 15:17_

---

_Label `rule` added by @charliermarsh on 2023-12-04 15:17_

---

_Label `preview` added by @charliermarsh on 2023-12-04 15:17_

---

_@charliermarsh approved on 2023-12-04 17:31_

Looks good to me, just gonna tweak a bit before merging. This'll go out later today.

---

_Renamed from "Add too-many-positional rule" to "[`pylint`] Implement `too-many-positional` (`PLR0917`)" by @charliermarsh on 2023-12-04 17:54_

---

_Merged by @charliermarsh on 2023-12-04 18:03_

---

_Closed by @charliermarsh on 2023-12-04 18:03_

---

_Comment by @github-actions[bot] on 2023-12-04 18:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3176 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+32 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/argx.py#L311'>aiven/client/argx.py:311:9:</a> PLR0917 Too many positional arguments: (10/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/cli.py#L3724'>aiven/client/cli.py:3724:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/cli.py#L5717'>aiven/client/cli.py:5717:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1062'>aiven/client/client.py:1062:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1072'>aiven/client/client.py:1072:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1148'>aiven/client/client.py:1148:9:</a> PLR0917 Too many positional arguments: (10/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1188'>aiven/client/client.py:1188:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1213'>aiven/client/client.py:1213:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1298'>aiven/client/client.py:1298:9:</a> PLR0917 Too many positional arguments: (12/5)
+ <a href='https://github.com/aiven/aiven-client/blob/1f64f18fc1631bf5f6d8102b846f63ea60439054/aiven/client/client.py#L1334'>aiven/client/client.py:1334:9:</a> PLR0917 Too many positional arguments: (13/5)
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1169 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api/client/json_client.py#L57'>airflow/api/client/json_client.py:57:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api/client/local_client.py#L31'>airflow/api/client/local_client.py:31:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api/common/mark_tasks.py#L209'>airflow/api/common/mark_tasks.py:209:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api/common/trigger_dag.py#L34'>airflow/api/common/trigger_dag.py:34:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/api_connexion/endpoints/dag_endpoint.py#L144'>airflow/api_connexion/endpoints/dag_endpoint.py:144:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/auth/managers/fab/security_manager/override.py#L1437'>airflow/auth/managers/fab/security_manager/override.py:1437:9:</a> PLR0917 Too many positional arguments: (8/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/auth/managers/fab/security_manager/override.py#L1480'>airflow/auth/managers/fab/security_manager/override.py:1480:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0917 Too many positional arguments: (11/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1050'>airflow/configuration.py:1050:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1071'>airflow/configuration.py:1071:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1092'>airflow/configuration.py:1092:9:</a> PLR0917 Too many positional arguments: (8/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1114'>airflow/configuration.py:1114:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1366'>airflow/configuration.py:1366:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/35a1b7a63a7e9eab299955e0b35f2fd3614b22ee/airflow/configuration.py#L1599'>airflow/configuration.py:1599:9:</a> PLR0917 Too many positional arguments: (9/5)
... 1151 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+391 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/cli_config_file.py#L176'>samcli/cli/cli_config_file.py:176:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/global_config.py#L150'>samcli/cli/global_config.py:150:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/global_config.py#L162'>samcli/cli/global_config.py:162:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/global_config.py#L174'>samcli/cli/global_config.py:174:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/global_config.py#L184'>samcli/cli/global_config.py:184:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/cli/global_config.py#L223'>samcli/cli/global_config.py:223:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/commands/_utils/options.py#L184'>samcli/commands/_utils/options.py:184:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/commands/_utils/table_print.py#L103'>samcli/commands/_utils/table_print.py:103:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/commands/_utils/table_print.py#L15'>samcli/commands/_utils/table_print.py:15:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/b424910a64448d941c2780381df1e65606dcb443/samcli/commands/delete/command.py#L119'>samcli/commands/delete/command.py:119:5:</a> PLR0917 Too many positional arguments: (8/5)
... 381 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+54 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/colorbar_log.py#L15'>examples/basic/annotations/colorbar_log.py:15:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/gauges.py#L48'>examples/models/gauges.py:48:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/histogram.py#L17'>examples/plotting/histogram.py:17:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/topics/hierarchical/treemap.py#L26'>examples/topics/hierarchical/treemap.py:26:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/client/session.py#L194'>src/bokeh/client/session.py:194:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/client/session.py#L273'>src/bokeh/client/session.py:273:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/wrappers.py#L438'>src/bokeh/core/property/wrappers.py:438:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/document/callbacks.py#L223'>src/bokeh/document/callbacks.py:223:9:</a> PLR0917 Too many positional arguments: (8/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/document/events.py#L258'>src/bokeh/document/events.py:258:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/document/events.py#L290'>src/bokeh/document/events.py:290:9:</a> PLR0917 Too many positional arguments: (7/5)
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+15 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/models.py#L425'>securedrop/models.py:425:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/models.py#L78'>securedrop/models.py:78:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/pretty_bad_protocol/_meta.py#L135'>securedrop/pretty_bad_protocol/_meta.py:135:9:</a> PLR0917 Too many positional arguments: (10/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/pretty_bad_protocol/_meta.py#L752'>securedrop/pretty_bad_protocol/_meta.py:752:111:</a> RUF100 [*] Unused blanket `noqa` directive
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/pretty_bad_protocol/_meta.py#L785'>securedrop/pretty_bad_protocol/_meta.py:785:9:</a> PLR0917 Too many positional arguments: (8/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/8acc76905d4ce392873aebd92037f448e668efaa/securedrop/pretty_bad_protocol/_meta.py#L857'>securedrop/pretty_bad_protocol/_meta.py:857:9:</a> PLR0917 Too many positional arguments: (15/5)
... 10 additional changes omitted for rule PLR0917
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/api.py#L461'>blinkpy/api.py:461:11:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/api.py#L483'>blinkpy/api.py:483:11:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/auth.py#L144'>blinkpy/auth.py:144:15:</a> PLR0917 Too many positional arguments: (9/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/blinkpy.py#L322'>blinkpy/blinkpy.py:322:15:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/blinkpy.py#L392'>blinkpy/blinkpy.py:392:15:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinkpy/sync_module.py#L639'>blinkpy/sync_module.py:639:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/tests/test_blinkpy.py#L196'>tests/test_blinkpy.py:196:15:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/tests/test_blinkpy.py#L543'>tests/test_blinkpy.py:543:15:</a> PLR0917 Too many positional arguments: (8/5)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+103 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/alchemy/__init__.py#L139'>ibis/backends/base/sql/alchemy/__init__.py:139:9:</a> PLR0917 Too many positional arguments: (8/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/alchemy/__init__.py#L73'>ibis/backends/base/sql/alchemy/__init__.py:73:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/compiler/query_builder.py#L192'>ibis/backends/base/sql/compiler/query_builder.py:192:9:</a> PLR0917 Too many positional arguments: (15/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/compiler/select_builder.py#L26'>ibis/backends/base/sql/compiler/select_builder.py:26:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/compiler/translator.py#L192'>ibis/backends/base/sql/compiler/translator.py:192:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/ddl.py#L118'>ibis/backends/base/sql/ddl.py:118:9:</a> PLR0917 Too many positional arguments: (9/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/ddl.py#L168'>ibis/backends/base/sql/ddl.py:168:9:</a> PLR0917 Too many positional arguments: (9/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/ddl.py#L325'>ibis/backends/base/sql/ddl.py:325:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/ddl.py#L361'>ibis/backends/base/sql/ddl.py:361:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/9a4d1f8502ab81edba3679dea38df458a3f2bdbc/ibis/backends/base/sql/ddl.py#L405'>ibis/backends/base/sql/ddl.py:405:9:</a> PLR0917 Too many positional arguments: (6/5)
... 93 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+38 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/bulk_import.py#L79'>pymilvus/bulk_writer/bulk_import.py:79:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/remote_bulk_writer.py#L36'>pymilvus/bulk_writer/remote_bulk_writer.py:36:13:</a> PLR0917 Too many positional arguments: (10/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/bulk_writer/remote_bulk_writer.py#L58'>pymilvus/bulk_writer/remote_bulk_writer.py:58:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/abstract.py#L389'>pymilvus/client/abstract.py:389:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1281'>pymilvus/client/grpc_handler.py:1281:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1295'>pymilvus/client/grpc_handler.py:1295:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1338'>pymilvus/client/grpc_handler.py:1338:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1595'>pymilvus/client/grpc_handler.py:1595:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1617'>pymilvus/client/grpc_handler.py:1617:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/milvus-io/pymilvus/blob/161210ff0528b9080f66e1d36af7478423009ab8/pymilvus/client/grpc_handler.py#L1648'>pymilvus/client/grpc_handler.py:1648:9:</a> PLR0917 Too many positional arguments: (6/5)
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+694 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/groupby.py#L490'>asv_bench/benchmarks/groupby.py:490:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/groupby.py#L573'>asv_bench/benchmarks/groupby.py:573:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/groupby.py#L576'>asv_bench/benchmarks/groupby.py:576:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L108'>asv_bench/benchmarks/rolling.py:108:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L123'>asv_bench/benchmarks/rolling.py:123:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L214'>asv_bench/benchmarks/rolling.py:214:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L219'>asv_bench/benchmarks/rolling.py:219:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L241'>asv_bench/benchmarks/rolling.py:241:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L246'>asv_bench/benchmarks/rolling.py:246:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/7b528c91e5e273f38ea779b548ca64e80df4b346/asv_bench/benchmarks/rolling.py#L41'>asv_bench/benchmarks/rolling.py:41:9:</a> PLR0917 Too many positional arguments: (6/5)
... 684 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 3176 | 3176 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Branch deleted on 2023-12-05 08:03_

---
