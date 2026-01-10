```yaml
number: 9308
title: Change PLR0917 error message to match other PLR09XX messages
type: pull_request
state: merged
author: mikaelarguedas
labels:
  - bug
assignees: []
merged: true
base: main
head: homogenize_PLR_error_message
created_at: 2023-12-29T11:32:49Z
updated_at: 2023-12-29T12:45:35Z
url: https://github.com/astral-sh/ruff/pull/9308
synced_at: 2026-01-10T23:07:18Z
```

# Change PLR0917 error message to match other PLR09XX messages

---

_Pull request opened by @mikaelarguedas on 2023-12-29 11:32_

## Summary

Remove `:` for PLR0917 to make all PLR09XX message look the same

```
PLR0904	Too many public methods (21 > 20)
PLR0911	Too many return statements (16 > 6)
PLR0912	Too many branches (13 > 12)
PLR0913	Too many arguments in function definition (10 > 5)
PLR0915	Too many statements (118 > 50)
PLR0917	Too many positional arguments: (15/5)
```
## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-12-29 11:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2672 -2672 violations, +0 -0 fixes in 10 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+32 -32 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/argx.py#L311'>aiven/client/argx.py:311:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/argx.py#L311'>aiven/client/argx.py:311:9:</a> PLR0917 Too many positional arguments: (10/5)
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L3633'>aiven/client/cli.py:3633:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L3633'>aiven/client/cli.py:3633:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L5697'>aiven/client/cli.py:5697:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/cli.py#L5697'>aiven/client/cli.py:5697:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L1080'>aiven/client/client.py:1080:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L1080'>aiven/client/client.py:1080:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L1090'>aiven/client/client.py:1090:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aiven/aiven-client/blob/a87f8c83151788c097ae0a2e7d14abe0572f2f60/aiven/client/client.py#L1090'>aiven/client/client.py:1090:9:</a> PLR0917 Too many positional arguments: (6/5)
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1181 -1181 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/api_client.py#L33'>airflow/api/client/api_client.py:33:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/json_client.py#L57'>airflow/api/client/json_client.py:57:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/json_client.py#L57'>airflow/api/client/json_client.py:57:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/local_client.py#L31'>airflow/api/client/local_client.py:31:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/client/local_client.py#L31'>airflow/api/client/local_client.py:31:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/common/mark_tasks.py#L209'>airflow/api/common/mark_tasks.py:209:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/common/mark_tasks.py#L209'>airflow/api/common/mark_tasks.py:209:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/common/trigger_dag.py#L34'>airflow/api/common/trigger_dag.py:34:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api/common/trigger_dag.py#L34'>airflow/api/common/trigger_dag.py:34:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api_connexion/endpoints/dag_endpoint.py#L144'>airflow/api_connexion/endpoints/dag_endpoint.py:144:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/api_connexion/endpoints/dag_endpoint.py#L144'>airflow/api_connexion/endpoints/dag_endpoint.py:144:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/callbacks/callback_requests.py#L114'>airflow/callbacks/callback_requests.py:114:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/callbacks/callback_requests.py#L76'>airflow/callbacks/callback_requests.py:76:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0917 Too many positional arguments (11/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/cli/cli_config.py#L79'>airflow/cli/cli_config.py:79:9:</a> PLR0917 Too many positional arguments: (11/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/cli/commands/webserver_command.py#L84'>airflow/cli/commands/webserver_command.py:84:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/configuration.py#L1050'>airflow/configuration.py:1050:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/apache/airflow/blob/7cd326ccbd5da03900b01695c74a805ced980855/airflow/configuration.py#L1050'>airflow/configuration.py:1050:9:</a> PLR0917 Too many positional arguments: (7/5)
... 2340 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+399 -399 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/cli_config_file.py#L176'>samcli/cli/cli_config_file.py:176:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/cli_config_file.py#L176'>samcli/cli/cli_config_file.py:176:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/global_config.py#L184'>samcli/cli/global_config.py:184:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/global_config.py#L184'>samcli/cli/global_config.py:184:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/global_config.py#L223'>samcli/cli/global_config.py:223:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/cli/global_config.py#L223'>samcli/cli/global_config.py:223:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/commands/_utils/options.py#L195'>samcli/commands/_utils/options.py:195:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/commands/_utils/options.py#L195'>samcli/commands/_utils/options.py:195:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/commands/_utils/table_print.py#L103'>samcli/commands/_utils/table_print.py:103:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/aws/aws-sam-cli/blob/034e027b4010a709d101a035fa41813c63bc790e/samcli/commands/_utils/table_print.py#L103'>samcli/commands/_utils/table_print.py:103:5:</a> PLR0917 Too many positional arguments: (7/5)
... 788 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+54 -54 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/colorbar_log.py#L15'>examples/basic/annotations/colorbar_log.py:15:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/colorbar_log.py#L15'>examples/basic/annotations/colorbar_log.py:15:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/gauges.py#L48'>examples/models/gauges.py:48:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/gauges.py#L48'>examples/models/gauges.py:48:5:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/histogram.py#L17'>examples/plotting/histogram.py:17:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/plotting/histogram.py#L17'>examples/plotting/histogram.py:17:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/topics/hierarchical/treemap.py#L26'>examples/topics/hierarchical/treemap.py:26:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/topics/hierarchical/treemap.py#L26'>examples/topics/hierarchical/treemap.py:26:5:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/client/session.py#L194'>src/bokeh/client/session.py:194:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/client/session.py#L194'>src/bokeh/client/session.py:194:5:</a> PLR0917 Too many positional arguments: (6/5)
... 98 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/journalist_app/sessions.py#L100'>securedrop/journalist_app/sessions.py:100:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L425'>securedrop/models.py:425:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L425'>securedrop/models.py:425:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L78'>securedrop/models.py:78:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/models.py#L78'>securedrop/models.py:78:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_meta.py#L135'>securedrop/pretty_bad_protocol/_meta.py:135:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_meta.py#L135'>securedrop/pretty_bad_protocol/_meta.py:135:9:</a> PLR0917 Too many positional arguments: (10/5)
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_meta.py#L785'>securedrop/pretty_bad_protocol/_meta.py:785:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/securedrop/pretty_bad_protocol/_meta.py#L785'>securedrop/pretty_bad_protocol/_meta.py:785:9:</a> PLR0917 Too many positional arguments: (8/5)
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/api.py#L461'>blinkpy/api.py:461:11:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/api.py#L461'>blinkpy/api.py:461:11:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/api.py#L483'>blinkpy/api.py:483:11:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/api.py#L483'>blinkpy/api.py:483:11:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/auth.py#L144'>blinkpy/auth.py:144:15:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/auth.py#L144'>blinkpy/auth.py:144:15:</a> PLR0917 Too many positional arguments: (9/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/blinkpy.py#L322'>blinkpy/blinkpy.py:322:15:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/blinkpy.py#L322'>blinkpy/blinkpy.py:322:15:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/blinkpy.py#L392'>blinkpy/blinkpy.py:392:15:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/blinkpy/blinkpy.py#L392'>blinkpy/blinkpy.py:392:15:</a> PLR0917 Too many positional arguments: (6/5)
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+110 -110 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/alchemy/__init__.py#L138'>ibis/backends/base/sql/alchemy/__init__.py:138:9:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/alchemy/__init__.py#L138'>ibis/backends/base/sql/alchemy/__init__.py:138:9:</a> PLR0917 Too many positional arguments: (9/5)
+ <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/alchemy/__init__.py#L72'>ibis/backends/base/sql/alchemy/__init__.py:72:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/alchemy/__init__.py#L72'>ibis/backends/base/sql/alchemy/__init__.py:72:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/query_builder.py#L192'>ibis/backends/base/sql/compiler/query_builder.py:192:9:</a> PLR0917 Too many positional arguments (15/5)
- <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/query_builder.py#L192'>ibis/backends/base/sql/compiler/query_builder.py:192:9:</a> PLR0917 Too many positional arguments: (15/5)
+ <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/select_builder.py#L26'>ibis/backends/base/sql/compiler/select_builder.py:26:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/select_builder.py#L26'>ibis/backends/base/sql/compiler/select_builder.py:26:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/translator.py#L192'>ibis/backends/base/sql/compiler/translator.py:192:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/ibis-project/ibis/blob/83fe32658918bc06f626a1aa8281c61c18290918/ibis/backends/base/sql/compiler/translator.py#L192'>ibis/backends/base/sql/compiler/translator.py:192:9:</a> PLR0917 Too many positional arguments: (6/5)
... 210 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+662 -662 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L490'>asv_bench/benchmarks/groupby.py:490:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L490'>asv_bench/benchmarks/groupby.py:490:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L573'>asv_bench/benchmarks/groupby.py:573:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L573'>asv_bench/benchmarks/groupby.py:573:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L576'>asv_bench/benchmarks/groupby.py:576:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/groupby.py#L576'>asv_bench/benchmarks/groupby.py:576:9:</a> PLR0917 Too many positional arguments: (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L108'>asv_bench/benchmarks/rolling.py:108:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L108'>asv_bench/benchmarks/rolling.py:108:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L123'>asv_bench/benchmarks/rolling.py:123:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L123'>asv_bench/benchmarks/rolling.py:123:9:</a> PLR0917 Too many positional arguments: (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L214'>asv_bench/benchmarks/rolling.py:214:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/37975ebafae0ded16951a23d027ba63d23dca349/asv_bench/benchmarks/rolling.py#L214'>asv_bench/benchmarks/rolling.py:214:9:</a> PLR0917 Too many positional arguments: (6/5)
... 1312 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 5344 | 2672 | 2672 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2023-12-29 12:40_

Thank you!

---

_Label `bug` added by @charliermarsh on 2023-12-29 12:40_

---

_Merged by @charliermarsh on 2023-12-29 12:40_

---

_Closed by @charliermarsh on 2023-12-29 12:40_

---

_Branch deleted on 2023-12-29 12:45_

---
