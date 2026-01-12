```yaml
number: 9166
title: "[`pylint`] Implement `unnecessary-dunder-call` (`C2801`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-C2801
created_at: 2023-12-16T22:46:19Z
updated_at: 2024-01-03T18:15:06Z
url: https://github.com/astral-sh/ruff/pull/9166
synced_at: 2026-01-12T15:55:28Z
```

# [`pylint`] Implement `unnecessary-dunder-call` (`C2801`)

---

_@diceroll123_

## Summary

Implements [`C2801`/`unnecessary-dunder-calls`](https://pylint.readthedocs.io/en/stable/user_guide/messages/convention/unnecessary-dunder-call.html)

There are more that this could cover, but the implementations get a little less straightforward and ugly. Might come back to it in a future PR, or someone else can!

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-12-16 23:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+491 -0 violations, +0 -0 fixes in 12 projects; 29 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/activity.py#L479'>disnake/activity.py:479:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/activity.py#L591'>disnake/activity.py:591:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/activity.py#L695'>disnake/activity.py:695:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/activity.py#L870'>disnake/activity.py:870:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/colour.py#L68'>disnake/colour.py:68:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/context_managers.py#L60'>disnake/context_managers.py:60:16:</a> PLC2801 Unnecessary dunder call to `__enter__`
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/emoji.py#L128'>disnake/emoji.py:128:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/ext/commands/params.py#L1348'>disnake/ext/commands/params.py:1348:20:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/flags.py#L153'>disnake/flags.py:153:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
+ <a href='https://github.com/DisnakeDev/disnake/blob/6430c2bf9652449930e845f7be3d866a926baaa8/disnake/member.py#L359'>disnake/member.py:359:20:</a> PLC2801 [*] Unnecessary dunder call to `__eq__`. Use `==` operator.
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/argx.py#L189'>aiven/client/argx.py:189:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L141'>aiven/client/cli.py:141:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L34'>aiven/client/client.py:34:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+130 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1887'>airflow/configuration.py:1887:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1962'>airflow/configuration.py:1962:36:</a> PLC2801 Unnecessary dunder call to `__fspath__`. Use `os.fspath` function.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1966'>airflow/configuration.py:1966:62:</a> PLC2801 Unnecessary dunder call to `__fspath__`. Use `os.fspath` function.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1974'>airflow/configuration.py:1974:41:</a> PLC2801 Unnecessary dunder call to `__fspath__`. Use `os.fspath` function.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1979'>airflow/configuration.py:1979:65:</a> PLC2801 Unnecessary dunder call to `__fspath__`. Use `os.fspath` function.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/configuration.py#L1997'>airflow/configuration.py:1997:39:</a> PLC2801 Unnecessary dunder call to `__fspath__`. Use `os.fspath` function.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/models/baseoperator.py#L192'>airflow/models/baseoperator.py:192:16:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/models/taskmixin.py#L109'>airflow/models/taskmixin.py:109:9:</a> PLC2801 [*] Unnecessary dunder call to `__lshift__`. Use `<<` operator.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/models/taskmixin.py#L114'>airflow/models/taskmixin.py:114:9:</a> PLC2801 [*] Unnecessary dunder call to `__rshift__`. Use `>>` operator.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/models/xcom.py#L724'>airflow/models/xcom.py:724:21:</a> PLC2801 Unnecessary dunder call to `__enter__`
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/providers/elasticsearch/log/es_response.py#L57'>airflow/providers/elasticsearch/log/es_response.py:57:20:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L167'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:167:44:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/apache/airflow/blob/2093b6f3b94be9fae5d61042a9c280d9a835687b/airflow/providers/fab/auth_manager/cli_commands/user_command.py#L60'>airflow/providers/fab/auth_manager/cli_commands/user_command.py:60:66:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
... 117 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+56 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/commands/exceptions.py#L28'>samcli/commands/exceptions.py:28:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/commands/exceptions.py#L47'>samcli/commands/exceptions.py:47:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/commands/local/lib/debug_context.py#L31'>samcli/commands/local/lib/debug_context.py:31:16:</a> PLC2801 [*] Unnecessary dunder call to `__bool__`. Use `bool()` builtin.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L19'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:19:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L34'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:34:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L392'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:392:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L51'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:51:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/hook_packages/terraform/hooks/prepare/exceptions.py#L74'>samcli/hook_packages/terraform/hooks/prepare/exceptions.py:74:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/lib/cookiecutter/exceptions.py#L9'>samcli/lib/cookiecutter/exceptions.py:9:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/aws/aws-sam-cli/blob/b5503aeb70e463a09b59c7aea9e5782ddc945877/samcli/lib/docker/log_streamer.py#L20'>samcli/lib/docker/log_streamer.py:20:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
... 46 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/descriptors.py#L394'>src/bokeh/core/property/descriptors.py:394:16:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/descriptors.py#L444'>src/bokeh/core/property/descriptors.py:444:21:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/descriptors.py#L652'>src/bokeh/core/property/descriptors.py:652:17:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/descriptors.py#L944'>src/bokeh/core/property/descriptors.py:944:17:</a> PLC2801 Unnecessary dunder call to `__set__`. Use subscript assignment.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/core/property/wrappers.py#L476'>src/bokeh/core/property/wrappers.py:476:17:</a> PLC2801 Unnecessary dunder call to `__setitem__`. Use subscript assignment.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/util.py#L276'>src/bokeh/embed/util.py:276:16:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/plots.py#L909'>src/bokeh/models/plots.py:909:20:</a> PLC2801 Unnecessary dunder call to `__getattribute__`. Access attribute directly or use getattr built-in function..
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/core/property/test_descriptors.py#L105'>tests/unit/bokeh/core/property/test_descriptors.py:105:13:</a> PLC2801 Unnecessary dunder call to `__set__`. Use subscript assignment.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/core/property/test_descriptors.py#L98'>tests/unit/bokeh/core/property/test_descriptors.py:98:13:</a> PLC2801 Unnecessary dunder call to `__get__`. Use `get` method.
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/core/property/test_wrappers__property.py#L550'>tests/unit/bokeh/core/property/test_wrappers__property.py:550:12:</a> PLC2801 Unnecessary dunder call to `__copy__`. Use `copy.copy()` function.
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/journalist_gui/journalist_gui/SecureDropUpdater.py#L125'>journalist_gui/journalist_gui/SecureDropUpdater.py:125:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/journalist_gui/journalist_gui/SecureDropUpdater.py#L49'>journalist_gui/journalist_gui/SecureDropUpdater.py:49:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/journalist_gui/journalist_gui/SecureDropUpdater.py#L89'>journalist_gui/journalist_gui/SecureDropUpdater.py:89:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/journalist_app/sessions.py#L31'>securedrop/journalist_app/sessions.py:31:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/pretty_bad_protocol/_meta.py#L677'>securedrop/pretty_bad_protocol/_meta.py:677:54:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/pretty_bad_protocol/_meta.py#L688'>securedrop/pretty_bad_protocol/_meta.py:688:59:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/pretty_bad_protocol/_parsers.py#L1774'>securedrop/pretty_bad_protocol/_parsers.py:1774:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/pretty_bad_protocol/_util.py#L364'>securedrop/pretty_bad_protocol/_util.py:364:11:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/tests/migrations/migration_a9fe328b053a.py#L50'>securedrop/tests/migrations/migration_a9fe328b053a.py:50:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
+ <a href='https://github.com/freedomofpress/securedrop/blob/ed6142d6817c020d77398ca046e3a769628ca831/securedrop/tests/migrations/migration_a9fe328b053a.py#L72'>securedrop/tests/migrations/migration_a9fe328b053a.py:72:9:</a> PLC2801 Unnecessary dunder call to `__init__`. Instantiate class directly.
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/6536b63a206b67f296250110afe00309848aa167/blinkpy/sync_module.py#L759'>blinkpy/sync_module.py:759:16:</a> PLC2801 [*] Unnecessary dunder call to `__repr__`. Use `repr()` builtin.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L198'>ibis/common/bases.py:198:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L212'>ibis/common/bases.py:212:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L239'>ibis/common/bases.py:239:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L241'>ibis/common/bases.py:241:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L245'>ibis/common/bases.py:245:13:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/bases.py#L247'>ibis/common/bases.py:247:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/caching.py#L39'>ibis/common/caching.py:39:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/collections.py#L229'>ibis/common/collections.py:229:43:</a> PLC2801 [*] Unnecessary dunder call to `__ge__`. Use `>=` operator.
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/collections.py#L240'>ibis/common/collections.py:240:43:</a> PLC2801 [*] Unnecessary dunder call to `__le__`. Use `<=` operator.
+ <a href='https://github.com/ibis-project/ibis/blob/1a5a42081c2218581e9e9a5b2510bf491812ccff/ibis/common/collections.py#L295'>ibis/common/collections.py:295:9:</a> PLC2801 Unnecessary dunder call to `__setattr__`. Mutate attribute directly or use setattr built-in function..
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L179'>pymilvus/client/abstract.py:179:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L282'>pymilvus/client/abstract.py:282:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L360'>pymilvus/client/abstract.py:360:16:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L627'>pymilvus/client/abstract.py:627:35:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L627'>pymilvus/client/abstract.py:627:69:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L632'>pymilvus/client/abstract.py:632:20:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L639'>pymilvus/client/abstract.py:639:30:</a> PLC2801 [*] Unnecessary dunder call to `__len__`. Use `len()` builtin.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L641'>pymilvus/client/abstract.py:641:20:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/abstract.py#L648'>pymilvus/client/abstract.py:648:34:</a> PLC2801 Unnecessary dunder call to `__getitem__`. Access item via subscript.
+ <a href='https://github.com/milvus-io/pymilvus/blob/1574ae5cb9d4c7a77b1543317faf0937f30a9c28/pymilvus/client/prepare.py#L1177'>pymilvus/client/prepare.py:1177:24:</a> PLC2801 [*] Unnecessary dunder call to `__str__`. Use `str()` builtin.
... 3 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC2801 | 490 | 490 | 0 | 0 | 0 |
| E711 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @diceroll123 on 2023-12-16 23:50_

---

_Marked ready for review by @diceroll123 on 2023-12-19 17:38_

---

_@charliermarsh reviewed on 2024-01-03 17:35_

Reviewing now...

---

_@charliermarsh approved on 2024-01-03 18:02_

---

_Label `rule` added by @charliermarsh on 2024-01-03 18:04_

---

_Label `preview` added by @charliermarsh on 2024-01-03 18:04_

---

_Renamed from "[pylint] - implement `C2801`/`unnecessary-dunder-calls`" to "[`pylint`] Implement `unnecessary-dunder-calls` (`C2801`)" by @charliermarsh on 2024-01-03 18:04_

---

_Renamed from "[`pylint`] Implement `unnecessary-dunder-calls` (`C2801`)" to "[`pylint`] Implement `unnecessary-dunder-call` (`C2801`)" by @charliermarsh on 2024-01-03 18:04_

---

_Merged by @charliermarsh on 2024-01-03 18:08_

---

_Closed by @charliermarsh on 2024-01-03 18:08_

---
