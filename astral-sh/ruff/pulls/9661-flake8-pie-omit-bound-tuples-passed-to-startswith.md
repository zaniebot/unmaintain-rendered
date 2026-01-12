```yaml
number: 9661
title: "[`flake8-pie`] Omit bound tuples passed to `.startswith` or `.endswith`"
type: pull_request
state: merged
author: mikeleppane
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix(PIE810)/wrong_suggestion_with_tuple_of_strs
created_at: 2024-01-28T12:11:55Z
updated_at: 2024-01-28T21:27:42Z
url: https://github.com/astral-sh/ruff/pull/9661
synced_at: 2026-01-12T15:55:29Z
```

# [`flake8-pie`] Omit bound tuples passed to `.startswith` or `.endswith`

---

_@mikeleppane_

## Summary

This review contains a fix for [PIE810](https://docs.astral.sh/ruff/rules/multiple-starts-ends-with/) (multiple-starts-ends-with)

The problem is that ruff suggests combining multiple startswith/endswith calls into a single call even though there might be a call with tuple of strs. This leads to calling startswith/endswith with tuple of tuple of strs which is incorrect and violates startswith/endswith conctract and results in runtime failure.

However the following will be valid and fixed correctly => 
```python
x = ("hello", "world")
y = "h"
z = "w"
msg = "hello world"

if msg.startswith(x) or msg.startswith(y) or msg.startswith(z) :
      sys.exit(1)
```
```
ruff --fix --select PIE810 --unsafe-fixes
```
=> 
```python
if msg.startswith(x) or msg.startswith((y,z)):
      sys.exit(1)
```

See: https://github.com/astral-sh/ruff/issues/8906

## Test Plan

```bash
cargo test
```


---

_Renamed from "[pie-fix]: Incorrect suggestion when calling startswith or endswith with tuple of strs" to "[flake8pie-fix]: Incorrect suggestion when calling startswith or endswith with tuple of strs" by @mikeleppane on 2024-01-28 12:13_

---

_Comment by @github-actions[bot] on 2024-01-28 12:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+17 -20 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4fa8e45c9222f05cabef543c8fd33f737826ebe3/airflow/providers/google/suite/hooks/drive.py#L197'>airflow/providers/google/suite/hooks/drive.py:197:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/4fa8e45c9222f05cabef543c8fd33f737826ebe3/airflow/providers/google/suite/hooks/drive.py#L197'>airflow/providers/google/suite/hooks/drive.py:197:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/select_server.py#L28'>examples/reference/models/select_server.py:28:5:</a> SIM108 Use ternary operator `selected_df = df[df['label'] == active_select] if active_select != 'All' else df.copy()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/select_server.py#L28'>examples/reference/models/select_server.py:28:5:</a> SIM108 Use ternary operator `selected_df = df[df['label']==active_select] if active_select!='All' else df.copy()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = "index" if route == "/" else route[1:]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = 'index' if route == '/' else route[1:]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (",", ": ") if pretty else (",", ":")` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (',', ': ') if pretty else (',', ':')` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_selection_", defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_selection_', defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_hover_", defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_hover_', defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix="node_hover_", defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix='node_hover_', defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L290'>src/bokeh/server/tornado.py:290:9:</a> SIM108 Use ternary operator `applications = {'/' : applications} if isinstance(applications, Application) else dict(applications)` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L290'>src/bokeh/server/tornado.py:290:9:</a> SIM108 Use ternary operator `applications = {'/': applications} if isinstance(applications, Application) else dict(applications)` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == "/" else key + p[0]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == '/' else key + p[0]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if "\n" not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if '\n' not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example["type"]) if example.get("type") is not None else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example['type']) if example.get('type') is not None else None` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/tests/pyfunc/test_pyfunc_schema_enforcement.py#L2138'>tests/pyfunc/test_pyfunc_schema_enforcement.py:2138:5:</a> SIM108 Use ternary operator `df = pd.DataFrame([data]) if isinstance(data, dict) and all(
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/analytics/tests/test_activity_views.py#L236'>analytics/tests/test_activity_views.py:236:17:</a> SIM108 Use ternary operator `price_per_license = 0 if tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if '.' in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/migrations/0182_set_initial_value_is_private_flag.py#L37'>zerver/migrations/0182_set_initial_value_is_private_flag.py:37:9:</a> SIM108 Use ternary operator `percent = round((processed / total) * 100, 2) if total != 0 else 100.00` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/migrations/0182_set_initial_value_is_private_flag.py#L37'>zerver/migrations/0182_set_initial_value_is_private_flag.py:37:9:</a> SIM108 Use ternary operator `percent = round(processed / total * 100, 2) if total != 0 else 100.0` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/tests/test_settings.py#L371'>zerver/tests/test_settings.py:371:9:</a> SIM108 Use ternary operator `data = {setting_name: test_value} if setting_name not in [
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 37 | 17 | 20 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+17 -20 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4fa8e45c9222f05cabef543c8fd33f737826ebe3/airflow/providers/google/suite/hooks/drive.py#L197'>airflow/providers/google/suite/hooks/drive.py:197:13:</a> SIM108 Use ternary operator `path = f"{file_info['name']}" if current_file_id == file_id else f"{file_info['name']}/{path}"` instead of `if`-`else`-block
- <a href='https://github.com/apache/airflow/blob/4fa8e45c9222f05cabef543c8fd33f737826ebe3/airflow/providers/google/suite/hooks/drive.py#L197'>airflow/providers/google/suite/hooks/drive.py:197:13:</a> SIM108 Use ternary operator `path = f'{file_info["name"]}' if current_file_id == file_id else f'{file_info["name"]}/{path}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+14 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/select_server.py#L28'>examples/reference/models/select_server.py:28:5:</a> SIM108 Use ternary operator `selected_df = df[df['label'] == active_select] if active_select != 'All' else df.copy()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/select_server.py#L28'>examples/reference/models/select_server.py:28:5:</a> SIM108 Use ternary operator `selected_df = df[df['label']==active_select] if active_select!='All' else df.copy()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = "index" if route == "/" else route[1:]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L134'>src/bokeh/command/subcommands/file_output.py:134:9:</a> SIM108 Use ternary operator `base = 'index' if route == '/' else route[1:]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (",", ": ") if pretty else (",", ":")` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/json_encoder.py#L153'>src/bokeh/core/json_encoder.py:153:5:</a> SIM108 Use ternary operator `separators = (',', ': ') if pretty else (',', ':')` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_selection_", defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L105'>src/bokeh/plotting/_graph.py:105:5:</a> SIM108 Use ternary operator `sedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_selection_', defaults=edge_visuals) if any(x.startswith('edge_selection_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix="edge_hover_", defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L110'>src/bokeh/plotting/_graph.py:110:5:</a> SIM108 Use ternary operator `hedge_visuals = pop_visuals(MultiLine, kwargs, prefix='edge_hover_', defaults=edge_visuals) if any(x.startswith('edge_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix="node_hover_", defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_graph.py#L93'>src/bokeh/plotting/_graph.py:93:5:</a> SIM108 Use ternary operator `hnode_visuals = pop_visuals(marker_type, kwargs, prefix='node_hover_', defaults=node_visuals) if any(x.startswith('node_hover_') for x in kwargs) else None` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L290'>src/bokeh/server/tornado.py:290:9:</a> SIM108 Use ternary operator `applications = {'/' : applications} if isinstance(applications, Application) else dict(applications)` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L290'>src/bokeh/server/tornado.py:290:9:</a> SIM108 Use ternary operator `applications = {'/': applications} if isinstance(applications, Application) else dict(applications)` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == "/" else key + p[0]` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L414'>src/bokeh/server/tornado.py:414:17:</a> SIM108 Use ternary operator `route = p[0] if key == '/' else key + p[0]` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if "\n" not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L253'>src/bokeh/util/compiler.py:253:13:</a> SIM108 Use ternary operator `impl = FromFile(impl if isabs(impl) else join(self.path, impl)) if '\n' not in impl and impl.endswith(exts) else TypeScript(impl)` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example["type"]) if example.get("type") is not None else None` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L217'>tests/support/util/examples.py:217:9:</a> SIM108 Use ternary operator `example_type = getattr(Flags, example['type']) if example.get('type') is not None else None` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/5d13d6ec620a02de9a5e31201bf1becdb9722ea5/tests/pyfunc/test_pyfunc_schema_enforcement.py#L2138'>tests/pyfunc/test_pyfunc_schema_enforcement.py:2138:5:</a> SIM108 Use ternary operator `df = pd.DataFrame([data]) if isinstance(data, dict) and all(
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/analytics/tests/test_activity_views.py#L236'>analytics/tests/test_activity_views.py:236:17:</a> SIM108 Use ternary operator `price_per_license = 0 if tier in (
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if '.' in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/migrations/0182_set_initial_value_is_private_flag.py#L37'>zerver/migrations/0182_set_initial_value_is_private_flag.py:37:9:</a> SIM108 Use ternary operator `percent = round((processed / total) * 100, 2) if total != 0 else 100.00` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/migrations/0182_set_initial_value_is_private_flag.py#L37'>zerver/migrations/0182_set_initial_value_is_private_flag.py:37:9:</a> SIM108 Use ternary operator `percent = round(processed / total * 100, 2) if total != 0 else 100.0` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zerver/tests/test_settings.py#L371'>zerver/tests/test_settings.py:371:9:</a> SIM108 Use ternary operator `data = {setting_name: test_value} if setting_name not in [
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 37 | 17 | 20 | 0 | 0 |

</p>
</details>




---

_Renamed from "[flake8pie-fix]: Incorrect suggestion when calling startswith or endswith with tuple of strs" to "[`flake8-pie`] Destructure tuples when passed to `.startswith` or `.endswith`" by @charliermarsh on 2024-01-28 19:18_

---

_Label `bug` added by @charliermarsh on 2024-01-28 19:18_

---

_Label `autofix` added by @charliermarsh on 2024-01-28 19:18_

---

_@charliermarsh approved on 2024-01-28 19:23_

Thanks!

---

_Renamed from "[`flake8-pie`] Destructure tuples when passed to `.startswith` or `.endswith`" to "[`flake8-pie`] Omit bound tuples passed to `.startswith` or `.endswith`" by @charliermarsh on 2024-01-28 19:24_

---

_Merged by @charliermarsh on 2024-01-28 19:29_

---

_Closed by @charliermarsh on 2024-01-28 19:29_

---

_Comment by @deliro on 2024-01-28 21:27_

Thank you!

---
