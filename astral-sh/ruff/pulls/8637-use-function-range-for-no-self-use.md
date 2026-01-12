```yaml
number: 8637
title: "Use function range for `no-self-use`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/self
created_at: 2023-11-12T21:07:13Z
updated_at: 2023-11-12T21:37:53Z
url: https://github.com/astral-sh/ruff/pull/8637
synced_at: 2026-01-12T15:55:26Z
```

# Use function range for `no-self-use`

---

_@charliermarsh_

Previously, this rule used the range of the `self` annotation, but it's a lot more natural to use the range of the function name (since it also means the `# noqa` is associated with the method rather than its first argument).

Closes https://github.com/astral-sh/ruff/issues/8635.

---

_Label `bug` added by @charliermarsh on 2023-11-12 21:07_

---

_Comment by @github-actions[bot] on 2023-11-12 21:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+20763 -20763 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+17 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L103'>aiven/client/argx.py:103:26:</a> PLR6301 Method `_get_help_string` could be a function, class method, or static method
+ <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L103'>aiven/client/argx.py:103:9:</a> PLR6301 Method `_get_help_string` could be a function, class method, or static method
- <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L298'>aiven/client/argx.py:298:25:</a> PLR6301 Method `expected_errors` could be a function, class method, or static method
+ <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L298'>aiven/client/argx.py:298:9:</a> PLR6301 Method `expected_errors` could be a function, class method, or static method
+ <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L301'>aiven/client/argx.py:301:9:</a> PLR6301 Method `_to_mapping_collection` could be a function, class method, or static method
- <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/argx.py#L302'>aiven/client/argx.py:302:9:</a> PLR6301 Method `_to_mapping_collection` could be a function, class method, or static method
- <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/cli.py#L1012'>aiven/client/cli.py:1012:39:</a> PLR6301 Method `_format_service_notifications` could be a function, class method, or static method
+ <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/cli.py#L1012'>aiven/client/cli.py:1012:9:</a> PLR6301 Method `_format_service_notifications` could be a function, class method, or static method
- <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/cli.py#L1028'>aiven/client/cli.py:1028:37:</a> PLR6301 Method `print_service_notifications` could be a function, class method, or static method
+ <a href='https://github.com/aiven/aiven-client/blob/4c3ddfffbce42ba0fe5f749deb84d72b419399ff/aiven/client/cli.py#L1028'>aiven/client/cli.py:1028:9:</a> PLR6301 Method `print_service_notifications` could be a function, class method, or static method
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6415 -6415 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L31'>airflow/api/client/local_client.py:31:9:</a> PLR6301 Method `trigger_dag` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L32'>airflow/api/client/local_client.py:32:9:</a> PLR6301 Method `trigger_dag` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L58'>airflow/api/client/local_client.py:58:20:</a> PLR6301 Method `delete_dag` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L58'>airflow/api/client/local_client.py:58:9:</a> PLR6301 Method `delete_dag` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L62'>airflow/api/client/local_client.py:62:18:</a> PLR6301 Method `get_pool` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L62'>airflow/api/client/local_client.py:62:9:</a> PLR6301 Method `get_pool` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L68'>airflow/api/client/local_client.py:68:19:</a> PLR6301 Method `get_pools` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L68'>airflow/api/client/local_client.py:68:9:</a> PLR6301 Method `get_pools` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L71'>airflow/api/client/local_client.py:71:21:</a> PLR6301 Method `create_pool` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L71'>airflow/api/client/local_client.py:71:9:</a> PLR6301 Method `create_pool` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L86'>airflow/api/client/local_client.py:86:21:</a> PLR6301 Method `delete_pool` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L86'>airflow/api/client/local_client.py:86:9:</a> PLR6301 Method `delete_pool` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L90'>airflow/api/client/local_client.py:90:21:</a> PLR6301 Method `get_lineage` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api/client/local_client.py#L90'>airflow/api/client/local_client.py:90:9:</a> PLR6301 Method `get_lineage` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4f5e482c1f9eebc3824d29e446828f4a3066a184/airflow/api_connexion/schemas/common_schema.py#L118'>airflow/api_connexion/schemas/common_schema.py:118:22:</a> PLR6301 Method `get_obj_type` could be a function, class method, or static method
... 12815 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+342 -342 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/cli/command.py#L133'>samcli/cli/command.py:133:25:</a> PLR6301 Method `format_commands` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/cli/command.py#L133'>samcli/cli/command.py:133:9:</a> PLR6301 Method `format_commands` could be a function, class method, or static method
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/list/json_consumer.py#L14'>samcli/commands/list/json_consumer.py:14:17:</a> PLR6301 Method `consume` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/list/json_consumer.py#L14'>samcli/commands/list/json_consumer.py:14:9:</a> PLR6301 Method `consume` could be a function, class method, or static method
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/list/table_consumer.py#L15'>samcli/commands/list/table_consumer.py:15:17:</a> PLR6301 Method `consume` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/list/table_consumer.py#L15'>samcli/commands/list/table_consumer.py:15:9:</a> PLR6301 Method `consume` could be a function, class method, or static method
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/traces/trace_console_consumers.py#L17'>samcli/commands/traces/trace_console_consumers.py:17:17:</a> PLR6301 Method `consume` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/commands/traces/trace_console_consumers.py#L17'>samcli/commands/traces/trace_console_consumers.py:17:9:</a> PLR6301 Method `consume` could be a function, class method, or static method
- <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/lib/cookiecutter/question.py#L133'>samcli/lib/cookiecutter/question.py:133:16:</a> PLR6301 Method `prompt` could be a function, class method, or static method
+ <a href='https://github.com/aws/aws-sam-cli/blob/d63fa5d112f16f495c713ddbabcc2efeb69f1fa2/samcli/lib/cookiecutter/question.py#L133'>samcli/lib/cookiecutter/question.py:133:9:</a> PLR6301 Method `prompt` could be a function, class method, or static method
... 674 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1872 -1872 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/server/app/server_auth/auth.py#L34'>examples/server/app/server_auth/auth.py:34:26:</a> PLR6301 Method `check_permission` could be a function, class method, or static method
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/server/app/server_auth/auth.py#L34'>examples/server/app/server_auth/auth.py:34:9:</a> PLR6301 Method `check_permission` could be a function, class method, or static method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/system.py#L50'>release/system.py:50:15:</a> PLR6301 Method `abort` could be a function, class method, or static method
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/system.py#L50'>release/system.py:50:9:</a> PLR6301 Method `abort` could be a function, class method, or static method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/system.py#L54'>release/system.py:54:12:</a> PLR6301 Method `cd` could be a function, class method, or static method
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/release/system.py#L54'>release/system.py:54:9:</a> PLR6301 Method `cd` could be a function, class method, or static method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/handler.py#L198'>src/bokeh/application/handlers/handler.py:198:25:</a> PLR6301 Method `process_request` could be a function, class method, or static method
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/handler.py#L198'>src/bokeh/application/handlers/handler.py:198:9:</a> PLR6301 Method `process_request` could be a function, class method, or static method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/handler.py#L220'>src/bokeh/application/handlers/handler.py:220:18:</a> PLR6301 Method `url_path` could be a function, class method, or static method
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/application/handlers/handler.py#L220'>src/bokeh/application/handlers/handler.py:220:9:</a> PLR6301 Method `url_path` could be a function, class method, or static method
... 3734 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+182 -182 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L103'>admin/securedrop_admin/__init__.py:103:13:</a> PLR6301 Method `validate` could be a function, class method, or static method
- <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L103'>admin/securedrop_admin/__init__.py:103:22:</a> PLR6301 Method `validate` could be a function, class method, or static method
+ <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L109'>admin/securedrop_admin/__init__.py:109:13:</a> PLR6301 Method `validate` could be a function, class method, or static method
- <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L109'>admin/securedrop_admin/__init__.py:109:22:</a> PLR6301 Method `validate` could be a function, class method, or static method
+ <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L115'>admin/securedrop_admin/__init__.py:115:13:</a> PLR6301 Method `validate` could be a function, class method, or static method
- <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L115'>admin/securedrop_admin/__init__.py:115:22:</a> PLR6301 Method `validate` could be a function, class method, or static method
+ <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L122'>admin/securedrop_admin/__init__.py:122:13:</a> PLR6301 Method `validate` could be a function, class method, or static method
- <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L122'>admin/securedrop_admin/__init__.py:122:22:</a> PLR6301 Method `validate` could be a function, class method, or static method
+ <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L130'>admin/securedrop_admin/__init__.py:130:13:</a> PLR6301 Method `validate` could be a function, class method, or static method
- <a href='https://github.com/freedomofpress/securedrop/blob/acf1a8291b566c965aae3ba1471b0893345aeb87/admin/securedrop_admin/__init__.py#L130'>admin/securedrop_admin/__init__.py:130:22:</a> PLR6301 Method `validate` could be a function, class method, or static method
... 354 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/auth.py#L92'>blinkpy/auth.py:92:16:</a> PLR6301 Method `logout` could be a function, class method, or static method
+ <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/auth.py#L92'>blinkpy/auth.py:92:9:</a> PLR6301 Method `logout` could be a function, class method, or static method
+ <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/sync_module.py#L537'>blinkpy/sync_module.py:537:15:</a> PLR6301 Method `get_network_info` could be a function, class method, or static method
- <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/sync_module.py#L537'>blinkpy/sync_module.py:537:32:</a> PLR6301 Method `get_network_info` could be a function, class method, or static method
+ <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/sync_module.py#L600'>blinkpy/sync_module.py:600:15:</a> PLR6301 Method `get_network_info` could be a function, class method, or static method
- <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/blinkpy/sync_module.py#L600'>blinkpy/sync_module.py:600:32:</a> PLR6301 Method `get_network_info` could be a function, class method, or static method
+ <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/tests/test_auth.py#L277'>tests/test_auth.py:277:15:</a> PLR6301 Method `get` could be a function, class method, or static method
- <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/tests/test_auth.py#L277'>tests/test_auth.py:277:19:</a> PLR6301 Method `get` could be a function, class method, or static method
+ <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/tests/test_auth.py#L281'>tests/test_auth.py:281:15:</a> PLR6301 Method `post` could be a function, class method, or static method
- <a href='https://github.com/fronzbot/blinkpy/blob/daea3cf6dcee9ef6f1b1812301762a55bf82162b/tests/test_auth.py#L281'>tests/test_auth.py:281:20:</a> PLR6301 Method `post` could be a function, class method, or static method
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+109 -109 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/docs/_renderer.py#L12'>docs/_renderer.py:12:16:</a> PLR6301 Method `render` could be a function, class method, or static method
+ <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/docs/_renderer.py#L12'>docs/_renderer.py:12:9:</a> PLR6301 Method `render` could be a function, class method, or static method
- <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L137'>ibis/backends/base/__init__.py:137:18:</a> PLR6301 Method `_qualify` could be a function, class method, or static method
+ <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L137'>ibis/backends/base/__init__.py:137:9:</a> PLR6301 Method `_qualify` could be a function, class method, or static method
- <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L140'>ibis/backends/base/__init__.py:140:20:</a> PLR6301 Method `_unqualify` could be a function, class method, or static method
+ <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L140'>ibis/backends/base/__init__.py:140:9:</a> PLR6301 Method `_unqualify` could be a function, class method, or static method
+ <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L590'>ibis/backends/base/__init__.py:590:9:</a> PLR6301 Method `to_delta` could be a function, class method, or static method
- <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/__init__.py#L591'>ibis/backends/base/__init__.py:591:9:</a> PLR6301 Method `to_delta` could be a function, class method, or static method
- <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/sql/__init__.py#L353'>ibis/backends/base/sql/__init__.py:353:14:</a> PLR6301 Method `_log` could be a function, class method, or static method
+ <a href='https://github.com/ibis-project/ibis/blob/c72882ea204d6ab5c1fc1d6246b4e3a525b7d253/ibis/backends/base/sql/__init__.py#L353'>ibis/backends/base/sql/__init__.py:353:9:</a> PLR6301 Method `_log` could be a function, class method, or static method
... 208 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+21 -21 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/examples/multithreading_hello_milvus.py#L49'>examples/multithreading_hello_milvus.py:49:21:</a> PLR6301 Method `insert_data` could be a function, class method, or static method
+ <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/examples/multithreading_hello_milvus.py#L49'>examples/multithreading_hello_milvus.py:49:9:</a> PLR6301 Method `insert_data` could be a function, class method, or static method
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/buffer.py#L72'>pymilvus/bulk_writer/buffer.py:72:16:</a> PLR6301 Method `_throw` could be a function, class method, or static method
+ <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/buffer.py#L72'>pymilvus/bulk_writer/buffer.py:72:9:</a> PLR6301 Method `_throw` could be a function, class method, or static method
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/bulk_writer.py#L101'>pymilvus/bulk_writer/bulk_writer.py:101:16:</a> PLR6301 Method `_throw` could be a function, class method, or static method
+ <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/bulk_writer.py#L101'>pymilvus/bulk_writer/bulk_writer.py:101:9:</a> PLR6301 Method `_throw` could be a function, class method, or static method
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/remote_bulk_writer.py#L126'>pymilvus/bulk_writer/remote_bulk_writer.py:126:19:</a> PLR6301 Method `_local_rm` could be a function, class method, or static method
+ <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/bulk_writer/remote_bulk_writer.py#L126'>pymilvus/bulk_writer/remote_bulk_writer.py:126:9:</a> PLR6301 Method `_local_rm` could be a function, class method, or static method
+ <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/client/abstract.py#L309'>pymilvus/client/abstract.py:309:9:</a> PLR6301 Method `get_fields_by_range` could be a function, class method, or static method
- <a href='https://github.com/milvus-io/pymilvus/blob/142820167b1edc4948f76049659377140de60ae9/pymilvus/client/abstract.py#L310'>pymilvus/client/abstract.py:310:9:</a> PLR6301 Method `get_fields_by_range` could be a function, class method, or static method
... 32 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 41526 | 20763 | 20763 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-12 21:37_

---

_Closed by @charliermarsh on 2023-11-12 21:37_

---

_Branch deleted on 2023-11-12 21:37_

---
