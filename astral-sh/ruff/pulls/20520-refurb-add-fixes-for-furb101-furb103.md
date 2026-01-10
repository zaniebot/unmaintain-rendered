```yaml
number: 20520
title: "[`refurb`] Add fixes for `FURB101`, `FURB103`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb101-fix
created_at: 2025-09-22T20:07:08Z
updated_at: 2025-10-06T22:51:12Z
url: https://github.com/astral-sh/ruff/pull/20520
synced_at: 2026-01-10T17:34:34Z
```

# [`refurb`] Add fixes for `FURB101`, `FURB103`

---

_Pull request opened by @chirizxc on 2025-09-22 20:07_

## Summary

Part of `PTH-*` fixes: https://github.com/astral-sh/ruff/pull/19404#issuecomment-3089639686

## Test Plan
`cargo nextest run furb`


---

_Comment by @github-actions[bot] on 2025-09-22 20:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +840 -0 fixes in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/disnake/__main__.py#L293'>disnake/__main__.py:293:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(str(new_directory / "config.py"))....`
- <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/disnake/__main__.py#L293'>disnake/__main__.py:293:14:</a> FURB103 `open` and `write` should be replaced by `Path(str(new_directory / "config.py"))....`
+ <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/disnake/__main__.py#L312'>disnake/__main__.py:312:18:</a> FURB103 [*] `open` and `write` should be replaced by `Path(str(new_directory / ".gitignore")).write_text(_gitignore_template, encoding="utf-8")`
- <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/disnake/__main__.py#L312'>disnake/__main__.py:312:18:</a> FURB103 `open` and `write` should be replaced by `Path(str(new_directory / ".gitignore")).write_text(_gitignore_template, encoding="utf-8")`
+ <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/docs/conf.py#L112'>docs/conf.py:112:6:</a> FURB101 [*] `open` and `read` should be replaced by `Path("../disnake/__init__.py").read_text()`
- <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/docs/conf.py#L112'>docs/conf.py:112:6:</a> FURB101 `open` and `read` should be replaced by `Path("../disnake/__init__.py").read_text()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/scripts/codemods/base.py#L51'>scripts/codemods/base.py:51:18:</a> FURB101 [*] `open` and `read` should be replaced by `Path(self.context.filename).read_text(encoding="utf-8")`
- <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/scripts/codemods/base.py#L51'>scripts/codemods/base.py:51:18:</a> FURB101 `open` and `read` should be replaced by `Path(self.context.filename).read_text(encoding="utf-8")`
+ <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/setup.py#L8'>setup.py:8:6:</a> FURB101 [*] `open` and `read` should be replaced by `Path("disnake/__init__.py").read_text(encoding="utf-8")`
- <a href='https://github.com/DisnakeDev/disnake/blob/541f0263cf9651d4369f80ac6ddc494c8d6267d1/setup.py#L8'>setup.py:8:6:</a> FURB101 `open` and `read` should be replaced by `Path("disnake/__init__.py").read_text(encoding="utf-8")`
... 1 additional changes omitted for rule FURB101
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +208 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/cli/commands/pool_command.py#L117'>airflow-core/src/airflow/cli/commands/pool_command.py:117:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(filepath).read_text()`
- <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/cli/commands/pool_command.py#L117'>airflow-core/src/airflow/cli/commands/pool_command.py:117:10:</a> FURB101 `open` and `read` should be replaced by `Path(filepath).read_text()`
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/cli/commands/pool_command.py#L147'>airflow-core/src/airflow/cli/commands/pool_command.py:147:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filepath)....`
- <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/cli/commands/pool_command.py#L147'>airflow-core/src/airflow/cli/commands/pool_command.py:147:10:</a> FURB103 `open` and `write` should be replaced by `Path(filepath)....`
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/example_dags/example_kubernetes_executor.py#L90'>airflow-core/src/airflow/example_dags/example_kubernetes_executor.py:90:18:</a> FURB103 [*] `open` and `write` should be replaced by `Path("/foo/volume_mount_test.txt").write_text("Hello")`
- <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/example_dags/example_kubernetes_executor.py#L90'>airflow-core/src/airflow/example_dags/example_kubernetes_executor.py:90:18:</a> FURB103 `open` and `write` should be replaced by `Path("/foo/volume_mount_test.txt").write_text("Hello")`
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/secrets/local_filesystem.py#L119'>airflow-core/src/airflow/secrets/local_filesystem.py:119:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(file_path).read_text()`
- <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/secrets/local_filesystem.py#L119'>airflow-core/src/airflow/secrets/local_filesystem.py:119:10:</a> FURB101 `open` and `read` should be replaced by `Path(file_path).read_text()`
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/secrets/local_filesystem.py#L142'>airflow-core/src/airflow/secrets/local_filesystem.py:142:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(file_path).read_text()`
- <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/secrets/local_filesystem.py#L142'>airflow-core/src/airflow/secrets/local_filesystem.py:142:10:</a> FURB101 `open` and `read` should be replaced by `Path(file_path).read_text()`
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/src/airflow/secrets/local_filesystem.py#L68'>airflow-core/src/airflow/secrets/local_filesystem.py:68:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(file_path).read_text()`
... 72 additional changes omitted for rule FURB101
+ <a href='https://github.com/apache/airflow/blob/d448d08566cf0f91d7e7e7aba6ad5fd9dd554ea2/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L118'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:118:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(control_file).write_text("pause")`
... 196 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +28 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L155'>superset/cli/importexport.py:155:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(path).read_text()`
- <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L155'>superset/cli/importexport.py:155:14:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text()`
+ <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L192'>superset/cli/importexport.py:192:18:</a> FURB101 [*] `open` and `read` should be replaced by `Path(path).read_text()`
- <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L192'>superset/cli/importexport.py:192:18:</a> FURB101 `open` and `read` should be replaced by `Path(path).read_text()`
+ <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L231'>superset/cli/importexport.py:231:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(dashboard_file).write_text(data)`
- <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L231'>superset/cli/importexport.py:231:14:</a> FURB103 `open` and `write` should be replaced by `Path(dashboard_file).write_text(data)`
+ <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L325'>superset/cli/importexport.py:325:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(path_).read_text()`
- <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/cli/importexport.py#L325'>superset/cli/importexport.py:325:14:</a> FURB101 `open` and `read` should be replaced by `Path(path_).read_text()`
... 11 additional changes omitted for rule FURB101
+ <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/utils/core.py#L1482'>superset/utils/core.py:1482:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(path).write_text(certificate)`
- <a href='https://github.com/apache/superset/blob/72464afb2e7a5af8511a2b319be9dc4cd1fccd7f/superset/utils/core.py#L1482'>superset/utils/core.py:1482:14:</a> FURB103 `open` and `write` should be replaced by `Path(path).write_text(certificate)`
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +110 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename)....`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Button widgets", doc))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Button widgets", doc))`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Calendar 2014", doc))`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> FURB103 `open` and `write` should be replaced by `Path(filename).write_text(file_html(title="Calendar 2014", doc))`
... 73 additional changes omitted for rule FURB103
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:6:</a> FURB101 [*] `open` and `read` should be replaced by `Path(join(dirname(__file__), "razzies-clean.csv")).read_text()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:6:</a> FURB101 `open` and `read` should be replaced by `Path(join(dirname(__file__), "razzies-clean.csv")).read_text()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L315'>scripts/milestone.py:315:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(CHANGELOG).read_text()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/milestone.py#L315'>scripts/milestone.py:315:10:</a> FURB101 `open` and `read` should be replaced by `Path(CHANGELOG).read_text()`
... 100 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/.github/workflows/algolia/upload-algolia-api.py#L165'>.github/workflows/algolia/upload-algolia-api.py:165:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(qmd).read_text()`
- <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/.github/workflows/algolia/upload-algolia-api.py#L165'>.github/workflows/algolia/upload-algolia-api.py:165:14:</a> FURB101 `open` and `read` should be replaced by `Path(qmd).read_text()`
+ <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/docs/posts/pydata-performance-part2/datafusion_native.py#L5'>docs/posts/pydata-performance-part2/datafusion_native.py:5:6:</a> FURB101 [*] `open` and `read` should be replaced by `Path("./datafusion_native.sql").read_text()`
- <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/docs/posts/pydata-performance-part2/datafusion_native.py#L5'>docs/posts/pydata-performance-part2/datafusion_native.py:5:6:</a> FURB101 `open` and `read` should be replaced by `Path("./datafusion_native.sql").read_text()`
+ <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/ibis/backends/duckdb/tests/test_decompile_tpch.py#L78'>ibis/backends/duckdb/tests/test_decompile_tpch.py:78:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(tpch_query_file).read_text()`
- <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/ibis/backends/duckdb/tests/test_decompile_tpch.py#L78'>ibis/backends/duckdb/tests/test_decompile_tpch.py:78:10:</a> FURB101 `open` and `read` should be replaced by `Path(tpch_query_file).read_text()`
... 1 additional changes omitted for rule FURB101
+ <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/ibis/expr/visualize.py#L182'>ibis/expr/visualize.py:182:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(path).write_bytes(piped_source)`
- <a href='https://github.com/ibis-project/ibis/blob/f755c02fc70b229955086fca8adb417dd96457ff/ibis/expr/visualize.py#L182'>ibis/expr/visualize.py:182:14:</a> FURB103 `open` and `write` should be replaced by `Path(path).write_bytes(piped_source)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path("/etc/hostname").read_text()`
- <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> FURB101 `open` and `read` should be replaced by `Path("/etc/hostname").read_text()`
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/services/get_executions.py#L366'>src/latch_cli/services/get_executions.py:366:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(log_file)....`
- <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/services/get_executions.py#L366'>src/latch_cli/services/get_executions.py:366:14:</a> FURB103 `open` and `write` should be replaced by `Path(log_file)....`
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/services/init/init.py#L42'>src/latch_cli/services/init/init.py:42:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(version_f).write_text("0.0.0")`
- <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/services/init/init.py#L42'>src/latch_cli/services/init/init.py:42:10:</a> FURB103 `open` and `write` should be replaced by `Path(version_f).write_text("0.0.0")`
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/utils/__init__.py#L336'>src/latch_cli/utils/__init__.py:336:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(cache_location)....`
- <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/utils/__init__.py#L336'>src/latch_cli/utils/__init__.py:336:10:</a> FURB103 `open` and `write` should be replaced by `Path(cache_location)....`
... 7 additional changes omitted for rule FURB103
+ <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/utils/__init__.py#L345'>src/latch_cli/utils/__init__.py:345:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(cache_location).read_text()`
- <a href='https://github.com/latchbio/latch/blob/593bb5d84736212a7f1e9d26391e5530ea161887/src/latch_cli/utils/__init__.py#L345'>src/latch_cli/utils/__init__.py:345:14:</a> FURB101 `open` and `read` should be replaced by `Path(cache_location).read_text()`
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+0 -0 violations, +64 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/__init__.py#L2128'>pkg_resources/__init__.py:2128:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(file_path).read_bytes()`
- <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/__init__.py#L2128'>pkg_resources/__init__.py:2128:14:</a> FURB101 `open` and `read` should be replaced by `Path(file_path).read_bytes()`
+ <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/__init__.py#L2203'>pkg_resources/__init__.py:2203:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(self.path).read_text(encoding='utf-8', errors="replace")`
- <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/__init__.py#L2203'>pkg_resources/__init__.py:2203:14:</a> FURB101 `open` and `read` should be replaced by `Path(self.path).read_text(encoding='utf-8', errors="replace")`
+ <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/tests/test_pkg_resources.py#L188'>pkg_resources/tests/test_pkg_resources.py:188:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(metadata_path).write_bytes(metadata)`
- <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/pkg_resources/tests/test_pkg_resources.py#L188'>pkg_resources/tests/test_pkg_resources.py:188:10:</a> FURB103 `open` and `write` should be replaced by `Path(metadata_path).write_bytes(metadata)`
+ <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/setuptools/archive_util.py#L133'>setuptools/archive_util.py:133:18:</a> FURB103 [*] `open` and `write` should be replaced by `Path(target).write_bytes(data)`
- <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/setuptools/archive_util.py#L133'>setuptools/archive_util.py:133:18:</a> FURB103 `open` and `write` should be replaced by `Path(target).write_bytes(data)`
+ <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/setuptools/command/bdist_egg.py#L70'>setuptools/command/bdist_egg.py:70:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(pyfile)....`
- <a href='https://github.com/pypa/setuptools/blob/9cc2f5c05c333cd4cecd2c0d9e7c5e208f2a3148/setuptools/command/bdist_egg.py#L70'>setuptools/command/bdist_egg.py:70:10:</a> FURB103 `open` and `write` should be replaced by `Path(pyfile)....`
... 21 additional changes omitted for rule FURB103
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +240 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L187'>corporate/tests/test_stripe.py:187:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(fixture_path).read_bytes()`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L187'>corporate/tests/test_stripe.py:187:14:</a> FURB101 `open` and `read` should be replaced by `Path(fixture_path).read_bytes()`
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L280'>corporate/tests/test_stripe.py:280:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(fixture_file).read_text()`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L280'>corporate/tests/test_stripe.py:280:14:</a> FURB101 `open` and `read` should be replaced by `Path(fixture_file).read_text()`
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L306'>corporate/tests/test_stripe.py:306:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(fixture_file).write_text(file_content)`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/tests/test_stripe.py#L306'>corporate/tests/test_stripe.py:306:14:</a> FURB103 `open` and `write` should be replaced by `Path(fixture_file).write_text(file_content)`
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/views/portico.py#L286'>corporate/views/portico.py:286:14:</a> FURB101 [*] `open` and `read` should be replaced by `Path(settings.CONTRIBUTOR_DATA_FILE_PATH).read_bytes()`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/corporate/views/portico.py#L286'>corporate/views/portico.py:286:14:</a> FURB101 `open` and `read` should be replaced by `Path(settings.CONTRIBUTOR_DATA_FILE_PATH).read_bytes()`
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/puppet_cache.py#L23'>scripts/lib/puppet_cache.py:23:10:</a> FURB101 [*] `open` and `read` should be replaced by `Path(PUPPET_DEPS_FILE_PATH).read_text()`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/puppet_cache.py#L23'>scripts/lib/puppet_cache.py:23:10:</a> FURB101 `open` and `read` should be replaced by `Path(PUPPET_DEPS_FILE_PATH).read_text()`
... 157 additional changes omitted for rule FURB101
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/zulip_tools.py#L538'>scripts/lib/zulip_tools.py:538:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(hash_path).write_text(new_hash)`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/zulip_tools.py#L538'>scripts/lib/zulip_tools.py:538:10:</a> FURB103 `open` and `write` should be replaced by `Path(hash_path).write_text(new_hash)`
+ <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/zulip_tools.py#L728'>scripts/lib/zulip_tools.py:728:10:</a> FURB103 [*] `open` and `write` should be replaced by `Path(path + ".tmp")....`
- <a href='https://github.com/zulip/zulip/blob/7a96ca24120cba54b402a43ea15f82b39817c95e/scripts/lib/zulip_tools.py#L728'>scripts/lib/zulip_tools.py:728:10:</a> FURB103 `open` and `write` should be replaced by `Path(path + ".tmp")....`
... 226 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB103 | 442 | 0 | 0 | 442 | 0 |
| FURB101 | 398 | 0 | 0 | 398 | 0 |

</p>
</details>




---

_Comment by @chirizxc on 2025-09-23 11:05_

@ntBre I think this PR can include furb103 as well, in essence the code will be almost identical

---

_Renamed from "[`refurb`] Add fix forread-whole-file (`FURB101`)" to "[`refurb`] Add fixes for `FURB101`, `FURB103`" by @chirizxc on 2025-09-23 11:05_

---

_Label `fixes` added by @ntBre on 2025-09-25 20:52_

---

_Label `preview` added by @ntBre on 2025-09-25 20:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:57 on 2025-09-25 20:53_

I think we might want to preserve the old `message` with the `{filename}` and `{suggestion}` placeholders because they get rendered in the docs website:

<img width="853" height="113" alt="Image" src="https://github.com/user-attachments/assets/165978de-5ffa-45a8-8f38-837c261fddb2" />

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:59 on 2025-09-25 20:54_

It's fine here, though. I don't think these are rendered anywhere.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:187 on 2025-09-25 21:03_

I don't really understand this change, it looks like it will result in the same return value. Is that true, or am I missing something?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:56 on 2025-09-25 21:05_

Same suggestion as above for the `message` placeholders.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview_FURB103.py.snap`:163 on 2025-09-25 21:10_

Nice! I was curious how we'd handle this case, but I think it makes sense only to fix the cases where we can remove the whole `with`. It might be worth adding a `## Fix availability` section pointing that out, though.

I think we probably do have all the information needed to remove parts of the `with`, but we can always expand the fix later.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:6 on 2025-09-25 21:11_

nit: I would put this back at the top, regrouping std, ruff, and crate imports.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:410 on 2025-09-25 21:16_

I might be missing something, but I thought we already constructed a replacement using the `Generator`. Can we reuse that snippet that we passed into `SourceCodeSnippet`?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:361 on 2025-09-25 21:17_

I'm not sure how helpful this helper function really is. There will be a little duplication, but I think the two `match`es on `target` justify keeping these code paths separate. And there will be a bit less duplication in any case if we can reuse the `SourceCodeSnippet` as I mention below.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/mod.rs`:68 on 2025-09-25 21:18_

nit: we might want to make the signature match `rules` above. It's not really helpful right now, but that will make it easier to reuse the function for other rules in the future, if needed.

---

_@ntBre reviewed on 2025-09-25 21:20_

Thank you! This looks great overall, I just had a few nits and a couple of slightly larger suggestions.

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:187 on 2025-09-25 21:23_

yes

---

_@chirizxc reviewed on 2025-09-25 21:23_

---

_@chirizxc reviewed on 2025-09-26 14:49_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview_FURB103.py.snap`:163 on 2025-09-26 14:49_

I don't think we should write about it, as it will break the code when trying to fix it

---

_@chirizxc reviewed on 2025-09-26 14:50_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:60 on 2025-09-26 14:50_

Should we follow the same style in these places? 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/read_whole_file.rs`:60 on 2025-10-06 20:14_

No I think these are okay because only the `message` is rendered in the docs table.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:219 on 2025-10-06 20:22_

I don't think we can use this here because it will truncate the `call` with `...` if it's too long. That's fine for the error message but not for the suggested fix, which always needs to be valid code.

I meant that we should pass the `&str` we pass to `SourceCodeSnippet::from_str` into this function as well, not reuse the `SourceCodeSnippet` itself. I think we can just return a `String` from `make_suggestion` to reuse here and only wrap it in a `SourceCodeSnippet` where that's needed.

---

_@ntBre reviewed on 2025-10-06 20:26_

Thanks! I just had one clarification about earlier suggestion, but this looks good to me otherwise.

---

_@ntBre approved on 2025-10-06 22:08_

Thank you! I went through a few of the ecosystem results, and they all look right to me too.

---

_Merged by @ntBre on 2025-10-06 22:09_

---

_Closed by @ntBre on 2025-10-06 22:09_

---

_Branch deleted on 2025-10-06 22:51_

---
