```yaml
number: 15775
title: "[minor] Simplify some `ExprStringLiteral` creation logic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/simplify-string-literal-creations
created_at: 2025-01-27T18:45:28Z
updated_at: 2025-01-27T18:53:41Z
url: https://github.com/astral-sh/ruff/pull/15775
synced_at: 2026-01-10T19:57:22Z
```

# [minor] Simplify some `ExprStringLiteral` creation logic

---

_Pull request opened by @AlexWaygood on 2025-01-27 18:45_

## Summary

A couple of minor opportunities for simplification that I spotted as a result of reviewing https://github.com/astral-sh/ruff/pull/15726

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2025-01-27 18:45_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-27 18:45_

---

_Merged by @AlexWaygood on 2025-01-27 18:51_

---

_Closed by @AlexWaygood on 2025-01-27 18:51_

---

_Branch deleted on 2025-01-27 18:51_

---

_Comment by @github-actions[bot] on 2025-01-27 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+15 -15 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `"common.compat cncf.kubernetes"` instead of string join
- <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `'common.compat cncf.kubernetes'` instead of string join
- <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/crazy_functions/diagram_fns/file_tree.py#L22'>crazy_functions/diagram_fns/file_tree.py:22:9:</a> SIM108 Use ternary operator `suf = "..." if len(comment) > self.comment_maxlen_show else ""` instead of `if`-`else`-block
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/crazy_functions/diagram_fns/file_tree.py#L22'>crazy_functions/diagram_fns/file_tree.py:22:9:</a> SIM108 Use ternary operator `suf = '...' if len(comment) > self.comment_maxlen_show else ''` instead of `if`-`else`-block
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/main.py#L139'>main.py:139:39:</a> SIM401 Use `functional[k].get("Color", "secondary")` instead of an `if` block
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/main.py#L139'>main.py:139:39:</a> SIM401 Use `functional[k].get('Color', 'secondary')` instead of an `if` block
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `not TAICHU_API_KEY == ""` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `not TAICHU_API_KEY == ''` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 20:1:10: RUF005 Consider `["none", *sorted(kernels.keys())]` instead of concatenation
- examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 20:1:10: RUF005 Consider `['none', *sorted(kernels.keys())]` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = "index" if route == "/" else route[1:]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = 'index' if route == '/' else route[1:]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (",", ": ") if pretty else (",", ":")` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (',', ': ') if pretty else (',', ':')` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_selection_", defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_selection_', defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_hover_", defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_hover_', defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix="node_hover_", defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix='node_hover_', defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == "/" else key + p[0]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == '/' else key + p[0]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if "\n" not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if '\n' not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example["type"]) if example.get("type") is not None else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example['type']) if example.get('type') is not None else None` instead of `if`-`else`-block
... 1 additional changes omitted for rule SIM108
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/32f9ee7a62cdeb9cca5b694dc0fdb805dbcbff87/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/32f9ee7a62cdeb9cca5b694dc0fdb805dbcbff87/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if '.' in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 22 | 11 | 11 | 0 | 0 |
| FLY002 | 2 | 1 | 1 | 0 | 0 |
| SIM401 | 2 | 1 | 1 | 0 | 0 |
| SIM103 | 2 | 1 | 1 | 0 | 0 |
| RUF005 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+71 -71 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `"common.compat cncf.kubernetes"` instead of string join
- <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `'common.compat cncf.kubernetes'` instead of string join
- <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/f6b05fe243c17ffb80a4d2497fa7849f27ea507e/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/crazy_functions/diagram_fns/file_tree.py#L22'>crazy_functions/diagram_fns/file_tree.py:22:9:</a> SIM108 Use ternary operator `suf = "..." if len(comment) > self.comment_maxlen_show else ""` instead of `if`-`else`-block
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/crazy_functions/diagram_fns/file_tree.py#L22'>crazy_functions/diagram_fns/file_tree.py:22:9:</a> SIM108 Use ternary operator `suf = '...' if len(comment) > self.comment_maxlen_show else ''` instead of `if`-`else`-block
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/main.py#L139'>main.py:139:39:</a> SIM401 Use `functional[k].get("Color", "secondary")` instead of an `if` block
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/main.py#L139'>main.py:139:39:</a> SIM401 Use `functional[k].get('Color', 'secondary')` instead of an `if` block
- <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `TAICHU_API_KEY != ""` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/1213ef19e5e3d016e5a6cb73e007bc70bef98bb8/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `TAICHU_API_KEY != ''` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+65 -65 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/components_themed.py#L89'>examples/output/apis/components_themed.py:89:6:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(html, encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/components_themed.py#L89'>examples/output/apis/components_themed.py:89:6:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(html, encoding='utf-8')`
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 20:1:10: RUF005 Consider `["none", *sorted(kernels.keys())]` instead of concatenation
- examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 20:1:10: RUF005 Consider `['none', *sorted(kernels.keys())]` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:6:</a> FURB101 `open` and `read` should be replaced by `Path(join(dirname(__file__), "razzies-clean.csv")).read_text()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:6:</a> FURB101 `open` and `read` should be replaced by `Path(join(dirname(__file__), 'razzies-clean.csv')).read_text()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L83'>examples/server/app/movies/main.py:83:25:</a> PLC1901 `director_val != ""` can be simplified to `director_val` as an empty string is falsey
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L83'>examples/server/app/movies/main.py:83:25:</a> PLC1901 `director_val != ''` can be simplified to `director_val` as an empty string is falsey
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L85'>examples/server/app/movies/main.py:85:21:</a> PLC1901 `cast_val != ""` can be simplified to `cast_val` as an empty string is falsey
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L85'>examples/server/app/movies/main.py:85:21:</a> PLC1901 `cast_val != ''` can be simplified to `cast_val` as an empty string is falsey
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L123'>src/bokeh/application/handlers/directory.py:123:18:</a> FURB101 `open` and `read` should be replaced by `Path(init_py).read_text(encoding="utf8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L123'>src/bokeh/application/handlers/directory.py:123:18:</a> FURB101 `open` and `read` should be replaced by `Path(init_py).read_text(encoding='utf8')`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = "index" if route == "/" else route[1:]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = 'index' if route == '/' else route[1:]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L175'>src/bokeh/command/subcommands/file_output.py:175:22:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(content, encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L175'>src/bokeh/command/subcommands/file_output.py:175:22:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(content, encoding='utf-8')`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (",", ": ") if pretty else (",", ":")` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (',', ': ') if pretty else (',', ':')` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L264'>src/bokeh/io/export.py:264:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L264'>src/bokeh/io/export.py:264:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding='utf-8')`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L294'>src/bokeh/io/export.py:294:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L294'>src/bokeh/io/export.py:294:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding='utf-8')`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L311'>src/bokeh/io/export.py:311:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L311'>src/bokeh/io/export.py:311:14:</a> FURB103 `open` and `write` should be replaced by `Path(tmp.path).write_text(html, encoding='utf-8')`
... 7 additional changes omitted for rule FURB103
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/callbacks.py#L200'>src/bokeh/models/callbacks.py:200:14:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text(encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/callbacks.py#L200'>src/bokeh/models/callbacks.py:200:14:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text(encoding='utf-8')`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_selection_", defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_selection_', defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_hover_", defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_hover_', defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix="node_hover_", defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix='node_hover_', defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
... 7 additional changes omitted for rule SIM108
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_tools.py#L188'>src/bokeh/plotting/_tools.py:188:20:</a> PLC1901 `tool == ""` can be simplified to `not tool` as an empty string is falsey
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_tools.py#L188'>src/bokeh/plotting/_tools.py:188:20:</a> PLC1901 `tool == ''` can be simplified to `not tool` as an empty string is falsey
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/us_holidays.py#L79'>src/bokeh/sampledata/us_holidays.py:79:10:</a> FURB101 `open` and `read` should be replaced by `Path(package_path("USHolidays.ics")).read_text()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sampledata/us_holidays.py#L79'>src/bokeh/sampledata/us_holidays.py:79:10:</a> FURB101 `open` and `read` should be replaced by `Path(package_path('USHolidays.ics')).read_text()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L151'>src/bokeh/server/util.py:151:28:</a> PLC1901 `parts[0] == ""` can be simplified to `not parts[0]` as an empty string is falsey
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L151'>src/bokeh/server/util.py:151:28:</a> PLC1901 `parts[0] == ''` can be simplified to `not parts[0]` as an empty string is falsey
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L159'>src/bokeh/server/util.py:159:28:</a> PLC1901 `parts[0] == ""` can be simplified to `not parts[0]` as an empty string is falsey
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L159'>src/bokeh/server/util.py:159:28:</a> PLC1901 `parts[0] == ''` can be simplified to `not parts[0]` as an empty string is falsey
... 77 additional changes omitted for rule PLC1901
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L194'>src/bokeh/util/compiler.py:194:14:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text(encoding="utf-8")`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L194'>src/bokeh/util/compiler.py:194:14:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text(encoding='utf-8')`
... 1 additional changes omitted for rule FURB101
... 88 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/32f9ee7a62cdeb9cca5b694dc0fdb805dbcbff87/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/32f9ee7a62cdeb9cca5b694dc0fdb805dbcbff87/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if '.' in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC1901 | 86 | 43 | 43 | 0 | 0 |
| SIM108 | 22 | 11 | 11 | 0 | 0 |
| FURB103 | 16 | 8 | 8 | 0 | 0 |
| FURB101 | 10 | 5 | 5 | 0 | 0 |
| FLY002 | 2 | 1 | 1 | 0 | 0 |
| SIM401 | 2 | 1 | 1 | 0 | 0 |
| SIM103 | 2 | 1 | 1 | 0 | 0 |
| RUF005 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
