```yaml
number: 9746
title: Fix bug where selection included deprecated rules during preview
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: release/0.2.0
head: zb/fix-preview-deprecated
created_at: 2024-01-31T17:22:18Z
updated_at: 2024-02-01T14:45:09Z
url: https://github.com/astral-sh/ruff/pull/9746
synced_at: 2026-01-10T22:57:09Z
```

# Fix bug where selection included deprecated rules during preview

---

_Pull request opened by @zanieb on 2024-01-31 17:22_

Cherry-picked from https://github.com/astral-sh/ruff/pull/9714 which is being abandoned for now because we need to invest more into our redirection infrastructure before it is feasible.

Fixes a bug in the implementation where we improperly included deprecated rules in `RuleSelector.rules()` when preview is on. Includes some clean-up of error messages and the implementation.

---

_Label `internal` added by @zanieb on 2024-01-31 17:26_

---

_Review requested from @charliermarsh by @zanieb on 2024-01-31 17:36_

---

_Comment by @github-actions[bot] on 2024-01-31 17:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB131
	- FURB113
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -33929 violations, +0 -0 fixes in 4 projects; 2 project errors; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -22899 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/__init__.py#L46'>airflow/api/__init__.py:46:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/auth/backend/kerberos_auth.py#L73'>airflow/api/auth/backend/kerberos_auth.py:73:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L27'>airflow/api/client/api_client.py:27:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L45'>airflow/api/client/api_client.py:45:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L52'>airflow/api/client/api_client.py:52:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L59'>airflow/api/client/api_client.py:59:19:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L63'>airflow/api/client/api_client.py:63:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L73'>airflow/api/client/api_client.py:73:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/api_client.py#L80'>airflow/api/client/api_client.py:80:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/json_client.py#L100'>airflow/api/client/json_client.py:100:19:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/json_client.py#L111'>airflow/api/client/json_client.py:111:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/json_client.py#L132'>airflow/api/client/json_client.py:132:21:</a> ANN101 Missing type annotation for `self` in method
... 22043 additional changes omitted for rule ANN101
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/client/local_client.py#L80'>airflow/api/client/local_client.py:80:13:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/common/experimental/get_code.py#L41'>airflow/api/common/experimental/get_code.py:41:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api/common/experimental/pool.py#L65'>airflow/api/common/experimental/pool.py:65:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/connection_endpoint.py#L131'>airflow/api_connexion/endpoints/connection_endpoint.py:131:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/connection_endpoint.py#L164'>airflow/api_connexion/endpoints/connection_endpoint.py:164:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/connection_endpoint.py#L169'>airflow/api_connexion/endpoints/connection_endpoint.py:169:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/connection_endpoint.py#L206'>airflow/api_connexion/endpoints/connection_endpoint.py:206:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/dag_endpoint.py#L138'>airflow/api_connexion/endpoints/dag_endpoint.py:138:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/dag_endpoint.py#L148'>airflow/api_connexion/endpoints/dag_endpoint.py:148:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/dag_endpoint.py#L171'>airflow/api_connexion/endpoints/dag_endpoint.py:171:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/api_connexion/endpoints/dag_endpoint.py#L220'>airflow/api_connexion/endpoints/dag_endpoint.py:220:9:</a> TRY200 Use `raise from` to specify exception cause
... 351 additional changes omitted for rule TRY200
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/callbacks/callback_requests.py#L57'>airflow/callbacks/callback_requests.py:57:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/callbacks/callback_requests.py#L95'>airflow/callbacks/callback_requests.py:95:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/cli/commands/standalone_command.py#L51'>airflow/cli/commands/standalone_command.py:51:20:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/dag_processing/manager.py#L496'>airflow/dag_processing/manager.py:496:9:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/dag_processing/processor.py#L420'>airflow/dag_processing/processor.py:420:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/dag_processing/processor.py#L795'>airflow/dag_processing/processor.py:795:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/exceptions.py#L223'>airflow/exceptions.py:223:15:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/executors/base_executor.py#L498'>airflow/executors/base_executor.py:498:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/12da83641ca2778ce37fe3e223a3506254c6237a/airflow/executors/executor_loader.py#L121'>airflow/executors/executor_loader.py:121:9:</a> ANN102 Missing type annotation for `cls` in classmethod
... 22866 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -4125 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L32'>examples/output/apis/autoload_static.py:32:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L41'>examples/output/apis/autoload_static.py:41:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L15'>examples/server/api/tornado_embed.py:15:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L27'>examples/server/app/server_auth/auth.py:27:13:</a> ANN101 Missing type annotation for `self` in method
... 4003 additional changes omitted for rule ANN101
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/session.py#L524'>src/bokeh/client/session.py:524:28:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L115'>src/bokeh/colors/color.py:115:18:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L132'>src/bokeh/colors/color.py:132:18:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L245'>src/bokeh/colors/color.py:245:18:</a> ANN102 Missing type annotation for `cls` in classmethod
... 4115 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/80e13b40496ca70908217383810a43b38f175d96/ibis/common/tests/test_graph.py#L25'>ibis/common/tests/test_graph.py:25:22:</a> RUF023 [*] `MyNode.__match_args__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -6905 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L105'>analytics/lib/counts.py:105:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L112'>analytics/lib/counts.py:112:26:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L49'>analytics/lib/counts.py:49:24:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L55'>analytics/lib/counts.py:55:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L73'>analytics/lib/counts.py:73:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/lib/counts.py#L76'>analytics/lib/counts.py:76:30:</a> ANN101 Missing type annotation for `self` in method
... 6614 additional changes omitted for rule ANN101
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/analytics/views/stats.py#L156'>analytics/views/stats.py:156:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/confirmation/models.py#L102'>confirmation/models.py:102:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/corporate/lib/registration.py#L115'>corporate/lib/registration.py:115:9:</a> TRY200 Use `raise from` to specify exception cause
- <a href='https://github.com/zulip/zulip/blob/b522ffa3170e7acecd2029a68d9cd331b336f883/corporate/lib/remote_billing_util.py#L124'>corporate/lib/remote_billing_util.py:124:9:</a> TRY200 Use `raise from` to specify exception cause
... 6895 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in your configuration:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'mccabe' -> 'lint.mccabe'
  - 'per-file-ignores' -> 'lint.per-file-ignores'


ruff failed
  Cause: Selection of deprecated rule `TRY200` is not allowed when preview mode is enabled.
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview mode is enabled.
```

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN101 | 32681 | 0 | 32681 | 0 | 0 |
| TRY200 | 636 | 0 | 636 | 0 | 0 |
| ANN102 | 612 | 0 | 612 | 0 | 0 |
| RUF023 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Merged by @zanieb on 2024-02-01 14:45_

---

_Closed by @zanieb on 2024-02-01 14:45_

---

_Branch deleted on 2024-02-01 14:45_

---
