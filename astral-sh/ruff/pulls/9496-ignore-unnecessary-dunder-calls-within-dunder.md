```yaml
number: 9496
title: Ignore unnecessary dunder calls within dunder definitions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dunder
created_at: 2024-01-12T19:32:40Z
updated_at: 2024-01-12T19:48:43Z
url: https://github.com/astral-sh/ruff/pull/9496
synced_at: 2026-01-12T15:55:29Z
```

# Ignore unnecessary dunder calls within dunder definitions

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/9486.

---

_Label `bug` added by @charliermarsh on 2024-01-12 19:32_

---

_Comment by @codspeed-hq[bot] on 2024-01-12 19:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/dunder)

### Merging #9496 will **degrade performances by 4.33%**

<sub>Comparing <code>charlie/dunder</code> (8aafed0) with <code>main</code> (7504bf3)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/dunder)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/dunder` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.33% |


---

_Comment by @github-actions[bot] on 2024-01-12 19:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -229 violations, +0 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/activity.py#L479'>disnake/activity.py:479:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/activity.py#L591'>disnake/activity.py:591:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/activity.py#L695'>disnake/activity.py:695:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/activity.py#L870'>disnake/activity.py:870:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/colour.py#L68'>disnake/colour.py:68:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/context_managers.py#L60'>disnake/context_managers.py:60:16:</a> PLC2801 Unnecessary dunder call to `__enter__`
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/emoji.py#L128'>disnake/emoji.py:128:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/ext/commands/params.py#L1348'>disnake/ext/commands/params.py:1348:20:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/flags.py#L154'>disnake/flags.py:154:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
- <a href='https://github.com/DisnakeDev/disnake/blob/a5c1a8f902e9a7e173f71e65827d59de8a445adb/disnake/member.py#L359'>disnake/member.py:359:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/argx.py#L189'>aiven/client/argx.py:189:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L141'>aiven/client/cli.py:141:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L34'>aiven/client/client.py:34:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/configuration.py#L1887'>airflow/configuration.py:1887:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/models/baseoperator.py#L192'>airflow/models/baseoperator.py:192:16:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/models/taskmixin.py#L109'>airflow/models/taskmixin.py:109:9:</a> PLC2801 [*] Unnecessary dunder call to `__lshift__`. Use `<<` operator.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/models/taskmixin.py#L114'>airflow/models/taskmixin.py:114:9:</a> PLC2801 [*] Unnecessary dunder call to `__rshift__`. Use `>>` operator.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers/elasticsearch/log/es_response.py#L57'>airflow/providers/elasticsearch/log/es_response.py:57:20:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers/microsoft/psrp/hooks/psrp.py#L105'>airflow/providers/microsoft/psrp/hooks/psrp.py:105:9:</a> PLC2801 Unnecessary dunder call to `__enter__`
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers/microsoft/psrp/hooks/psrp.py#L106'>airflow/providers/microsoft/psrp/hooks/psrp.py:106:9:</a> PLC2801 Unnecessary dunder call to `__enter__`
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers_manager.py#L107'>airflow/providers_manager.py:107:9:</a> PLC2801 Unnecessary dunder call to `__setitem__`. Use subscript assignment.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers_manager.py#L110'>airflow/providers_manager.py:110:17:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/apache/airflow/blob/142f08abb5fad30fd0d0d79f270b826793b273d7/airflow/providers_manager.py#L116'>airflow/providers_manager.py:116:13:</a> PLC2801 Unnecessary dunder call to `__setitem__`. Use subscript assignment.
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/commands/exceptions.py#L28'>samcli/commands/exceptions.py:28:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/commands/exceptions.py#L47'>samcli/commands/exceptions.py:47:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/commands/local/lib/debug_context.py#L31'>samcli/commands/local/lib/debug_context.py:31:16:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L19'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:19:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L34'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:34:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L392'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:392:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L51'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:51:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L74'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:74:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/lib/cookiecutter/exceptions.py#L9'>samcli/lib/cookiecutter/exceptions.py:9:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/aws/aws-sam-cli/blob/012963dbc3286147823f25905f2cfa549414d3c6/samcli/lib/docker/log_streamer.py#L20'>samcli/lib/docker/log_streamer.py:20:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:16:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L909'>src/bokeh/models/plots.py:909:20:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/journalist_gui/journalist_gui/SecureDropUpdater.py#L125'>journalist_gui/journalist_gui/SecureDropUpdater.py:125:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/journalist_gui/journalist_gui/SecureDropUpdater.py#L49'>journalist_gui/journalist_gui/SecureDropUpdater.py:49:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/journalist_gui/journalist_gui/SecureDropUpdater.py#L89'>journalist_gui/journalist_gui/SecureDropUpdater.py:89:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/journalist_app/sessions.py#L31'>securedrop/journalist_app/sessions.py:31:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/pretty_bad_protocol/_parsers.py#L1774'>securedrop/pretty_bad_protocol/_parsers.py:1774:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/migrations/migration_a9fe328b053a.py#L50'>securedrop/tests/migrations/migration_a9fe328b053a.py:50:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/migrations/migration_a9fe328b053a.py#L72'>securedrop/tests/migrations/migration_a9fe328b053a.py:72:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/migrations/migration_b58139cfdc8c.py#L121'>securedrop/tests/migrations/migration_b58139cfdc8c.py:121:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/migrations/migration_b58139cfdc8c.py#L165'>securedrop/tests/migrations/migration_b58139cfdc8c.py:165:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/freedomofpress/securedrop/blob/8010052d516d3cacc4032ac7e05f32099aa38cd3/securedrop/tests/migrations/migration_c5a02eb52f2d.py#L19'>securedrop/tests/migrations/migration_c5a02eb52f2d.py:19:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/7d0e1f6cbb4b571c556e1e7c8ee1d87d1f0257d3/blinkpy/sync_module.py#L767'>blinkpy/sync_module.py:767:16:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -21 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L198'>ibis/common/bases.py:198:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L212'>ibis/common/bases.py:212:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L239'>ibis/common/bases.py:239:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L241'>ibis/common/bases.py:241:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L245'>ibis/common/bases.py:245:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/bases.py#L247'>ibis/common/bases.py:247:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/caching.py#L39'>ibis/common/caching.py:39:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/collections.py#L229'>ibis/common/collections.py:229:43:</a> PLC2801 [*] Unnecessary dunder call to `__ge__`. Use `>=` operator.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/collections.py#L240'>ibis/common/collections.py:240:43:</a> PLC2801 [*] Unnecessary dunder call to `__le__`. Use `<=` operator.
- <a href='https://github.com/ibis-project/ibis/blob/1ea08141744e4fa76e83928cfd696997385fbfcd/ibis/common/collections.py#L295'>ibis/common/collections.py:295:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L179'>pymilvus/client/abstract.py:179:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L282'>pymilvus/client/abstract.py:282:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L360'>pymilvus/client/abstract.py:360:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L627'>pymilvus/client/abstract.py:627:35:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L627'>pymilvus/client/abstract.py:627:69:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L632'>pymilvus/client/abstract.py:632:20:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L639'>pymilvus/client/abstract.py:639:30:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L641'>pymilvus/client/abstract.py:641:20:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/abstract.py#L648'>pymilvus/client/abstract.py:648:34:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/milvus-io/pymilvus/blob/1036ad834d2530dcd298e559f5da309962d66015/pymilvus/client/ts_utils.py#L22'>pymilvus/client/ts_utils.py:22:16:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -52 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/_config/config.py#L227'>pandas/_config/config.py:227:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/_config/config.py#L228'>pandas/_config/config.py:228:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/_config/config.py#L231'>pandas/_config/config.py:231:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/_config/config.py#L243'>pandas/_config/config.py:243:18:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/_config/config.py#L248'>pandas/_config/config.py:248:17:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/conftest.py#L598'>pandas/conftest.py:598:20:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/core/_numba/extensions.py#L188'>pandas/core/_numba/extensions.py:188:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/core/_numba/extensions.py#L199'>pandas/core/_numba/extensions.py:199:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/core/_numba/extensions.py#L563'>pandas/core/_numba/extensions.py:563:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/core/accessor.py#L229'>pandas/core/accessor.py:229:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function.
- <a href='https://github.com/pandas-dev/pandas/blob/612823e824805b97a2dbe258ba808dc572083d49/pandas/core/arrays/arrow/extension_types.py#L26'>pandas/core/arrays/arrow/extension_types.py:26:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
... 41 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2801 | 229 | 0 | 229 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-01-12 19:48_

---

_Closed by @charliermarsh on 2024-01-12 19:48_

---

_Branch deleted on 2024-01-12 19:48_

---
