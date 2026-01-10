```yaml
number: 21163
title: "Consistently wrap tokens in parser diagnostics in `backticks` instead of 'quotes'"
type: pull_request
state: merged
author: lucach
labels:
  - linter
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: tokens_backticks
created_at: 2025-10-31T13:39:14Z
updated_at: 2025-10-31T15:59:12Z
url: https://github.com/astral-sh/ruff/pull/21163
synced_at: 2026-01-10T16:59:49Z
```

# Consistently wrap tokens in parser diagnostics in `backticks` instead of 'quotes'

---

_Pull request opened by @lucach on 2025-10-31 13:39_

The parser currently uses single quotes to wrap tokens. This is inconsistent with the rest of ruff/ty, which use backticks.

For example, see the inconsistent diagnostics produced in this simple example: https://play.ty.dev/0a9d6eab-6599-4a1d-8e40-032091f7f50f

Consistently wrapping tokens in backticks produces uniform diagnostics. Following the style decision of #723, in #2889 some quotes were already switched into backticks.

This is also in line with Rust's guide on diagnostics (https://rustc-dev-guide.rust-lang.org/diagnostics.html#diagnostic-structure):

> When code or an identifier must appear in a message or label, it should be surrounded with backticks


---

_Review requested from @carljm by @lucach on 2025-10-31 13:39_

---

_Review requested from @AlexWaygood by @lucach on 2025-10-31 13:39_

---

_Review requested from @sharkdp by @lucach on 2025-10-31 13:39_

---

_Review requested from @dcreager by @lucach on 2025-10-31 13:39_

---

_Review requested from @MichaReiser by @lucach on 2025-10-31 13:39_

---

_Review requested from @dhruvmanila by @lucach on 2025-10-31 13:39_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-31 13:44_

---

_Comment by @github-actions[bot] on 2025-10-31 14:02_

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
ℹ️ ecosystem check **detected linter changes**. (+1843 -1843 violations, +0 -0 fixes in 9 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L30'>airflow-core/src/airflow/operators/__init__.py:30:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L30'>airflow-core/src/airflow/operators/__init__.py:30:13:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L40'>airflow-core/src/airflow/operators/__init__.py:40:11:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L40'>airflow-core/src/airflow/operators/__init__.py:40:11:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L43'>airflow-core/src/airflow/operators/__init__.py:43:15:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L43'>airflow-core/src/airflow/operators/__init__.py:43:15:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L67'>airflow-core/src/airflow/operators/__init__.py:67:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/operators/__init__.py#L67'>airflow-core/src/airflow/operators/__init__.py:67:13:</a> E231 [*] Missing whitespace after `:`
- <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/sensors/__init__.py#L35'>airflow-core/src/airflow/sensors/__init__.py:35:13:</a> E231 [*] Missing whitespace after ':'
+ <a href='https://github.com/apache/airflow/blob/0448569f0a350c8b94924cdd462406133c407c3c/airflow-core/src/airflow/sensors/__init__.py#L35'>airflow-core/src/airflow/sensors/__init__.py:35:13:</a> E231 [*] Missing whitespace after `:`
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
- <a href='https://github.com/mlflow/mlflow/blob/c9b697c6615c4535c3d2c5655e08563246371986/mlflow/types/llm.py#L829'>mlflow/types/llm.py:829:42:</a> E231 [*] Missing whitespace after ','
+ <a href='https://github.com/mlflow/mlflow/blob/c9b697c6615c4535c3d2c5655e08563246371986/mlflow/types/llm.py#L829'>mlflow/types/llm.py:829:42:</a> E231 [*] Missing whitespace after `,`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select A,E703,F704,B015,B018,D100</pre>
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
... 18 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E231 | 3658 | 1829 | 1829 | 0 | 0 |
| invalid-syntax: | 28 | 14 | 14 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@Gankra approved on 2025-10-31 15:55_

Nice, thanks!

---

_Label `linter` added by @Gankra on 2025-10-31 15:57_

---

_Label `ty` added by @Gankra on 2025-10-31 15:57_

---

_Label `diagnostics` added by @Gankra on 2025-10-31 15:57_

---

_Renamed from "Consistently wrap tokens in backticks" to "Consistently wrap tokens in parser diagnostics in `backticks` instead of 'quotes'" by @Gankra on 2025-10-31 15:57_

---

_Renamed from "Consistently wrap tokens in parser diagnostics in `backticks` instead of 'quotes'" to "Consistently wrap tokens in parser diagnostics in \`backticks\` instead of 'quotes'" by @Gankra on 2025-10-31 15:58_

---

_Renamed from "Consistently wrap tokens in parser diagnostics in \`backticks\` instead of 'quotes'" to "Consistently wrap tokens in parser diagnostics in `backticks` instead of 'quotes'" by @Gankra on 2025-10-31 15:58_

---

_Merged by @Gankra on 2025-10-31 15:59_

---

_Closed by @Gankra on 2025-10-31 15:59_

---
