```yaml
number: 10667
title: "Modify diagnostic ranges for shell-related `bandit` rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: ruff-0.5
head: charlie/bandit
created_at: 2024-03-30T00:37:23Z
updated_at: 2024-06-25T12:39:33Z
url: https://github.com/astral-sh/ruff/pull/10667
synced_at: 2026-01-10T21:55:59Z
```

# Modify diagnostic ranges for shell-related `bandit` rules

---

_Pull request opened by @charliermarsh on 2024-03-30 00:37_

## Summary

The rules that flag shell calls now center on the function name, rather than the shell argument (which isn't really relevant -- especially it it's falsy). The rules that center on _specific arguments_ (e.g., a wildcard in a string) now highlight the argument.

Closes https://github.com/astral-sh/ruff/issues/9994.


---

_Label `bug` added by @charliermarsh on 2024-03-30 00:37_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-30 00:37_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-03-30 00:37_

---

_Comment by @github-actions[bot] on 2024-03-30 00:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+252 -36306 violations, +0 -0 fixes in 5 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+172 -25349 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/auth/backend/kerberos_auth.py#L69'>airflow/api/auth/backend/kerberos_auth.py:69:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/client/api_client.py#L28'>airflow/api/client/api_client.py:28:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/client/api_client.py#L34'>airflow/api/client/api_client.py:34:21:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/client/api_client.py#L46'>airflow/api/client/api_client.py:46:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/client/api_client.py#L53'>airflow/api/client/api_client.py:53:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/api/client/api_client.py#L60'>airflow/api/client/api_client.py:60:19:</a> ANN101 Missing type annotation for `self` in method
... 24628 additional changes omitted for rule ANN101
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/callbacks/callback_requests.py#L57'>airflow/callbacks/callback_requests.py:57:19:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/callbacks/callback_requests.py#L95'>airflow/callbacks/callback_requests.py:95:19:</a> ANN102 Missing type annotation for `cls` in classmethod
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:31:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:35:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:34:</a> S603 `subprocess` call: check for execution of untrusted input
... 297 additional changes omitted for rule S603
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/standalone_command.py#L51'>airflow/cli/commands/standalone_command.py:51:20:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/dag_processing/manager.py#L498'>airflow/dag_processing/manager.py:498:9:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/dag_processing/processor.py#L422'>airflow/dag_processing/processor.py:422:21:</a> ANN102 Missing type annotation for `cls` in classmethod
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/dag_processing/processor.py#L803'>airflow/dag_processing/processor.py:803:21:</a> ANN102 Missing type annotation for `cls` in classmethod
... 539 additional changes omitted for rule ANN102
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:35:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:45:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:27:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:37:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/providers/apache/beam/hooks/beam.py#L575'>airflow/providers/apache/beam/hooks/beam.py:575:25:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/providers/apache/beam/hooks/beam.py#L577'>airflow/providers/apache/beam/hooks/beam.py:577:13:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/utils/pydantic.py#L60'>airflow/utils/pydantic.py:60:13:</a> D107 Missing docstring in `__init__`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/utils/pydantic.py#L60'>airflow/utils/pydantic.py:60:13:</a> D107 Missing docstring in `__init__`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1082'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1082:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1091'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1091:17:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1094'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1094:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1103'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1103:17:</a> S604 Function call with `shell=True` parameter identified, security issue
... 11 additional changes omitted for rule S604
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L660'>hatch_build.py:660:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L660'>hatch_build.py:660:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L673'>hatch_build.py:673:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L673'>hatch_build.py:673:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
... 25487 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+38 -4126 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L32'>examples/output/apis/autoload_static.py:32:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L41'>examples/output/apis/autoload_static.py:41:20:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:13:</a> ANN101 Missing type annotation for `self` in method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L46'>examples/output/apis/server_document/flask_server.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L15'>examples/server/api/tornado_embed.py:15:13:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L27'>examples/server/app/server_auth/auth.py:27:13:</a> ANN101 Missing type annotation for `self` in method
... 4003 additional changes omitted for rule ANN101
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:34:</a> S602 `subprocess` call with `shell=True` identified, security issue
... 4154 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/7a67ed8cb0c04c97bc89010a90622d271d6d9a5b/devops/scripts/verify-mo.py#L116'>devops/scripts/verify-mo.py:116:16:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/freedomofpress/securedrop/blob/7a67ed8cb0c04c97bc89010a90622d271d6d9a5b/devops/scripts/verify-mo.py#L120'>devops/scripts/verify-mo.py:120:26:</a> RUF100 [*] Unused `noqa` directive (unused: `S602`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:11:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:28:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:26:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:9:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:52:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:9:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+37 -6828 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L104'>analytics/lib/counts.py:104:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L111'>analytics/lib/counts.py:111:26:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L48'>analytics/lib/counts.py:48:24:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L54'>analytics/lib/counts.py:54:9:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L72'>analytics/lib/counts.py:72:18:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/analytics/lib/counts.py#L75'>analytics/lib/counts.py:75:30:</a> ANN101 Missing type annotation for `self` in method
... 6736 additional changes omitted for rule ANN101
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/corporate/models.py#L197'>corporate/models.py:197:45:</a> ANN102 Missing type annotation for `cls` in classmethod
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L143'>scripts/lib/check_rabbitmq_queue.py:143:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L144'>scripts/lib/check_rabbitmq_queue.py:144:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L160'>scripts/lib/check_rabbitmq_queue.py:160:23:</a> S603 `subprocess` call: check for execution of untrusted input
... 6855 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN101 | 35382 | 0 | 35382 | 0 | 0 |
| ANN102 | 674 | 0 | 674 | 0 | 0 |
| S603 | 454 | 227 | 227 | 0 | 0 |
| S602 | 21 | 11 | 10 | 0 | 0 |
| S604 | 16 | 8 | 8 | 0 | 0 |
| S605 | 8 | 4 | 4 | 0 | 0 |
| D107 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+251 -249 violations, +0 -0 fixes in 5 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+171 -171 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/dag_command.py#L309'>airflow/cli/commands/dag_command.py:309:31:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/info_command.py#L199'>airflow/cli/commands/info_command.py:199:35:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L166'>airflow/cli/commands/internal_api_command.py:166:34:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L179'>airflow/cli/commands/internal_api_command.py:179:22:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/internal_api_command.py#L179'>airflow/cli/commands/internal_api_command.py:179:39:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/cli/commands/standalone_command.py#L267'>airflow/cli/commands/standalone_command.py:267:24:</a> S603 `subprocess` call: check for execution of untrusted input
... 294 additional changes omitted for rule S603
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:35:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L132'>airflow/example_dags/example_kubernetes_executor.py:132:45:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:27:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/example_dags/example_kubernetes_executor.py#L94'>airflow/example_dags/example_kubernetes_executor.py:94:37:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/providers/apache/beam/hooks/beam.py#L575'>airflow/providers/apache/beam/hooks/beam.py:575:25:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/airflow/providers/apache/beam/hooks/beam.py#L577'>airflow/providers/apache/beam/hooks/beam.py:577:13:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1082'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1082:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1091'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1091:17:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1094'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1094:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1103'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1103:17:</a> S604 Function call with `shell=True` parameter identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1106'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1106:13:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1115'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1115:17:</a> S604 Function call with `shell=True` parameter identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L160'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:160:87:</a> S604 Function call with `shell=True` parameter identified, security issue
... 8 additional changes omitted for rule S604
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L660'>hatch_build.py:660:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L660'>hatch_build.py:660:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L673'>hatch_build.py:673:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/hatch_build.py#L673'>hatch_build.py:673:59:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/scripts/ci/pre_commit/ruff_format.py#L26'>scripts/ci/pre_commit/ruff_format.py:26:1:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/scripts/ci/pre_commit/ruff_format.py#L26'>scripts/ci/pre_commit/ruff_format.py:26:33:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/dags/test_on_kill.py#L44'>tests/dags/test_on_kill.py:44:13:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/dags/test_on_kill.py#L44'>tests/dags/test_on_kill.py:44:23:</a> S605 Starting a process with a shell: seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/system/providers/amazon/aws/example_emr_eks.py#L102'>tests/system/providers/amazon/aws/example_emr_eks.py:102:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/system/providers/amazon/aws/example_emr_eks.py#L104'>tests/system/providers/amazon/aws/example_emr_eks.py:104:9:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/system/providers/amazon/aws/example_emr_eks.py#L120'>tests/system/providers/amazon/aws/example_emr_eks.py:120:13:</a> S602 `subprocess` call with `shell=True` identified, security issue
... 8 additional changes omitted for rule S602
- <a href='https://github.com/apache/airflow/blob/19baf9c60553ca7ae5af466f94f2bb23ee306df8/tests/task/task_runner/test_standard_task_runner.py#L340'>tests/task/task_runner/test_standard_task_runner.py:340:19:</a> S605 Starting a process with a shell, possible injection detected
... 308 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+38 -38 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L46'>examples/output/apis/server_document/flask_server.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:34:</a> S602 `subprocess` call with `shell=True` identified, security issue
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:20:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:26:</a> S603 `subprocess` call: check for execution of untrusted input
... 69 additional changes omitted for rule S603
... 68 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/7a67ed8cb0c04c97bc89010a90622d271d6d9a5b/devops/scripts/verify-mo.py#L116'>devops/scripts/verify-mo.py:116:16:</a> S602 `subprocess` call with `shell=True` identified, security issue
+ <a href='https://github.com/freedomofpress/securedrop/blob/7a67ed8cb0c04c97bc89010a90622d271d6d9a5b/devops/scripts/verify-mo.py#L120'>devops/scripts/verify-mo.py:120:26:</a> RUF100 [*] Unused `noqa` directive (unused: `S602`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:11:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L144'>packaging/docker/entrypoint.py:144:28:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:26:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L166'>packaging/docker/entrypoint.py:166:9:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:52:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
+ <a href='https://github.com/rotki/rotki/blob/60fae4fcaaa2fffd55ae24ced57b41bb8c09459b/packaging/docker/entrypoint.py#L174'>packaging/docker/entrypoint.py:174:9:</a> S602 `subprocess` call with `shell=True` seems safe, but may be changed in the future; consider rewriting without `shell`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+37 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L143'>scripts/lib/check_rabbitmq_queue.py:143:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L144'>scripts/lib/check_rabbitmq_queue.py:144:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L160'>scripts/lib/check_rabbitmq_queue.py:160:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/check_rabbitmq_queue.py#L161'>scripts/lib/check_rabbitmq_queue.py:161:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/hash_reqs.py#L38'>scripts/lib/hash_reqs.py:38:12:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/hash_reqs.py#L38'>scripts/lib/hash_reqs.py:38:36:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/puppet_cache.py#L25'>scripts/lib/puppet_cache.py:25:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/puppet_cache.py#L27'>scripts/lib/puppet_cache.py:27:9:</a> S603 `subprocess` call: check for execution of untrusted input
+ <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/setup_venv.py#L179'>scripts/lib/setup_venv.py:179:31:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/84cd835a8cd44619c0d76d0627e194370b4e80a4/scripts/lib/setup_venv.py#L179'>scripts/lib/setup_venv.py:179:55:</a> S603 `subprocess` call: check for execution of untrusted input
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S603 | 454 | 227 | 227 | 0 | 0 |
| S602 | 21 | 11 | 10 | 0 | 0 |
| S604 | 16 | 8 | 8 | 0 | 0 |
| S605 | 8 | 4 | 4 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila reviewed on 2024-04-01 08:25_

This has the side effect that some of the `noqa` comments would need to be moved like in the case highlighted by the ecosystem checks: https://github.com/freedomofpress/securedrop/blob/da8ac79a2017001e017b2a742fcb8c81b2f3e7b0/devops/scripts/verify-mo.py#L120:

```py
        return subprocess.run(  # <- The below `noqa` comment should be here now
            cmd,
            capture_output=True,
            env=os.environ,
            shell=True,  # noqa: S602  # <- This `noqa` command needs to be moved up
        )
```


This now raises `RUF100 [*] Unused noqa directive (unused: S602)`.

I think this change should go either under preview or in the next minor release.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs`:459 on 2024-04-02 08:26_

Nit: Rename to `evaluate_shell_keywrod` and return a `Option<Truthiness>` directly. The `ShellKeyword` type seems superfluous now that it only stores `Truthiness`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S605_S605.py.snap`:123 on 2024-04-02 08:28_

To me it's now unclear what `shell` is referring to. What I assumed from before is that the first argument ("true") is the `shell` argument. Now, that context is lost. I'm not saying that we shouldn't make this change but it might require fine-tuning the diagnostic message as well.

---

_@MichaReiser approved on 2024-04-02 08:29_

The code looks good to me. I'm not sure if the incompatibility with another tool warrants a breaking change

---

_Comment by @charliermarsh on 2024-04-02 12:22_

To be clear, the motivation here isn't achieving complete compatibility with another tool. Some of our ranges are _still_ different. The motivation is that the current ranges don't really make sense (why highlight `shell=False` for a diagnostic that's about `subprocess.call`?).

(We definitely can't ship these without a minor bump though.)

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-05-16 02:12_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-25 09:18_

---

_Comment by @MichaReiser on 2024-06-25 11:42_

This makes sense to me. I'll pick this for 0.5

---

_Merged by @MichaReiser on 2024-06-25 12:39_

---

_Closed by @MichaReiser on 2024-06-25 12:39_

---

_Branch deleted on 2024-06-25 12:39_

---
