```yaml
number: 15726
title: Preserve quote style in generated code
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/quote-style
created_at: 2025-01-24T16:55:37Z
updated_at: 2025-01-27T18:41:06Z
url: https://github.com/astral-sh/ruff/pull/15726
synced_at: 2026-01-12T15:55:52Z
```

# Preserve quote style in generated code

---

_@ntBre_

## Summary

This is a first step toward fixing #7799 by using the quoting style stored in the `flags` field on `ast::StringLiteral`s to select a quoting style. This PR does not include support for f-strings or byte strings.

Several rules also needed small updates to pass along existing quoting styles instead of using `StringLiteralFlags::default()`. The remaining snapshot changes are intentional and should preserve the quotes from the input strings. However, this has the unsatisfying, if not outright incorrect, effect of changes like the `quote3` case I note below.

I'm also not entirely happy with the new `DYNAMIC` flag, but I think it's useful to be able to determine whether the string flags are set intentionally or if we really want single quotes, which are the current default. I considered making the flags field an `Option` but thought this would be a less intrusive change. This test case was the main one I couldn't fix without this flag:

https://github.com/astral-sh/ruff/blob/aa2e3e2ce59469d1ed598c3efe625097ed4bdb47/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP018.py#L45

There's no string literal to inherit the quote style from, and inheriting it from `checker.stylist.quote()` yields `f"{""}"`, which is valid in Python 3.12+, but I don't think the generator knows that (cf #9660).

## Test Plan

Existing tests with some accepted updates, plus a few new RUF055 tests for raw strings.

Fixes #14132


---

_Comment by @github-actions[bot] on 2025-01-24 17:04_

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
+ <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `"common.compat cncf.kubernetes"` instead of string join
- <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `'common.compat cncf.kubernetes'` instead of string join
- <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
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
+ <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `"common.compat cncf.kubernetes"` instead of string join
- <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/dev/breeze/src/airflow_breeze/global_constants.py#L591'>dev/breeze/src/airflow_breeze/global_constants.py:591:25:</a> FLY002 Consider `'common.compat cncf.kubernetes'` instead of string join
- <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/d024cdab190eb46eb0ce21679f44f08df5690cb9/providers/src/airflow/providers/google/suite/hooks/drive.py#L194'>providers/src/airflow/providers/google/suite/hooks/drive.py:194:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
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

_Label `fixes` added by @AlexWaygood on 2025-01-24 17:04_

---

_Marked ready for review by @ntBre on 2025-01-24 17:20_

---

_Comment by @ntBre on 2025-01-24 17:21_

All of the ecosystem changes look like what I expected. The new version preserves the initial quoting styles instead of using the default single quotes.

---

_Comment by @MichaReiser on 2025-01-24 17:59_

I haven't looked at the implementation closely because it's late here. So I might be off with what I write here. 

Generally, my expectation would be that the generator only preserves quotes if it doesn't lead to a syntax error due to invalid quotes,e.g in nested f-strings. This would be similar to using the formatter with the `QuoteStyle::Preserve`. Unfortunately, just preserving isn't always possible ([formatter logic](https://github.com/astral-sh/ruff/blob/420365811f27d597ea33a62270667ce9cee1bb5f/crates/ruff_python_formatter/src/string/normalize.rs#L36-L153)). 

I agree with you that adding a `DYNAMIC` to `StringFlags` seems off. Do we need that functionality on a per string level or would it be enough to configure it at the generator level?

---

_Comment by @ntBre on 2025-01-24 18:17_

No worries, thanks for taking a look!

> Generally, my expectation would be that the generator only preserves quotes if it doesn't lead to a syntax error due to invalid quotes,e.g in nested f-strings.

This is definitely important to keep in mind. I *think* this is handled by still calling `Generator::p_str_repr`, which calls `UnicodeEscape::with_preferred_quote`. Now we get the "preferred" quote from the string rather than the `Generator`, but the escape code can still override it. Only the raw string case skips this escaping path.

> I agree with you that adding a `DYNAMIC` to `StringFlags` seems off. Do we need that functionality on a per string level or would it be enough to configure it at the generator level?

This is an interesting idea. I'll give it a try, thanks!


---

_Comment by @MichaReiser on 2025-01-24 18:32_

Thanks for explaining. 

Another alternative to `DYNAMIC`: Could you use a stylist to determine the preferred quote in this one fixer where you don't have access to an existing quote style? Never mind, you explained why this doesn't work. Although I kind of would expect the generator not to generated invalid code. 

---

_Comment by @ntBre on 2025-01-24 18:45_

> Although I kind of would expect the generator not to generated invalid code.

Yeah this case was pretty confusing to me. I think it only coincidentally worked in the original code because the default quotes are single quotes. It only uses the `Generator` to build the replacement for `str()`, so it doesn't consider the enclosing f-string at all: https://github.com/astral-sh/ruff/blob/aa2e3e2ce59469d1ed598c3efe625097ed4bdb47/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs#L200-L201

Maybe I focused too much on replicating the current behavior instead of fixing a latent bug.

---

_Comment by @MichaReiser on 2025-01-24 19:08_

Oh interesting. That makes sense and seems a general problem with how we use the `Generator`. The `Generator` can only generate valid code if it knows about the surrounding quotes but it seems we don't forward them here. 

To me, that suggests that we should track the `quote_style` of the enclosing string (if any) in `Checker` as we traverse and pass it on to the `Generator` to ensure that it is aware of any surrounding quotes. 

---

_Comment by @AlexWaygood on 2025-01-24 19:15_

Going back to the problem of how the `Generator` should determine whether to use the quote-style from the stylist versus the quote-style from the flags... here's another idea: I wonder if we could remove the `Default` implementation for `StringLiteralFlags`, and instead implement `From<&Stylist>` for `StringLiteralFlags`. That way we'd add an easy way to construct a new `StringLiteralFlags` instance with the stylist-determined preferred quote, but we'd also force users of the API to think carefully about whether they really want an empty `StringLiteralFlags` instance or whether they should be taking the quote style from somewhere else. If we did this, I think we could have the generator only ever use the quote style from the flags rather than ever looking at the quote style given to it by the stylist. Here's a quick prototype (that I'm sure could be cleaned up a bit). (The diff is relative to `main` rather than your branch, I'm afraid!)

<details>
<summary>Prototype</summary>

```diff
diff --git a/crates/ruff_linter/src/checkers/ast/analyze/expression.rs b/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
index 74d877f91..a39ffae63 100644
--- a/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
+++ b/crates/ruff_linter/src/checkers/ast/analyze/expression.rs
@@ -506,7 +506,7 @@ pub(crate) fn expression(expr: &Expr, checker: &mut Checker) {
                                     checker,
                                     attr,
                                     call,
-                                    string_value.to_str(),
+                                    string_value,
                                 );
                             }
                         } else if attr == "format" {
diff --git a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs
index 9323ab6d7..16c60c8a5 100644
--- a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs
+++ b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs
@@ -4,8 +4,8 @@ use ruff_diagnostics::{Diagnostic, Edit, Fix, FixAvailability, Violation};
 use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::comparable::ComparableExpr;
 use ruff_python_ast::parenthesize::parenthesized_range;
-use ruff_python_ast::{self as ast, Expr, ExprCall, ExprContext};
-use ruff_python_codegen::Generator;
+use ruff_python_ast::{self as ast, Expr, ExprCall, ExprContext, StringLiteralFlags};
+use ruff_python_codegen::{Generator, Stylist};
 use ruff_python_trivia::CommentRanges;
 use ruff_python_trivia::{SimpleTokenKind, SimpleTokenizer};
 use ruff_text_size::{Ranged, TextRange, TextSize};
@@ -280,7 +280,7 @@ impl Violation for PytestDuplicateParametrizeTestCases {
     }
 }
 
-fn elts_to_csv(elts: &[Expr], generator: Generator) -> Option<String> {
+fn elts_to_csv(elts: &[Expr], generator: Generator, stylist: &Stylist) -> Option<String> {
     if !elts.iter().all(Expr::is_string_literal_expr) {
         return None;
     }
@@ -298,7 +298,8 @@ fn elts_to_csv(elts: &[Expr], generator: Generator) -> Option<String> {
                 acc
             })
             .into_boxed_str(),
-        ..ast::StringLiteral::default()
+        range: TextRange::default(),
+        flags: StringLiteralFlags::from(stylist),
     });
     Some(generator.expr(&node))
 }
@@ -358,8 +359,9 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                                 .iter()
                                 .map(|name| {
                                     Expr::from(ast::StringLiteral {
-                                        value: (*name).to_string().into_boxed_str(),
-                                        ..ast::StringLiteral::default()
+                                        value: Box::from(*name),
+                                        range: TextRange::default(),
+                                        flags: StringLiteralFlags::from(checker.stylist()),
                                     })
                                 })
                                 .collect(),
@@ -393,8 +395,9 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                                 .iter()
                                 .map(|name| {
                                     Expr::from(ast::StringLiteral {
-                                        value: (*name).to_string().into_boxed_str(),
-                                        ..ast::StringLiteral::default()
+                                        value: Box::from(*name),
+                                        range: TextRange::default(),
+                                        flags: StringLiteralFlags::from(checker.stylist()),
                                     })
     if !elts.iter().all(Expr::is_string_literal_expr) {
         return None;
     }
@@ -298,7 +298,8 @@ fn elts_to_csv(elts: &[Expr], generator: Generator) -> Option<String> {
                 acc
             })
             .into_boxed_str(),
-        ..ast::StringLiteral::default()
+        range: TextRange::default(),
+        flags: StringLiteralFlags::from(stylist),
     });
     Some(generator.expr(&node))
 }
@@ -358,8 +359,9 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                                 .iter()
                                 .map(|name| {
                                     Expr::from(ast::StringLiteral {
-                                        value: (*name).to_string().into_boxed_str(),
-                                        ..ast::StringLiteral::default()
+                                        value: Box::from(*name),
+                                        range: TextRange::default(),
+                                        flags: StringLiteralFlags::from(checker.stylist()),
                                     })
                                 })
                                 .collect(),
@@ -393,8 +395,9 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                                 .iter()
                                 .map(|name| {
                                     Expr::from(ast::StringLiteral {
-                                        value: (*name).to_string().into_boxed_str(),
-                                        ..ast::StringLiteral::default()
+                                        value: Box::from(*name),
+                                        range: TextRange::default(),
+                                        flags: StringLiteralFlags::from(checker.stylist()),
                                     })
                                 })
                                 .collect(),
@@ -444,7 +447,7 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                             },
                             expr.range(),
                         );
-                        if let Some(content) = elts_to_csv(elts, checker.generator()) {
+                        if let Some(content) = elts_to_csv(elts, checker.generator(), checker.stylist()) {
                             diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
                                 content,
                                 expr.range(),
@@ -489,7 +492,7 @@ fn check_names(checker: &mut Checker, call: &ExprCall, expr: &Expr, argvalues: &
                             },
                             expr.range(),
                         );
-                        if let Some(content) = elts_to_csv(elts, checker.generator()) {
+                        if let Some(content) = elts_to_csv(elts, checker.generator(), checker.stylist()) {
                             diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
                                 content,
                                 expr.range(),
diff --git a/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs b/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs
index 50c17cfeb..cb4aa3317 100644
--- a/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs
+++ b/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs
@@ -1,7 +1,7 @@
 use ruff_python_ast::{
     self as ast, str_prefix::StringLiteralPrefix, Arguments, Expr, StringLiteralFlags,
 };
-use ruff_text_size::Ranged;
+use ruff_text_size::{Ranged, TextRange};
 
 use crate::fix::snippet::SourceCodeSnippet;
 use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix, FixAvailability, Violation};
@@ -220,14 +220,14 @@ fn check_os_environ_subscript(checker: &mut Checker, expr: &Expr) {
     );
     let node = ast::StringLiteral {
         value: capital_env_var.into_boxed_str(),
-        flags: StringLiteralFlags::default().with_prefix({
+        flags: StringLiteralFlags::from(checker.stylist()).with_prefix({
             if env_var.is_unicode() {
                 StringLiteralPrefix::Unicode
             } else {
                 StringLiteralPrefix::Empty
             }
         }),
-        ..ast::StringLiteral::default()
+        range: TextRange::default(),
     };
     let new_env_var = node.into();
     diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
diff --git a/crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs b/crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs
index 375d30b49..a46635d5f 100644
--- a/crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs
+++ b/crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs
@@ -3,8 +3,8 @@ use std::cmp::Ordering;
 use ruff_diagnostics::{Applicability, Diagnostic, Edit, Fix, FixAvailability, Violation};
 use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::{
-    Expr, ExprCall, ExprContext, ExprList, ExprStringLiteral, ExprUnaryOp, StringLiteral,
-    StringLiteralFlags, StringLiteralValue, UnaryOp,
+    str::Quote, Expr, ExprCall, ExprContext, ExprList, ExprStringLiteral, ExprUnaryOp, StringFlags,
+    StringLiteral, StringLiteralFlags, StringLiteralValue, UnaryOp,
 };
 use ruff_text_size::{Ranged, TextRange};
 
@@ -62,7 +62,7 @@ pub(crate) fn split_static_string(
     checker: &mut Checker,
     attr: &str,
     call: &ExprCall,
-    str_value: &str,
+    str_value: &StringLiteralValue,
 ) {
     let ExprCall { arguments, .. } = call;
 
@@ -115,16 +115,16 @@ pub(crate) fn split_static_string(
     checker.diagnostics.push(diagnostic);
 }
 
-fn construct_replacement(elts: &[&str]) -> Expr {
+fn construct_replacement(elts: &[&str], quote: Quote) -> Expr {
     Expr::List(ExprList {
         elts: elts
             .iter()
             .map(|elt| {
                 Expr::StringLiteral(ExprStringLiteral {
                     value: StringLiteralValue::single(StringLiteral {
-                        value: (*elt).to_string().into_boxed_str(),
+                        value: Box::from(*elt),
                         range: TextRange::default(),
-                        flags: StringLiteralFlags::default(),
+                        flags: StringLiteralFlags::empty().with_quote_style(quote),
                     }),
                     range: TextRange::default(),
                 })
@@ -135,7 +135,7 @@ fn construct_replacement(elts: &[&str]) -> Expr {
     })
 }
 
-fn split_default(str_value: &str, max_split: i32) -> Option<Expr> {
+fn split_default(str_value: &StringLiteralValue, max_split: i32) -> Option<Expr> {
     // From the Python documentation:
     // > If sep is not specified or is None, a different splitting algorithm is applied: runs of
     // > consecutive whitespace are regarded as a single separator, and the result will contain
@@ -151,30 +151,43 @@ fn split_default(str_value: &str, max_split: i32) -> Option<Expr> {
             None
         }
         Ordering::Equal => {
-            let list_items: Vec<&str> = vec![str_value];
-            Some(construct_replacement(&list_items))
+            let list_items: Vec<&str> = vec![str_value.to_str()];
+            Some(construct_replacement(
+                &list_items,
+                str_value.flags().quote_style(),
+            ))
         }
         Ordering::Less => {
-            let list_items: Vec<&str> = str_value.split_whitespace().collect();
-            Some(construct_replacement(&list_items))
+            let list_items: Vec<&str> = str_value.to_str().split_whitespace().collect();
+            Some(construct_replacement(
+                &list_items,
+                str_value.flags().quote_style(),
+            ))
         }
     }
 }
 
-fn split_sep(str_value: &str, sep_value: &str, max_split: i32, direction: Direction) -> Expr {
+fn split_sep(
+    str_value: &StringLiteralValue,
+    sep_value: &str,
+    max_split: i32,
+    direction: Direction,
+) -> Expr {
+    let value = str_value.to_str();
+    let quote = str_value.flags().quote_style();
     let list_items: Vec<&str> = if let Ok(split_n) = usize::try_from(max_split) {
         match direction {
-            Direction::Left => str_value.splitn(split_n + 1, sep_value).collect(),
-            Direction::Right => str_value.rsplitn(split_n + 1, sep_value).collect(),
+            Direction::Left => value.splitn(split_n + 1, sep_value).collect(),
+            Direction::Right => value.rsplitn(split_n + 1, sep_value).collect(),
         }
     } else {
         match direction {
-            Direction::Left => str_value.split(sep_value).collect(),
-            Direction::Right => str_value.rsplit(sep_value).collect(),
+            Direction::Left => value.split(sep_value).collect(),
+            Direction::Right => value.rsplit(sep_value).collect(),
         }
     };
 
-    construct_replacement(&list_items)
+    construct_replacement(&list_items, quote)
 }
 
 /// Returns the value of the `maxsplit` argument as an `i32`, if it is a numeric value.
diff --git a/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs b/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
index c6881536f..c5979c8c9 100644
--- a/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
+++ b/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
@@ -366,7 +366,7 @@ impl<'a> QuoteAnnotator<'a> {
         generator.expr(&Expr::from(ast::StringLiteral {
             range: TextRange::default(),
             value: annotation.into_boxed_str(),
-            flags: ast::StringLiteralFlags::default(),
+            flags: ast::StringLiteralFlags::from(self.stylist),
         }))
     }
 
diff --git a/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs b/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
index af100938e..d7c0cefda 100644
--- a/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
+++ b/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
@@ -75,7 +75,14 @@ fn build_fstring(joiner: &str, joinees: &[Expr]) -> Option<Expr> {
                 })
                 .join(joiner)
                 .into_boxed_str(),
-            ..ast::StringLiteral::default()
+            range: TextRange::default(),
+            flags: joinees
+                .first()
+                .unwrap()
+                .as_string_literal_expr()
+                .unwrap()
+                .value
+                .flags(),
         };
         return Some(node.into());
     }
diff --git a/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs b/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
index 1aa1ea934..812fd81e1 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
@@ -163,7 +163,7 @@ fn generate_keyword_fix(checker: &Checker, call: &ast::ExprCall) -> Fix {
                 .expr(&Expr::StringLiteral(ast::ExprStringLiteral {
                     value: ast::StringLiteralValue::single(ast::StringLiteral {
                         value: "utf-8".to_string().into_boxed_str(),
-                        flags: StringLiteralFlags::default(),
+                        flags: StringLiteralFlags::from(checker.stylist()),
                         range: TextRange::default(),
                     }),
                     range: TextRange::default(),
diff --git a/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs b/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs
index 4e71ee73a..702249bec 100644
--- a/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs
+++ b/crates/ruff_linter/src/rules/pyupgrade/rules/native_literals.rs
@@ -35,7 +35,12 @@ impl FromStr for LiteralType {
 impl LiteralType {
     fn as_zero_value_expr(self) -> Expr {
         match self {
-            LiteralType::Str => ast::ExprStringLiteral::default().into(),
+            LiteralType::Str => ast::StringLiteral {
+                value: Box::default(),
+                range: TextRange::default(),
+                flags: ast::StringLiteralFlags::empty(),
+            }
+            .into(),
             LiteralType::Bytes => ast::ExprBytesLiteral::default().into(),
             LiteralType::Int => ast::ExprNumberLiteral {
                 value: ast::Number::Int(Int::from(0u8)),
diff --git a/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs b/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
index da9b3de4f..4a2b7ce0b 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
@@ -66,7 +66,8 @@ pub(crate) fn assert_with_print_message(checker: &mut Checker, stmt: &ast::StmtA
             diagnostic.set_fix(Fix::unsafe_edit(Edit::range_replacement(
                 checker.generator().stmt(&Stmt::Assert(ast::StmtAssert {
                     test: stmt.test.clone(),
-                    msg: print_arguments::to_expr(&call.arguments).map(Box::new),
+                    msg: print_arguments::to_expr(&call.arguments, checker.stylist().quote())
+                        .map(Box::new),
                     range: TextRange::default(),
                 })),
                 // We have to replace the entire statement,
@@ -89,9 +90,9 @@ pub(crate) fn assert_with_print_message(checker: &mut Checker, stmt: &ast::StmtA
 mod print_arguments {
     use itertools::Itertools;
     use ruff_python_ast::{
-        Arguments, ConversionFlag, Expr, ExprFString, ExprStringLiteral, FString, FStringElement,
-        FStringElements, FStringExpressionElement, FStringFlags, FStringLiteralElement,
-        FStringValue, StringLiteral, StringLiteralFlags, StringLiteralValue,
+        str::Quote, Arguments, ConversionFlag, Expr, ExprFString, ExprStringLiteral, FString,
+        FStringElement, FStringElements, FStringExpressionElement, FStringFlags,
+        FStringLiteralElement, FStringValue, StringLiteral, StringLiteralFlags, StringLiteralValue,
     };
     use ruff_text_size::TextRange;
 
@@ -140,12 +141,13 @@ mod print_arguments {
     /// literals.
     fn fstring_elements_to_string_literals<'a>(
         mut elements: impl ExactSizeIterator<Item = &'a FStringElement>,
+        quote: Quote,
     ) -> Option<Vec<StringLiteral>> {
         elements.try_fold(Vec::with_capacity(elements.len()), |mut acc, element| {
             if let FStringElement::Literal(literal) = element {
                 acc.push(StringLiteral {
                     value: literal.value.clone(),
-                    flags: StringLiteralFlags::default(),
+                    flags: StringLiteralFlags::empty().with_quote_style(quote),
                     range: TextRange::default(),
                 });
                 Some(acc)
@@ -162,6 +164,7 @@ mod print_arguments {
     fn args_to_string_literal_expr<'a>(
         args: impl ExactSizeIterator<Item = &'a Vec<FStringElement>>,
         sep: impl ExactSizeIterator<Item = &'a FStringElement>,
+        quote: Quote,
     ) -> Option<Expr> {
         // If there are no arguments, short-circuit and return `None`
         if args.len() == 0 {
@@ -174,8 +177,8 @@ mod print_arguments {
         // of a concatenated string literal. (e.g. "text", "text" "text") The `sep` will
         // be inserted only between the outer Vecs.
         let (Some(sep), Some(args)) = (
-            fstring_elements_to_string_literals(sep),
-            args.map(|arg| fstring_elements_to_string_literals(arg.iter()))
+            fstring_elements_to_string_literals(sep, quote),
+            args.map(|arg| fstring_elements_to_string_literals(arg.iter(), quote))
                 .collect::<Option<Vec<_>>>(),
         ) else {
             // If any of the arguments are not string literals, return None
@@ -203,7 +206,7 @@ mod print_arguments {
             range: TextRange::default(),
             value: StringLiteralValue::single(StringLiteral {
                 value: combined_string.into(),
-                flags: StringLiteralFlags::default(),
+                flags: StringLiteralFlags::empty().with_quote_style(quote),
                 range: TextRange::default(),
             }),
         }))
@@ -256,7 +259,7 @@ mod print_arguments {
     /// - [`Some`]<[`Expr::StringLiteral`]> if all arguments including `sep` are string literals.
     /// - [`Some`]<[`Expr::FString`]> if any of the arguments are not string literals.
     /// - [`None`] if the `print` contains no positional arguments at all.
-    pub(super) fn to_expr(arguments: &Arguments) -> Option<Expr> {
+    pub(super) fn to_expr(arguments: &Arguments, quote: Quote) -> Option<Expr> {
         // Convert the `sep` argument into `FStringElement`s
         let sep = arguments
             .find_keyword("sep")
@@ -286,7 +289,7 @@ mod print_arguments {
 
         // Attempt to convert the `sep` and `args` arguments to a string literal,
         // falling back to an f-string if the arguments are not all string literals.
-        args_to_string_literal_expr(args.iter(), sep.iter())
+        args_to_string_literal_expr(args.iter(), sep.iter(), quote)
             .or_else(|| args_to_fstring_expr(args.into_iter(), sep.into_iter()))
     }
 }
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index ac779c56a..71fd1de12 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1221,14 +1221,14 @@ impl fmt::Debug for FStringElements {
 
 /// An AST node that represents either a single string literal or an implicitly
 /// concatenated string literals.
-#[derive(Clone, Debug, Default, PartialEq)]
+#[derive(Clone, Debug, PartialEq)]
 pub struct ExprStringLiteral {
     pub range: TextRange,
     pub value: StringLiteralValue,
 }
 
 /// The value representing a [`ExprStringLiteral`].
-#[derive(Clone, Debug, Default, PartialEq)]
+#[derive(Clone, Debug, PartialEq)]
 pub struct StringLiteralValue {
     inner: StringLiteralValueInner,
 }
@@ -1241,6 +1241,18 @@ impl StringLiteralValue {
         }
     }
 
+    /// Returns the [`StringLiteralFlags`] associated with this string literal.
+    ///
+    /// For an implicitly concatenated string, it returns the flags for the first literal.
+    pub fn flags(&self) -> StringLiteralFlags {
+        self.iter()
+            .next()
+            .expect(
+                "There should always be at least one string literal in an `ExprStringLiteral` node",
+            )
+            .flags
+    }
+
     /// Creates a new string literal with the given values that represents an
     /// implicitly concatenated strings.
     ///
@@ -1371,12 +1383,6 @@ enum StringLiteralValueInner {
     Concatenated(ConcatenatedStringLiteral),
 }
 
-impl Default for StringLiteralValueInner {
-    fn default() -> Self {
-        Self::Single(StringLiteral::default())
-    }
-}
-
 bitflags! {
     #[derive(Debug, Default, Copy, Clone, PartialEq, Eq, Hash)]
     struct StringLiteralFlagsInner: u8 {
@@ -1414,10 +1420,14 @@ bitflags! {
 
 /// Flags that can be queried to obtain information
 /// regarding the prefixes and quotes used for a string literal.
-#[derive(Default, Copy, Clone, Eq, PartialEq, Hash)]
+#[derive(Copy, Clone, Eq, PartialEq, Hash)]
 pub struct StringLiteralFlags(StringLiteralFlagsInner);
 
 impl StringLiteralFlags {
+    pub fn empty() -> Self {
+        Self(StringLiteralFlagsInner::empty())
+    }
+
     #[must_use]
     pub fn with_quote_style(mut self, quote_style: Quote) -> Self {
         self.0
@@ -1520,7 +1530,7 @@ impl fmt::Debug for StringLiteralFlags {
 
 /// An AST node that represents a single string literal which is part of an
 /// [`ExprStringLiteral`].
-#[derive(Clone, Debug, Default, PartialEq)]
+#[derive(Clone, Debug, PartialEq)]
 pub struct StringLiteral {
     pub range: TextRange,
     pub value: Box<str>,
@@ -1546,7 +1556,7 @@ impl StringLiteral {
         Self {
             range,
             value: "".into(),
-            flags: StringLiteralFlags::default().with_invalid(),
+            flags: StringLiteralFlags::empty().with_invalid(),
         }
     }
 }
@@ -2115,7 +2125,7 @@ impl From<AnyStringFlags> for StringLiteralFlags {
                 value.prefix()
             )
         };
-        let new = StringLiteralFlags::default()
+        let new = StringLiteralFlags::empty()
             .with_quote_style(value.quote_style())
             .with_prefix(prefix);
         if value.is_triple_quoted() {
diff --git a/crates/ruff_python_codegen/src/stylist.rs b/crates/ruff_python_codegen/src/stylist.rs
index 20217c279..49b5fa380 100644
--- a/crates/ruff_python_codegen/src/stylist.rs
+++ b/crates/ruff_python_codegen/src/stylist.rs
@@ -4,6 +4,7 @@ use std::cell::OnceCell;
 use std::ops::Deref;
 
 use ruff_python_ast::str::Quote;
+use ruff_python_ast::StringLiteralFlags;
 use ruff_python_parser::{Token, TokenKind, Tokens};
 use ruff_source_file::{find_newline, LineEnding, LineRanges};
 use ruff_text_size::Ranged;
@@ -45,6 +46,12 @@ impl<'a> Stylist<'a> {
     }
 }
 
+impl From<&Stylist<'_>> for StringLiteralFlags {
+    fn from(stylist: &Stylist) -> Self {
+        StringLiteralFlags::empty().with_quote_style(stylist.quote())
+    }
+}
+
 fn detect_quote(tokens: &[Token]) -> Quote {
     for token in tokens {
         match token.kind() {
diff --git a/crates/ruff_python_formatter/tests/normalizer.rs b/crates/ruff_python_formatter/tests/normalizer.rs
index b83a8cee0..dd83f0612 100644
--- a/crates/ruff_python_formatter/tests/normalizer.rs
+++ b/crates/ruff_python_formatter/tests/normalizer.rs
@@ -81,7 +81,7 @@ impl Transformer for Normalizer {
                         string.value = ast::StringLiteralValue::single(ast::StringLiteral {
                             value: string.value.to_str().to_string().into_boxed_str(),
                             range: string.range,
-                            flags: StringLiteralFlags::default(),
+                            flags: StringLiteralFlags::empty(),
                         });
                     }
                 }
```

</details>

---

_Comment by @ntBre on 2025-01-24 19:47_

I think removing the `Default` implementation is a really good idea. I was wondering how we would bring this to the attention of everyone using the `Generator` in the future, so this seems like a good fix.

For the UP018 case, I've obviously misunderstood something. It does look at the f-string somehow because this is the output for an f-string with single quotes:

```shell
> ruff check --select UP018 --diff try.py
--- try.py
+++ try.py
@@ -1 +1 @@
-f'{str()}'
+f'{""}'

Would fix 1 error.
```

Now I see that `Checker::generator` calls `Checker::f_string_quote_style`, oops. I guess that's why the `DYNAMIC` code works: the generator does track the quote style to use like Micha suggested.

---

_Review requested from @MichaReiser by @ntBre on 2025-01-24 21:00_

---

_Comment by @ntBre on 2025-01-24 21:09_

Okay I rebased on main and applied Alex's patch. Unfortunately it still didn't get rid of the need for some kind of `DYNAMIC` behavior -- the tricky UP018 case still failed without it. I'll keep thinking about this over the weekend.

Also apparently I have not been running doctests locally, that's the cause of the CI failures (some of them used `StringLiteralFlags::default`).

---

_Review requested from @dhruvmanila by @ntBre on 2025-01-24 21:17_

---

_@ntBre reviewed on 2025-01-24 22:01_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-24 22:01_

This is the `quote3` case I referred to in the PR. The single quotes on `active` are preserved at the cost of escaping them.

---

_Comment by @zanieb on 2025-01-24 22:21_

Is this a bug fix (and therefore allowed to change in stable) or a behavioral change (which should be gated by preview)?

---

_Comment by @ntBre on 2025-01-24 22:49_

#7799 is labeled as a bug, but I think the specific bugs it links to have been fixed in other ways. It feels more like a behavioral change to me, but I'm interested to know what other people think.

---

_Comment by @ntBre on 2025-01-24 22:54_

Actually, I think #14132 is fixed by this PR, so I guess it's a bug fix!

---

_Comment by @AlexWaygood on 2025-01-24 22:58_

I would say this is a bugfix. Ruff's autofixes shouldn't generally change the quoting style of strings if they don't need to; this PR fixes a number of cases where we _have_ been changing those details unnecessarily as a side effect of autofixes.

I think this makes the changes that ruff will make in its autofixes strictly more minimal, so I don't think it's a breaking change

---

_Comment by @AlexWaygood on 2025-01-25 19:07_

> Okay I rebased on main and applied Alex's patch. Unfortunately it still didn't get rid of the need for some kind of `DYNAMIC` behavior -- the tricky UP018 case still failed without it. I'll keep thinking about this over the weekend.

Ahh, I see. This is because the `self.quote` on the `Generator` produced by `Checker::generator()` tracks whether we're visiting a string (and tracks the quotation of that string, using this to determine what quotation the `Generator` should prefer), but the preferred quote on the `Stylist` does not take account of this.

https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff_linter/src/checkers/ast/mod.rs#L295-L302

I think what this means is that my idea of a `From<&Stylist>` implementation for `StringLiteralFlags` was a bad idea, since the `Stylist` struct doesn't keep track of where we are in the AST. Instead, we should factor out the logic in the `Checker::generator()` function so that we can use it in both `Checker::generator()` and for constructing `StringLiteralFlags` instances in autofix logic for various rules. Something like this?

```diff
diff --git a/crates/ruff_linter/src/checkers/ast/mod.rs b/crates/ruff_linter/src/checkers/ast/mod.rs
index 80a7880b2..15916adeb 100644
--- a/crates/ruff_linter/src/checkers/ast/mod.rs
+++ b/crates/ruff_linter/src/checkers/ast/mod.rs
@@ -296,11 +296,21 @@ impl<'a> Checker<'a> {
     pub(crate) fn generator(&self) -> Generator {
         Generator::new(
             self.stylist.indentation(),
-            self.f_string_quote_style().unwrap_or(self.stylist.quote()),
+            self.preferred_quote(),
             self.stylist.line_ending(),
         )
     }
 
+    /// Return the preferred quote for a generated `StringLiteral` node, given where we are in the AST.
+    pub(crate) fn preferred_quote(&self) -> Quote {
+        self.f_string_quote_style().unwrap_or(self.stylist.quote())
+    }
+
+    /// Return the default string flags a generated `StringLiteral` node should use, given where we are in the AST.
+    pub(crate) fn default_string_flags(&self) -> ast::StringLiteralFlags {
+        ast::StringLiteralFlags::empty().with_quote_style(self.preferred_quote())
+    }
+
     /// Returns the appropriate quoting for f-string by reversing the one used outside of
     /// the f-string.
     ///
```

I _think_ if we then use these methods everywhere we're currently doing things like `StringLiteralFlags::from(stylist)` or `StringLiteralFlags::empty().with_quote_style(stylist.quote())`, it should remove the need for `StringLiteralFlags::DYNAMIC`.

---

_Label `bug` added by @AlexWaygood on 2025-01-25 19:43_

---

_@AlexWaygood reviewed on 2025-01-25 20:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:86 on 2025-01-25 20:00_

I know this just comes straight from my previous patch, but it would be good to do this with a little less `unwrap`ping. Maybe something like this?

```diff
diff --git a/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs b/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
index d7c0cefda..f26288f7d 100644
--- a/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
+++ b/crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs
@@ -1,5 +1,4 @@
 use ast::FStringFlags;
-use itertools::Itertools;
 
 use crate::fix::edits::pad;
 use ruff_diagnostics::{AlwaysFixableViolation, Diagnostic, Edit, Fix};
@@ -63,27 +62,29 @@ fn is_static_length(elts: &[Expr]) -> bool {
 fn build_fstring(joiner: &str, joinees: &[Expr]) -> Option<Expr> {
     // If all elements are string constants, join them into a single string.
     if joinees.iter().all(Expr::is_string_literal_expr) {
+        let mut flags = None;
+        let mut new_value = String::new();
+
+        for (i, expr) in joinees
+            .iter()
+            .filter_map(Expr::as_string_literal_expr)
+            .enumerate()
+        {
+            new_value.push_str(expr.value.to_str());
+            if i < joinees.len() - 1 {
+                new_value.push_str(joiner);
+            }
+            if flags.is_none() {
+                flags = Some(expr.value.flags());
+            }
+        }
+
         let node = ast::StringLiteral {
-            value: joinees
-                .iter()
-                .filter_map(|expr| {
-                    if let Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) = expr {
-                        Some(value.to_str())
-                    } else {
-                        None
-                    }
-                })
-                .join(joiner)
-                .into_boxed_str(),
+            value: new_value.into_boxed_str(),
             range: TextRange::default(),
-            flags: joinees
-                .first()
-                .unwrap()
-                .as_string_literal_expr()
-                .unwrap()
-                .value
-                .flags(),
+            flags: flags.expect("Expected at least one element"),
         };
+
         return Some(node.into());
     }
```

---

_@ntBre reviewed on 2025-01-25 20:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:86 on 2025-01-25 20:11_

Makes sense to me. We could also use some question marks since the function returns an `Option`, but I think `if flags.is_none() { ... }` is closer to what I had before anyway.

I was also slightly annoyed by the `if joinees.iter().all(Expr::is_string_literal_expr) {` immediately followed by `.filter_map(Expr::as_string_literal_expr)` but didn't see a good fix before. ~~By moving it out of a closure and into a for loop we could use let-else and return early!~~ Edit: I keep forgetting there's more code in this function...

---

_@AlexWaygood reviewed on 2025-01-25 20:16_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:86 on 2025-01-25 20:16_

> I was also slightly annoyed by the `if joinees.iter().all(Expr::is_string_literal_expr) {` immediately followed by `.filter_map(Expr::as_string_literal_expr)`

Yeah, I agree that this is a bit annoying... I think it's a micro-optimisation to avoid allocating a `String` on the heap if `joinees` aren't all `ExprStringLiteral`s. But the micro-optimisation might well not be worth it!

---

_@ntBre reviewed on 2025-01-25 20:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:86 on 2025-01-25 20:24_

Do you prefer moving `new_value` out into a separate loop? This is what I had before with the flag setting mixed into the existing closure:

```rust
    if joinees.iter().all(Expr::is_string_literal_expr) {
        let mut flags = None;
        let node = ast::StringLiteral {
            value: joinees
                .iter()
                .filter_map(|expr| {
                    if let Expr::StringLiteral(ast::ExprStringLiteral { value, .. }) = expr {
                        if flags.is_none() {
                            // take the flags from the first Expr
                            flags = Some(value.flags());
                        }
                        Some(value.to_str())
                    } else {
                        None
                    }
                })
                .join(joiner)
                .into_boxed_str(),
            flags: flags.unwrap_or_default(),
            ..ast::StringLiteral::default()
        };
        return Some(node.into());
    }
```

`unwrap_or_default` could be replaced with `expect` like your code above or `?` if we want to bail out.

---

_@AlexWaygood reviewed on 2025-01-25 20:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flynt/rules/static_join_to_fstring.rs`:86 on 2025-01-25 20:26_

Ooh that diff LGTM! Happy to go with that!

---

_Comment by @ntBre on 2025-01-25 20:55_

Great call on `Checker::default_string_flags`! That did indeed allow us to delete the `DYNAMIC` flag, and I think it's a much nicer approach.

---

_Comment by @ntBre on 2025-01-25 21:18_

Should I revert the `Default` changes on `StringLiteralFlags`? I definitely want to point people toward the new `Checker` API for getting these flags. It would almost be nice if that were the only way to construct the flags, at least in rules, but I'm sure there are places where we really want either `default` or `empty`.

I tried marking `empty` `pub(crate)` to test this. 3/4 issues were reasonable to fix, but `Checker::default_string_flags` can't access it at `pub(crate)`! So I guess there's no way to enforce this, but I can at least add a note on the `empty` documentation. That's a benefit of `empty` over a derived `default` at least.

---

_Comment by @AlexWaygood on 2025-01-25 22:15_

> I tried marking `empty` `pub(crate)` to test this. 3/4 issues were reasonable to fix, but `Checker::default_string_flags` can't access it at `pub(crate)`! So I guess there's no way to enforce this, but I can at least add a note on the `empty` documentation. That's a benefit of `empty` over a derived `default` at least.

I think you could make `StringLiteralFlags::empty()` `pub(crate)` if you added a `pub` `StringLiteralFlags::new()` method, which _has_ to have a `Quote` passed into it? Something like this (relative to your branch)?

```diff
diff --git a/crates/ruff_linter/src/checkers/ast/mod.rs b/crates/ruff_linter/src/checkers/ast/mod.rs
index f82b5141d..7e0a730f8 100644
--- a/crates/ruff_linter/src/checkers/ast/mod.rs
+++ b/crates/ruff_linter/src/checkers/ast/mod.rs
@@ -310,7 +310,7 @@ impl<'a> Checker<'a> {
     /// Return the default string flags a generated `StringLiteral` node should use, given where we
     /// are in the AST.
     pub(crate) fn default_string_flags(&self) -> ast::StringLiteralFlags {
-        ast::StringLiteralFlags::empty().with_quote_style(self.preferred_quote())
+        ast::StringLiteralFlags::new(self.preferred_quote())
     }
 
     /// Returns the appropriate quoting for f-string by reversing the one used outside of
diff --git a/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs b/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
index 4a2b7ce0b..07b1f31ee 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs
@@ -147,7 +147,7 @@ mod print_arguments {
             if let FStringElement::Literal(literal) = element {
                 acc.push(StringLiteral {
                     value: literal.value.clone(),
-                    flags: StringLiteralFlags::empty().with_quote_style(quote),
+                    flags: StringLiteralFlags::new(quote),
                     range: TextRange::default(),
                 });
                 Some(acc)
@@ -206,7 +206,7 @@ mod print_arguments {
             range: TextRange::default(),
             value: StringLiteralValue::single(StringLiteral {
                 value: combined_string.into(),
-                flags: StringLiteralFlags::empty().with_quote_style(quote),
+                flags: StringLiteralFlags::new(quote),
                 range: TextRange::default(),
             }),
         }))
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index 71fd1de12..ca3b23a6b 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1424,7 +1424,11 @@ bitflags! {
 pub struct StringLiteralFlags(StringLiteralFlagsInner);
 
 impl StringLiteralFlags {
-    pub fn empty() -> Self {
+    pub fn new(quote_style: Quote) -> Self {
+        Self::empty().with_quote_style(quote_style)
+    }
+
+    pub(crate) fn empty() -> Self {
         Self(StringLiteralFlagsInner::empty())
     }
 
diff --git a/crates/ruff_python_formatter/tests/normalizer.rs b/crates/ruff_python_formatter/tests/normalizer.rs
index dd83f0612..f3027a569 100644
--- a/crates/ruff_python_formatter/tests/normalizer.rs
+++ b/crates/ruff_python_formatter/tests/normalizer.rs
@@ -8,7 +8,7 @@ use ruff_python_ast::visitor::transformer;
 use ruff_python_ast::visitor::transformer::Transformer;
 use ruff_python_ast::{
     self as ast, BytesLiteralFlags, Expr, FStringElement, FStringFlags, FStringLiteralElement,
-    FStringPart, Stmt, StringFlags, StringLiteralFlags,
+    FStringPart, Stmt, StringFlags,
 };
 use ruff_text_size::{Ranged, TextRange};
 
@@ -81,7 +81,7 @@ impl Transformer for Normalizer {
                         string.value = ast::StringLiteralValue::single(ast::StringLiteral {
                             value: string.value.to_str().to_string().into_boxed_str(),
                             range: string.range,
-                            flags: StringLiteralFlags::empty(),
+                            flags: string.value.flags(),
                         });
                     }
                 }
```

(not _100%_ sure about that formatter change -- Micha or Dhruv can advise on that one!)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs`:69 on 2025-01-26 09:54_

```suggestion
                    msg: print_arguments::to_expr(&call.arguments, checker.preferred_quote())
```

Or you could use `checker.default_string_flags()` and pass a `StringLiteralFlags` instance down into the lower functions rather than passing a `Quote` down -- either works!

---

_@AlexWaygood reviewed on 2025-01-26 09:55_

---

_@AlexWaygood reviewed on 2025-01-26 10:55_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 10:55_

I actually think this is okay. The reason why escaping is necessary here is because double quotes are used for the second string in the slice here (for `"inactive"`); it's impossible to retain the quoting style for both strings without escaping the quotes on one of them. The annotation is still valid -- it's still understood by [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=db0b8efe8e8d9fca0441ab6fc0948b5f) and [pyright](https://pyright-play.net/?strict=true&enableExperimentalFeatures=true&reportImplicitOverride=true&reportShadowedImports=true&reportUnusedCallResult=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AUNQCYnBTAAUAHgFxQBERpFlANoAdbuVEBdbgEoO1KAqggSANxJUA%2BvAQl20oA).

I agree that it would be _cleaner_ if the quotes were normalized here so that no escaping is required. You could argue that that's a job for a formatter, though, rather than a linter autofix? 

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 10:58_

hmm, but I suppose a formatter wouldn't fix the code introduced by this fix, since that would actually change the contents of the string from the formatter's perspective.

I guess in that case, it _would_ be nice if this autofix didn't introduce escaped quotes... though I would still say it doesn't necessarily have to block the PR being merged

---

_@AlexWaygood reviewed on 2025-01-26 10:58_

---

_@AlexWaygood reviewed on 2025-01-26 11:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 11:22_

cc. @Daverball in case he has any ideas for an easy fix for this :-)

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 12:26_

This looks like an artifact of how this specific fix works. First we stringize the quoted expression, taking care to expand nested forward references. Then we stringize the generated string again, treating it like a string literal in order to correctly render any nested quotes.

I think in this case it honestly makes more sense to ignore what quotes the original string literals were using, since ideally we always want to end up choosing the preferred quote for the outer string, even if that leads to an edit which changes more things. (I.e. the inner string should use either only our non-preferred quote or use both of them for nested cases, but never only our preferred quote)

I wouldn't want `Literal["Foo"]` to be turned into `'Literal["Foo"]'` if `"` was my preferred quote, but rather `"Literal['Foo']"`, similarly I wouldn't want it to be turned into `"Literal[\"Foo\"]"`.

---

_@Daverball reviewed on 2025-01-26 12:26_

---

_@AlexWaygood reviewed on 2025-01-26 14:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 14:59_

Okay great -- thanks @Daverball! Practically speaking, I think we could get there with something like this @ntBre:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs b/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
index cc976bf17..60970cbfc 100644
--- a/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
+++ b/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs
@@ -3,6 +3,7 @@ use std::cmp::Reverse;
 use ruff_diagnostics::Edit;
 use ruff_python_ast::helpers::{map_callable, map_subscript};
 use ruff_python_ast::name::QualifiedName;
+use ruff_python_ast::str::Quote;
 use ruff_python_ast::visitor::transformer::{walk_expr, Transformer};
 use ruff_python_ast::{self as ast, Decorator, Expr, StringLiteralFlags};
 use ruff_python_codegen::{Generator, Stylist};
@@ -381,6 +382,12 @@ impl<'a> QuoteAnnotator<'a> {
         if let Expr::Tuple(ast::ExprTuple { elts, .. }) = slice {
             if !elts.is_empty() {
                 self.visit_expr(&mut elts[0]);
+                // The outer annotation will use the preferred quote.
+                // As such, any quotes found in metadata elements inside an `Annotated` slice
+                // should use the opposite quote to the preferred quote.
+                for elt in elts.iter_mut().skip(1) {
+                    QuoteRewriter::new(self.stylist).visit_expr(elt);
+                }
             }
         }
     }
@@ -395,8 +402,10 @@ impl Transformer for QuoteAnnotator<'_> {
                         .semantic
                         .match_typing_qualified_name(&qualified_name, "Literal")
                     {
-                        // we don't want to modify anything inside `Literal`
-                        // so skip visiting this subscripts' slice
+                        // The outer annotation will use the preferred quote.
+                        // As such, any quotes found inside a `Literal` slice
+                        // should use the opposite quote to the preferred quote.
+                        QuoteRewriter::new(self.stylist).visit_expr(slice);
                     } else if self
                         .semantic
                         .match_typing_qualified_name(&qualified_name, "Annotated")
@@ -425,3 +434,22 @@ impl Transformer for QuoteAnnotator<'_> {
         }
     }
 }
+
+#[derive(Debug)]
+struct QuoteRewriter {
+    preferred_inner_quote: Quote,
+}
+
+impl QuoteRewriter {
+    fn new(stylist: &Stylist) -> Self {
+        Self {
+            preferred_inner_quote: stylist.quote().opposite(),
+        }
+    }
+}
+
+impl Transformer for QuoteRewriter {
+    fn visit_string_literal(&self, literal: &mut ast::StringLiteral) {
+        literal.flags = literal.flags.with_quote_style(self.preferred_inner_quote);
+    }
+}
```

---

_@ntBre reviewed on 2025-01-26 15:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 15:27_

Thanks for this feedback! I agree that using the preferred quote on the outer expression and not escaping inner quotes would be nicest. And I confirmed that the formatter will not fix this currently.

It's not immediately clear to me how to fix this, but I will at least track it as a follow up. I'm really stuck on this `DYNAMIC` flag idea, but barring that, I think it will require changes in [`QuoteAnnotator::into_annotation`](https://github.com/astral-sh/ruff/blob/brent%2Fquote-style/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L359) or at least somewhere between there and [`quote_annotation`](https://github.com/astral-sh/ruff/blob/brent%2Fquote-style/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L247).

---

_@ntBre reviewed on 2025-01-26 15:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 15:28_

Oops, should have refreshed before commenting! I'll try Alex's fix!

---

_@ntBre reviewed on 2025-01-26 16:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quote_typing-only-third-party-import_quote3.py.snap`:26 on 2025-01-26 16:02_

The patch worked perfectly! I was able to revert the quoting snapshot changes too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/assert_with_print_message.rs`:69 on 2025-01-26 16:15_

Ooh good catch. I went for the `StringLiteralFlags` approach, but I'm happy with either one too.

---

_@ntBre reviewed on 2025-01-26 16:15_

---

_Comment by @ntBre on 2025-01-26 17:02_

Thanks for the patches! They should all be applied now.

> I think you could make `StringLiteralFlags::empty()` `pub(crate)` if you added a `pub` `StringLiteralFlags::new()` method, which _has_ to have a `Quote` passed into it? Something like this (relative to your branch)?

I think this is a little better, but I'm still looking for a good place to document the best way to construct these for rule authors. `Quote`s are also `pub` and easy to construct, so you can still do `StringLiteralFlags::new(Quote::Double)`, for example. I tried adding this locally on `StringLiteralFlags::new`, but the links are broken, again because of crate boundaries/privacy. I can just remove the square brackets, or leave this out if it's not a big deal. Removing `..default()` and having to think about the `Quote` may already be plenty of friction anyway.

```rust
    /// Construct a new [`StringLiteralFlags`] with `quote_style`.
    ///
    /// If you're using a [`Generator`] to generate a fix from an existing string literal, consider
    /// passing along the [`StringLiteral.flags`] field or the result of the
    /// [`StringLiteralValue::flags`]` method. If you don't have an existing string but have a
    /// [`Checker`] available, consider using [`Checker::default_string_flags`], which will properly
    /// handle surrounding f-strings.
    pub fn new(quote_style: Quote) -> Self {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:312 on 2025-01-26 17:07_

It looks like this is now only actually used within this module; I think we can make it private!

```suggestion
    fn default_string_flags(&self) -> ast::StringLiteralFlags {
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-26 17:10_

I'd still love somebody more familiar with the formatter than me to OK this change -- I know Micha is out this week, so maybe @dhruvmanila?

---

_@AlexWaygood approved on 2025-01-26 17:11_

Brilliant work! This is a big improvement to our autofix infrastructure

---

_Comment by @AlexWaygood on 2025-01-26 17:12_

> I think this is a little better, but I'm still looking for a good place to document the best way to construct these for rule authors. `Quote`s are also `pub` and easy to construct, so you can still do `StringLiteralFlags::new(Quote::Double)`, for example. I tried adding this locally on `StringLiteralFlags::new`, but the links are broken, again because of crate boundaries/privacy. I can just remove the square brackets, or leave this out if it's not a big deal.

I think more internal documentation is almost always a good thing -- I'd add the docs, but just leave out the square brackets where necessary!

---

_@ntBre reviewed on 2025-01-26 17:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:312 on 2025-01-26 17:21_

This is still used by a bunch of rules on my branch. Did I miss a patch somewhere?

---

_Comment by @ntBre on 2025-01-26 17:23_

Thank you again! Sounds good, I'll add the docs and then hold off until we can double check the formatter changes.

---

_@AlexWaygood reviewed on 2025-01-26 17:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:312 on 2025-01-26 17:27_

Oops, sorry, I meant to suggest making `Checker::preferred_quote()` private, not `Checker::default_string_flags()`! _That_ one is now only used within this module 😄

---

_@ntBre reviewed on 2025-01-26 17:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:312 on 2025-01-26 17:30_

Oh of course, thanks for clarifying!

---

_Comment by @AlexWaygood on 2025-01-26 17:32_

Oh, it might be good to adjust the docs for this field as well: https://github.com/astral-sh/ruff/blob/cb3361e682fc52d7ac9fdc5e2558373eb1d94a1f/crates/ruff_python_codegen/src/generator.rs#L68-L69

We need to:
- Clarify that it now only impacts f-strings and bytestrings, not normal string literals
- Say that to control the quoting style of a `StringLiteral` you need to modify the `StringLiteralFlags` on the node before you pass it to the `Generator`

---

_@AlexWaygood reviewed on 2025-01-26 17:37_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1774 on 2025-01-26 17:37_

I suppose we _could_ keep these tests, but use bytestrings (or f-strings) rather than regular strings? Though obviously we'll hopefully be making the same change for bytestrings and f-strings too fairly soon!

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1774 on 2025-01-26 17:58_

Yeah, I felt pretty bad deleting a test. On the other hand, the function being tested here (`round_trip_with`) is defined in this `tests` module, and the only remaining uses are with `Quote::default`, so the test did seem outdated to me. I'm happy to restore it with another string type if you prefer, though, or update `round_trip_with` not to take a `Quote` argument. That seems like another way to clarify things.

---

_@ntBre reviewed on 2025-01-26 17:58_

---

_@AlexWaygood reviewed on 2025-01-26 18:04_

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1774 on 2025-01-26 18:04_

> the function being tested here (`round_trip_with`) is defined in this `tests` module, and the only remaining uses are with `Quote::default`, so the test did seem outdated to me.

Right. But I think these tests are essentially a unit test for the `quote` field, to check that modifying the `quote` the struct is initialized with modifies the quotes that are used by the generated code (hence the test function's name `set_quote`). So if we delete these tests, there are no longer any dedicated unit tests for the `quote` field, even though that field _is_ still used to decide what quotes the generator should use for bytestrings and f-strings.

Again, it's not a _huge_ deal since hopefully we'll get to bytestrings and f-strings pretty soon in followup PRs! But I would _weakly_ prefer to modify the tests slightly here rather than deleting them, until we're able to get rid of the `Generator::quote` field entirely.

---

_@ntBre reviewed on 2025-01-26 18:18_

---

_Review comment by @ntBre on `crates/ruff_python_codegen/src/generator.rs`:1774 on 2025-01-26 18:18_

That makes sense. I restored the test and extended it to all three string types. Hopefully the macro isn't too gratuitous. I thought it clarified the purpose of the test and made it more compact.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:440 on 2025-01-26 18:26_

```suggestion
/// A [`Transformer`] class that rewrites all strings in an expression
/// to use a specified quotation style
#[derive(Debug)]
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:169 on 2025-01-26 18:32_

you could do a driveby simplification here, if you like (since we're here!):

```diff
diff --git a/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs b/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
index 6d785760b..ba16f7fd2 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs
@@ -158,16 +158,11 @@ fn generate_keyword_fix(checker: &Checker, call: &ast::ExprCall) -> Fix {
     Fix::unsafe_edit(add_argument(
         &format!(
             "encoding={}",
-            checker
-                .generator()
-                .expr(&Expr::StringLiteral(ast::ExprStringLiteral {
-                    value: ast::StringLiteralValue::single(ast::StringLiteral {
-                        value: "utf-8".to_string().into_boxed_str(),
-                        flags: checker.default_string_flags(),
-                        range: TextRange::default(),
-                    }),
-                    range: TextRange::default(),
-                }))
+            checker.generator().expr(&Expr::from(ast::StringLiteral {
+                value: Box::from("utf-8"),
+                flags: checker.default_string_flags(),
+                range: TextRange::default(),
+            }))
         ),
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1437 on 2025-01-26 18:58_

The way I find this documentation most useful is when hovering over a struct or function in VSCode when I'm in another module (it displays a rendered version for me). E.g. here's the tooltip I get when I hover over `ast::StringLiteral` in the module for a linter rule:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/9cfc7a68-468d-4a55-b3b3-e84d4fe22ac8)

</details>

But I think I'm much more likely to hover over `StringLiteralFlags` in another module than `StringLiteralFlags::new()` if I want information on how to construct instances of the struct, especially if I'm not familiar with the struct and/or don't know about the `new()` method. The manual links you've included here also just give me a 404 if I click on them from inside VSCode :-(

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/72be58d7-f60e-40b7-ac55-c58e89d6ab9b)

</details>

So I think I'd prefer to move the docs to the struct itself, and probably skip the manual links. Something like this?

```diff
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index 8de1a0098..e2aba34bd 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1420,25 +1420,29 @@ bitflags! {
 
 /// Flags that can be queried to obtain information
 /// regarding the prefixes and quotes used for a string literal.
+///
+/// ## Notes on usage
+///
+/// If you're using a `Generator` from the `ruff_python_codegen` crate to generate a lint-rule
+/// fix from an existing string literal, consider passing along the [`StringLiteral::flags`]
+/// field or the result of the [`StringLiteralValue::flags`] method. If you don't have an
+/// existing string but have a `Checker` from the `ruff_linter` crate available, consider
+/// using `Checker::default_string_flags` to create instances of this struct; this method will
+/// properly handle surrounding f-strings. For usage that doesn't fit into one of these categories,
+/// the public constructor [`StringLiteralFlags::new`] can be used.
 #[derive(Copy, Clone, Eq, PartialEq, Hash)]
 pub struct StringLiteralFlags(StringLiteralFlagsInner);
 
 impl StringLiteralFlags {
     /// Construct a new [`StringLiteralFlags`] with `quote_style`.
     ///
-    /// If you're using a [`Generator`](../ruff_python_codegen/struct.Generator.html) to generate a
-    /// fix from an existing string literal, consider passing along the [`StringLiteral::flags`]
-    /// field or the result of the [`StringLiteralValue::flags`] method. If you don't have an
-    /// existing string but have a [`Checker`](../ruff_linter/checkers/ast/struct.Checker.html)
-    /// available, consider using [`Checker::default_string_flags`], which will properly handle
-    /// surrounding f-strings.
-    ///
-    /// [`Checker::default_string_flags`]:
-    /// ../ruff_linter/checkers/ast/struct.Checker.html#method.default_string_flags
+    /// See the documentation for [`StringLiteralFlags`] for caveats on this constructor,
+    /// and situations in which alternative ways to construct this struct should be used.
     pub fn new(quote_style: Quote) -> Self {
         Self::empty().with_quote_style(quote_style)
     }
 
+    /// Private-to-the-crate method for creating a new [`StringLiteralFlags`].
     pub(crate) fn empty() -> Self {
         Self(StringLiteralFlagsInner::empty())
     }
```


---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-26 18:59_

I guess this could now be

```suggestion
        let new = StringLiteralFlags::new(value.quote_style())
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_codegen/src/generator.rs`:1774 on 2025-01-26 19:00_

Nice, the macro seems fine to me!

---

_@AlexWaygood approved on 2025-01-26 19:01_

---

_@ntBre reviewed on 2025-01-26 20:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:169 on 2025-01-26 20:17_

Oh nice. I actually looked at this code too and almost opened a separate PR just changing it to `"encoding=\"utf-8\""`, but it turned out to be important to use the generator for weird cases where the `open` is nested in an f-string, for example. Something like `f"{[line for line in open('foo.txt')]}"` is what I came up with to break my "fix."

There are some `local`/`locale` typos in the docs that we can get while we're here too!

---

_@ntBre reviewed on 2025-01-26 20:20_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1437 on 2025-01-26 20:20_

Sounds good! I meant to mention that I was happy to delete the manual links. It worked well for the docs I opened in the browser, but I'm not surprised that they aren't very robust. It also makes sense to put the docs on the struct itself!

---

_@AlexWaygood reviewed on 2025-01-27 12:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-27 12:00_

...or we could consider getting rid of the `StringLiteralFlags::empty()` method altogether, and say that `StringLiteralFlags::new()` is the _only_ way of constructing the struct!

```diff
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index 8de1a0098..b272597b9 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -1436,11 +1436,7 @@ impl StringLiteralFlags {
     /// [`Checker::default_string_flags`]:
     /// ../ruff_linter/checkers/ast/struct.Checker.html#method.default_string_flags
     pub fn new(quote_style: Quote) -> Self {
-        Self::empty().with_quote_style(quote_style)
-    }
-
-    pub(crate) fn empty() -> Self {
-        Self(StringLiteralFlagsInner::empty())
+        Self(StringLiteralFlagsInner::empty()).with_quote_style(quote_style)
     }
 
     #[must_use]
@@ -1571,7 +1567,7 @@ impl StringLiteral {
         Self {
             range,
             value: "".into(),
-            flags: StringLiteralFlags::empty().with_invalid(),
+            flags: StringLiteralFlags::new(Quote::default()).with_invalid(),
         }
     }
 }
@@ -2140,9 +2136,7 @@ impl From<AnyStringFlags> for StringLiteralFlags {
                 value.prefix()
             )
         };
-        let new = StringLiteralFlags::empty()
-            .with_quote_style(value.quote_style())
-            .with_prefix(prefix);
+        let new = StringLiteralFlags::new(value.quote_style()).with_prefix(prefix);
         if value.is_triple_quoted() {
             new.with_triple_quotes()
         } else {
```

---

_@AlexWaygood reviewed on 2025-01-27 12:01_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1437 on 2025-01-27 12:01_

One other thought -- it might also be good to add to the doc-comment for `StringLiteralFlags::new()` that the instance returned will not have the triple-quoting flag set, nor any of the prefix flags.

---

_@ntBre reviewed on 2025-01-27 13:25_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1437 on 2025-01-27 13:25_

Oh that reminds me, I think one of the test cases loses its triple quotes. Actually the first SIM905 test case:

```text
SIM905.py:6:1: SIM905 [*] Consider using a list literal instead of `str.split`
   |
 5 |   # positives
 6 | / """
 7 | |     itemA
 8 | |     itemB
 9 | |     itemC
10 | | """.split()
   | |___________^ SIM905
11 |
12 |   "a,b,c,d".split(",")
   |
   = help: Replace with list literal

ℹ Safe fix
3  3  | no_sep = None
4  4  | 
5  5  | # positives
6     |-"""
7     |-	itemA
8     |-	itemB
9     |-	itemC
10    |-""".split()
   6  |+["itemA", "itemB", "itemC"]
11 7  | 
12 8  | "a,b,c,d".split(",")
13 9  | "a,b,c,d".split(None)
```

I guess calling `flags.quote_style()` in the generator means we only get single or double quote information, not all of the flags. This might be going beyond the scope of this PR, but is that something we want to preserve too? It would look pretty bad in this example as `["""itemA""", ...]` but I'm sure other cases would make more sense.

---

_@AlexWaygood reviewed on 2025-01-27 13:30_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1437 on 2025-01-27 13:30_

Ah, interesting question! I think in general, probably yes, but for that specific autofix, probably no? So it's probably best to defer that question to a followup PR so we can examine the impact changing this has on each rule's autofix (and see what the ecosystem report is).

---

_@AlexWaygood reviewed on 2025-01-27 13:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM905_SIM905.py.snap`:572 on 2025-01-27 13:32_

micro-nit: could you update the comment in the fixture (which will also result in this snapshot being updated) to reflect that we now retain the `u` prefix? Similar for a number of other comments in this file on lines where the suggested fix is now being changed

```suggestion
   58 |+[u"a", u"b"]  # [u"a", u"b"]
```

---

_@dhruvmanila reviewed on 2025-01-27 13:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1440 on 2025-01-27 13:39_

I think it might be confusing to see that one can construct a `StringLiteralFlags` only by supplying the quote via the `new` function and not any other parameters like prefix. I'd either (a) inline the logic `::empty()::with_quote_style(...)` everywhere this is being used or (b) implement `From<Quote>`

---

_@dhruvmanila reviewed on 2025-01-27 13:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1440 on 2025-01-27 13:40_

I see that this is only being used in `Checker::default_string_flags` so my preference would be to just inline the logic (a)

---

_@AlexWaygood reviewed on 2025-01-27 13:51_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1440 on 2025-01-27 13:51_

I think our worry was that contributors writing an autofix for the linter might not know about the `Checker::default_string_flags()` method (which should be the preferred way to create an instance of `StringLiteralFlags` in many autofixes following this PR). So they might use `StringLiteralFlags::empty()` to create a new instance instead, and that could easily slip through the net in PR review.

I do see your point, though. Maybe it'll be okay if we just put a big warning in the doc-comment above `StringLiteralFlags::empty()`?

---

_@dhruvmanila reviewed on 2025-01-27 13:51_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-27 13:51_

tl;dr, I don't think this matters but I'd prefer to keep using the default flags.

This normalizer is mainly used to normalize the formatted and unformatted AST to assert that both the ASTs are same (formatter shouldn't change the AST). This assertion happens here: https://github.com/astral-sh/ruff/blob/648ad8a20de2bc709470ffe908e42687d87dedc7/crates/ruff_python_formatter/tests/fixtures.rs#L384-L422

Why I think this doesn't matter is because after the normalization, we compare the AST using the comparable logic which only compares the string by its content and not any of the flags:

https://github.com/astral-sh/ruff/blob/648ad8a20de2bc709470ffe908e42687d87dedc7/crates/ruff_python_ast/src/comparable.rs#L694-L697

---

_@AlexWaygood reviewed on 2025-01-27 13:59_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-27 13:59_

Cool. So this + our other conversation at https://github.com/astral-sh/ruff/pull/15726#discussion_r1930545195 suggests to me that we should:
- Get rid of `StringLiteralFlags::new()`
- Promote `StringLiteralFlags::empty()` to `pub` rather than `pub(crate)` (but add a doc-comment that warns that you probably don't really ever want to use `StringLiteralFlags::empty()` from the linter crate)
- Use `StringLiteralFlags::empty()` at this location in the formatter

---

_@ntBre reviewed on 2025-01-27 13:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/snapshots/ruff_linter__rules__flake8_simplify__tests__SIM905_SIM905.py.snap`:572 on 2025-01-27 13:59_

Oh good catch. I think I caught them all now!

---

_@AlexWaygood reviewed on 2025-01-27 14:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:2148 on 2025-01-27 14:00_

(My suggestions here are superseded by my comment at https://github.com/astral-sh/ruff/pull/15726#discussion_r1930574926 😄)

---

_@dhruvmanila reviewed on 2025-01-27 14:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1440 on 2025-01-27 14:28_

I discussed this with Alex on Discord and we think that the `new` method should be removed and the logic be inlined, and update the `empty` documentation to include a note about it's usage in the linter.

---

_@ntBre reviewed on 2025-01-27 14:57_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/nodes.rs`:1440 on 2025-01-27 14:57_

Sounds good! Thanks for the suggestion and additional context!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:438 on 2025-01-27 15:16_

Ugh, this is on me -- it's Rust, not Python!

```suggestion
/// A [`Transformer`] struct that rewrites all strings in an expression
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1432 on 2025-01-27 15:17_

```suggestion
/// public constructor [`StringLiteralFlags::empty`] can be used.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-27 15:18_

(per the conversation at https://github.com/astral-sh/ruff/pull/15726#discussion_r1929823038 )

```suggestion
                            flags: StringLiteralFlags::empty(),
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lib.rs`:203 on 2025-01-27 15:18_

```suggestion
/// use ruff_python_ast::{StringLiteral, StringLiteralFlags};
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lib.rs`:208 on 2025-01-27 15:19_

(since `Quote::DOUBLE` is the default)

```suggestion
///     flags: StringLiteralFlags::empty(),
```

---

_@AlexWaygood approved on 2025-01-27 15:19_

---

_@ntBre reviewed on 2025-01-27 15:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:438 on 2025-01-27 15:24_

Wow I didn't notice either!

---

_@ntBre reviewed on 2025-01-27 15:32_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-27 15:32_

Sorry, I got distracted by the rest of the thread.

---

_@AlexWaygood reviewed on 2025-01-27 15:33_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/tests/normalizer.rs`:84 on 2025-01-27 15:33_

no worries!!

---

_@ntBre reviewed on 2025-01-27 15:37_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:208 on 2025-01-27 15:37_

Hmm, I think `Quote::Double` is the default for the `Quote` enum, but `StringLiteralFlags::empty` will default to single quotes since the `DOUBLE` flag is not set. Is that right?

I'm happy with the proposed changes here, and I don't think it has a big effect on the example code, just clarifying if we want single or double quotes.

---

_@AlexWaygood reviewed on 2025-01-27 15:45_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lib.rs`:208 on 2025-01-27 15:45_

Oh, great catch! Even more reason to get rid of the `StringLiteralFlags::default()` method, since I think we should usually prefer double quotes, but it looks like `StringLiteralFlags::default()` would have created a string with single quotes 😆

I don't think it matters much in this doctest whether single quotes or double quotes are used, though, so let's just use `::empty()`, which is simpler (and matches the behaviour on `main`)

---

_@AlexWaygood approved on 2025-01-27 15:58_

---

_Merged by @ntBre on 2025-01-27 18:41_

---

_Closed by @ntBre on 2025-01-27 18:41_

---

_Branch deleted on 2025-01-27 18:41_

---
