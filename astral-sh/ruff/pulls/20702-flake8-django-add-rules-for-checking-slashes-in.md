```yaml
number: 20702
title: "[`flake8-django`] Add rules for checking slashes in `path()`"
type: pull_request
state: closed
author: jvacek
labels:
  - rule
  - preview
assignees: []
base: main
head: dj014-dj015-slashes-in-path
created_at: 2025-10-05T16:03:20Z
updated_at: 2026-01-21T10:31:21Z
url: https://github.com/astral-sh/ruff/pull/20702
synced_at: 2026-01-21T10:59:59Z
```

# [`flake8-django`] Add rules for checking slashes in `path()`

---

_@jvacek_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
The MR adds two new rules: DJ014 and DJ015. These two rules enforce the slash consistency when making django paths.

In essence, you should define paths as `foo/` and not `foo` (DJ014) or `/foo/` (DJ015).
 

<!-- What's the purpose of the change? What does it do, and why? -->

There was a discussion in Discord about whether the upstream flake8-django package was abandoned yet or not. See here: https://discord.com/channels/1039017663004942429/1039017663512449056/1422941869091979284

I am still wondering if it makes sense to add these under `DJ` directly, and not `XDJ` or something like that. Happy to make a backport package for flake8 for this as well.
## Test Plan
- Added fixtures and snapshots

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-05 16:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+14 -14 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select A,E703,F704,B015,B018,D100</pre>
</p>
<p>

<pre>
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:11: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:11: invalid-syntax: Expected `,`, found name
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:16: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:16: invalid-syntax: Expected `,`, found `=`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:22: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:22: invalid-syntax: Expected `,`, found name
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:24: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:10:24: invalid-syntax: Expected `,`, found `;`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:27: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:27: invalid-syntax: Expected `,`, found `=`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:34: invalid-syntax: Expected ',', found name
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:34: invalid-syntax: Expected `,`, found name
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:48: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:48: invalid-syntax: Expected `,`, found `;`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected '}', found NonLogicalNewline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:11:49: invalid-syntax: Expected `}`, found NonLogicalNewline
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:5:14: invalid-syntax: Expected ':', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:5:14: invalid-syntax: Expected `:`, found `{`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:25: invalid-syntax: Expected ',', found '='
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:25: invalid-syntax: Expected `,`, found `=`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:46: invalid-syntax: Expected ',', found ';'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:46: invalid-syntax: Expected `,`, found `;`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:47: invalid-syntax: Expected '}', found newline
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:47: invalid-syntax: Expected `}`, found newline
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:9: invalid-syntax: Expected ',', found '{'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:6:9: invalid-syntax: Expected `,`, found `{`
- examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:13: invalid-syntax: Expected ':', found 'break'
+ examples/mcp/databricks_mcp_cookbook.ipynb:cell 30:7:13: invalid-syntax: Expected `:`, found `break`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| invalid-syntax: | 28 | 14 | 14 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1922 -1843 violations, +0 -0 fixes in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L30'>airflow-core/src/airflow/operators/__init__.py:30:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L30'>airflow-core/src/airflow/operators/__init__.py:30:13:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L40'>airflow-core/src/airflow/operators/__init__.py:40:11:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L40'>airflow-core/src/airflow/operators/__init__.py:40:11:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L43'>airflow-core/src/airflow/operators/__init__.py:43:15:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L43'>airflow-core/src/airflow/operators/__init__.py:43:15:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L67'>airflow-core/src/airflow/operators/__init__.py:67:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/operators/__init__.py#L67'>airflow-core/src/airflow/operators/__init__.py:67:13:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/sensors/__init__.py#L35'>airflow-core/src/airflow/sensors/__init__.py:35:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/47a1eb85a26aef8eccfebacc6b8874dec7e612aa/airflow-core/src/airflow/sensors/__init__.py#L35'>airflow-core/src/airflow/sensors/__init__.py:35:13:</a> E231 [*] Missing whitespace after `:`
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+8 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/buildcmd/test_build_cmd.py#L1030'>tests/integration/buildcmd/test_build_cmd.py:1030:39:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/buildcmd/test_build_cmd.py#L1030'>tests/integration/buildcmd/test_build_cmd.py:1030:39:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/buildcmd/test_build_cmd.py#L1755'>tests/integration/buildcmd/test_build_cmd.py:1755:39:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/buildcmd/test_build_cmd.py#L1755'>tests/integration/buildcmd/test_build_cmd.py:1755:39:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2386'>tests/integration/local/start_api/test_start_api.py:2386:40:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2386'>tests/integration/local/start_api/test_start_api.py:2386:40:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2405'>tests/integration/local/start_api/test_start_api.py:2405:40:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2405'>tests/integration/local/start_api/test_start_api.py:2405:40:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2424'>tests/integration/local/start_api/test_start_api.py:2424:40:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/aws/aws-sam-cli/blob/d6ec326ff2602cc3e69645dd6606e55814a88c3f/tests/integration/local/start_api/test_start_api.py#L2424'>tests/integration/local/start_api/test_start_api.py:2424:40:</a> E231 [*] Missing whitespace after `,`
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+508 -508 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L307'>config.py:307:39:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L307'>config.py:307:39:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L307'>config.py:307:75:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L307'>config.py:307:75:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L308'>config.py:308:40:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L308'>config.py:308:40:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L314'>config.py:314:80:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L314'>config.py:314:80:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L351'>config.py:351:85:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/config.py#L351'>config.py:351:85:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L227'>crazy_functions/Conversation_To_File.py:227:36:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Conversation_To_File.py#L227'>crazy_functions/Conversation_To_File.py:227:36:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/crazy_functions/Document_Conversation_Wrap.py#L31'>crazy_functions/Document_Conversation_Wrap.py:31:47:</a> E231 [*] Missing whitespace after ':'
... 1003 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1213 -1213 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L80'>examples/advanced/extensions/tool.py:80:25:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L80'>examples/advanced/extensions/tool.py:80:25:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L80'>examples/advanced/extensions/tool.py:80:41:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L80'>examples/advanced/extensions/tool.py:80:41:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L17'>examples/basic/annotations/arrowheads.py:17:27:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L17'>examples/basic/annotations/arrowheads.py:17:27:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L25'>examples/basic/annotations/colorbar_log.py:25:25:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L25'>examples/basic/annotations/colorbar_log.py:25:25:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L25'>examples/basic/annotations/colorbar_log.py:25:40:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L25'>examples/basic/annotations/colorbar_log.py:25:40:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:21:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:21:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:23:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:23:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:30:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:30:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:32:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:32:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:41:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:41:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:43:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:43:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:50:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:50:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:52:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_multi_index.py#L6'>examples/basic/annotations/legends_multi_index.py:6:52:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_additional.py#L7'>examples/basic/annotations/title_additional.py:7:12:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_additional.py#L7'>examples/basic/annotations/title_additional.py:7:12:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_additional.py#L7'>examples/basic/annotations/title_additional.py:7:19:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_additional.py#L7'>examples/basic/annotations/title_additional.py:7:19:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_basic.py#L5'>examples/basic/annotations/title_basic.py:5:12:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/title_basic.py#L5'>examples/basic/annotations/title_basic.py:5:12:</a> E231 [*] Missing whitespace after `,`
... 2394 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+22 -22 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/blinksync.py#L94'>blinksync/blinksync.py:94:103:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/blinksync.py#L94'>blinksync/blinksync.py:94:103:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:35:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:35:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:51:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:51:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:62:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L117'>blinksync/forms.py:117:62:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L11'>blinksync/forms.py:11:22:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/fronzbot/blinkpy/blob/f90792008059ee4e47a4b566000fcf5a714a1ce9/blinksync/forms.py#L11'>blinksync/forms.py:11:22:</a> E231 [*] Missing whitespace after `,`
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+68 -68 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L34'>examples/alter.py:34:105:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L34'>examples/alter.py:34:105:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L34'>examples/alter.py:34:53:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L34'>examples/alter.py:34:53:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L35'>examples/alter.py:35:52:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L35'>examples/alter.py:35:52:</a> E231 [*] Missing whitespace after `,`
- <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L36'>examples/alter.py:36:101:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L36'>examples/alter.py:36:101:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L36'>examples/alter.py:36:106:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/milvus-io/pymilvus/blob/87a0ae9ec042fd7c8ec8754790f0569b8f236b19/examples/alter.py#L36'>examples/alter.py:36:106:</a> E231 [*] Missing whitespace after `,`
... 126 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/579b88aab716cef055da38bf171b99b9217229d8/mlflow/types/llm.py#L829'>mlflow/types/llm.py:829:42:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/mlflow/mlflow/blob/579b88aab716cef055da38bf171b99b9217229d8/mlflow/types/llm.py#L829'>mlflow/types/llm.py:829:42:</a> E231 [*] Missing whitespace after `,`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+79 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/analytics/urls.py#L20'>analytics/urls.py:20:10:</a> DJ100 [*] URL route `stats/installation` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/analytics/urls.py#L22'>analytics/urls.py:22:10:</a> DJ100 [*] URL route `stats` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/analytics/urls.py#L29'>analytics/urls.py:29:14:</a> DJ100 [*] URL route `stats/remote/<int:remote_server_id>/installation` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L107'>corporate/urls.py:107:10:</a> DJ100 [*] URL route `activity` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L108'>corporate/urls.py:108:10:</a> DJ100 [*] URL route `activity/integrations` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L109'>corporate/urls.py:109:10:</a> DJ100 [*] URL route `activity/support` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L112'>corporate/urls.py:112:10:</a> DJ100 [*] URL route `activity/remote` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L113'>corporate/urls.py:113:10:</a> DJ100 [*] URL route `activity/remote/support` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L136'>corporate/urls.py:136:10:</a> DJ100 [*] URL route `apps/download/<platform>` is missing a trailing slash
+ <a href='https://github.com/zulip/zulip/blob/1e78447c50d9334ae73bf1c67166ad02a349ff7f/corporate/urls.py#L137'>corporate/urls.py:137:10:</a> DJ100 [*] URL route `apps/<platform>` is missing a trailing slash
... 69 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E231 | 3658 | 1829 | 1829 | 0 | 0 |
| DJ100 | 79 | 79 | 0 | 0 | 0 |
| invalid-syntax: | 28 | 14 | 14 | 0 | 0 |

</p>
</details>




---

_Comment by @jvacek on 2025-10-05 17:08_

Can anyone help me figure out what's up with the output in the comment above? I think it might be incorrectly removing parts of the ouptut as all of the `<foo:bar>` bits in the message are being removed, [but I do have them in the snapshots](https://github.com/astral-sh/ruff/pull/20702/files#diff-aa1602dea69d60eff8f64b144ae25343690f74b4d98de60449468e173aebdd98R142)

e.g., the third line has this
```
+ analytics/urls.py:29:14: DJ014 [*] URL route `stats/remote//installation` is missing a trailing slash
```
but should be showing this instead
```
+ analytics/urls.py:29:14: DJ014 [*] URL route `stats/remote/<int:remote_server_id>/installation` is missing a trailing slash
```


---

_Marked ready for review by @jvacek on 2025-10-05 17:08_

---

_Review requested from @ntBre by @ntBre on 2025-10-05 21:54_

---

_Comment by @ntBre on 2025-10-05 21:57_

> Can anyone help me figure out what's up with the output in the comment above? I think it might be incorrectly removing parts of the ouptut as all of the `<foo:bar>` bits in the message are being removed, [but I do have them in the snapshots](https://github.com/astral-sh/ruff/pull/20702/files#diff-aa1602dea69d60eff8f64b144ae25343690f74b4d98de60449468e173aebdd98R142)

I haven't looked super closely yet, but the `<foo:bar>` bits look kind of like markdown links. They might be getting stripped out somewhere in the ecosystem report generation if similar cases are working in the snapshot tests.

---

_Label `rule` added by @ntBre on 2025-10-05 21:57_

---

_Comment by @jvacek on 2025-10-06 17:08_

@ntBre Should I add some explicit note about these not being in the upstream package?

---

_Comment by @ntBre on 2025-10-06 18:03_

I think it's okay, but I'd have to double check what we've done for similar cases in the past.

---

_Renamed from "[ruff] Add rules for checking slashes in `path()` for Django" to "[flake8-django] Add rules for checking slashes in `path()`" by @jvacek on 2025-10-06 20:26_

---

_Converted to draft by @jvacek on 2025-10-07 11:49_

---

_Comment by @jvacek on 2025-10-07 11:49_

I'm gonna pull this down to Draft until I can get the CI to run

---

_Comment by @jvacek on 2025-10-07 12:28_

I removed the following check from the rules as I noticed that a lot of our `urls.py` files don't import anything from django.

This is a deviation from the other _django_ rules, but I believe the path checks will be sufficient for short-circuiting in non-django files, and the one less check is also an improvement for Django files. Curious what you think about this?

https://github.com/astral-sh/ruff/pull/20702/commits/cfe53bb43528be191e9d3358e0110fdbe2de7364#diff-f0f08bc80e806c09c11295057e6c8c9361be949dd66335a9e3ec9b49443d8a0fL63-L65

---

_Marked ready for review by @jvacek on 2025-10-07 12:38_

---

_@jvacek reviewed on 2025-10-07 12:41_

---

_Review comment by @jvacek on `crates/ruff/tests/cli/snapshots/cli__lint__requires_python_no_tool.snap`:15 on 2025-10-07 12:41_

I'm not sure what would cause these to be removed, is this intended?

---

_@ntBre reviewed on 2025-10-07 13:08_

---

_Review comment by @ntBre on `crates/ruff/tests/cli/snapshots/cli__lint__requires_python_no_tool.snap`:15 on 2025-10-07 13:08_

Yeah, newer versions of cargo-insta don't add this line, but it will linger around until someone updates a snapshot! So this is expected

---

_Comment by @jvacek on 2025-10-16 15:18_

@ntBre need anything else from me here still?

---

_Comment by @ntBre on 2025-10-16 15:28_

I don't think so, I just need some time to review :)

---

_Comment by @UnknownPlatypus on 2025-10-27 10:04_

Should these rule be added in the DJ1xx range in case the pylint plugin regain some traction to avoid conflicting rule codes ?

---

_Comment by @ntBre on 2025-10-27 12:49_

There was some discussion on Discord about that. It looks like the upstream's last commit was January 8th, 2024, so almost two years ago. That seemed long enough to me to add these in the DJ group, but I don't feel too strongly and am happy to reconsider if others do!

---

_Comment by @UnknownPlatypus on 2025-10-27 13:03_

I think it's fine to add to the DJ group too (otherwise it will add friction and confusion for people looking for Django related rules) but I would add new rules as code `DJ100` and `DJ101` instead of `DJ014` and `DJ015` to be extra-careful and maybe better convey these are not in the initial flake8 implementation. 
Maybe it's not necessary, feel free to ignore.

---

_Renamed from "[flake8-django] Add rules for checking slashes in `path()`" to "[`flake8-django`] Add rules for checking slashes in `path()`" by @amyreese on 2025-10-31 00:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:44 on 2025-10-31 15:48_

```suggestion
#[violation_metadata(preview_since = "0.14.4")]
```

We stopped using the `v` prefix a while back. Maybe I should try to add some validation in this macro, it's very new!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:45 on 2025-10-31 15:48_

I think the Rust convention is actually to use `Url`, even if it seems a bit weird.

```suggestion
pub(crate) struct DjangoUrlPathWithLeadingSlash {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:57 on 2025-10-31 15:51_

nit:


```suggestion
        "Remove the leading slash".to_string()
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:83 on 2025-10-31 15:56_

I found a couple of other rules where we do something like this:

https://github.com/astral-sh/ruff/blob/f70034f0476cffc07ed9d30e4922e7b9db54ee6e/crates/ruff_linter/src/rules/flake8_bandit/rules/unsafe_markup_use.rs#L124-L134

Basically it might be better to convert the `additional_path_functions` to `QualifiedName`s too instead of converting the `qualified_name` back to a string.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:90 on 2025-10-31 15:59_

If this can also be provided as a keyword we should use:

https://github.com/astral-sh/ruff/blob/f70034f0476cffc07ed9d30e4922e7b9db54ee6e/crates/ruff_python_ast/src/nodes.rs#L3325-L3327

Or we could use `find_positional` just above it, if it's positional-only.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:44 on 2025-10-31 16:05_

```suggestion
#[violation_metadata(preview_since = "0.14.4")]
```

We dropped the v prefix a while back. Maybe I should try to enforce this in the macro, it's new!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:95 on 2025-10-31 16:05_

We could use another let-else here to avoid some nesting, but I'm happy either way.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:122 on 2025-10-31 16:09_

I think we could use something like this here:

```suggestion
            let quote_len = value.first_literal_flags().quote_str().text_len();
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_with_leading_slash.rs`:128 on 2025-10-31 16:12_

Should this be an unsafe edit? From the rule description and what I found in the docs, it seems like this could change program behavior, making it unsafe. But maybe I'm being too pedantic, especially if the original behavior was probably unintentional.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:45 on 2025-10-31 16:15_

```suggestion
#[violation_metadata(preview_since = "0.14.4")]
pub(crate) struct DjangoUrlPathWithoutTrailingSlash {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:83 on 2025-10-31 16:15_

It might make sense to factor this out into a helper function if it's used in both rules.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:92 on 2025-10-31 16:16_

Same as  above, I think we can use one of our argument helper functions here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:125 on 2025-10-31 16:17_

Same suggestion as above again, using the quote flags.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:128 on 2025-10-31 16:19_

I'm pretty sure this is impossible to hit given the checks above, but we might want to use a `saturating_sub` or `checked_sub` here just in case the `route_arg.range().end()` somehow ends up less than `quote_len`.

I've caused a panic like this myself recently 😅 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:129 on 2025-10-31 16:20_

This one sounded safer to me in the docs, but just flagging to double check the safety of both rules.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_django/rules/url_path_without_trailing_slash.rs`:57 on 2025-10-31 16:21_

```suggestion
        "Add a trailing slash".to_string()
```

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:905 on 2025-10-31 16:29_

It's easy to miss this, but I think we're not supposed to add new settings here:

https://github.com/astral-sh/ruff/blob/f70034f0476cffc07ed9d30e4922e7b9db54ee6e/crates/ruff_workspace/src/options.rs#L867-L870

We should add this here instead:

https://github.com/astral-sh/ruff/blob/f70034f0476cffc07ed9d30e4922e7b9db54ee6e/crates/ruff_workspace/src/options.rs#L499-L505

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:1460 on 2025-10-31 16:32_

```suggestion
    pub extend_path_functions: Option<Vec<String>>,
```

I think `extend` is the more typical prefix for these settings. [extend-immutable-calls](https://docs.astral.sh/ruff/settings/#lint_flake8-bugbear_extend-immutable-calls) and [extend-allowed-calls](https://docs.astral.sh/ruff/settings/#lint_flake8-boolean-trap_extend-allowed-calls) are a couple of examples I looked at.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_django/DJ100.py`:46 on 2025-10-31 16:35_

Could we add some tests like these:

```pycon
>>> "/path\
... /".endswith("/")
True
>>> "\
... /path".startswith("/")
True
```

I think these could potentially cause problems with the string indexing we're doing in the fixes. One blunt solution would be just to skip fixes when there's a backslash in the route string.

---

_@ntBre requested changes on 2025-10-31 16:38_

Thank you! This looks great overall, I just had some minor suggestions.

It might be good to get a quick look from @amyreese too. She's almost definitely more familiar with django than I am!

---

_Label `preview` added by @ntBre on 2025-10-31 16:38_

---

_Comment by @MichaReiser on 2026-01-21 10:31_

Thank you for working on this. I'll close this PR due to inactivity. Feel free to resubmit it once the changes are addressed and we'll happily take another look

---

_Closed by @MichaReiser on 2026-01-21 10:31_

---
