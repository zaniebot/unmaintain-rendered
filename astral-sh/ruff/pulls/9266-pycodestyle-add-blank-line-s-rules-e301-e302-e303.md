```yaml
number: 9266
title: "[`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: blank_lines_non_logical
created_at: 2023-12-24T13:33:38Z
updated_at: 2024-02-21T16:52:58Z
url: https://github.com/astral-sh/ruff/pull/9266
synced_at: 2026-01-12T15:55:28Z
```

# [`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)

---

_@hoel-bagard_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/2402, it adds the `E301`, `E302`, `E303`, `E304`, `E305`, `E306` error rules along with their fixes.

This PR is based on https://github.com/astral-sh/ruff/pull/8720, but does not use logical lines and instead works directly on tokens.

## Test Plan
The test fixture uses [the one from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testsuite/E30.py
) with a some added tests.

---

_Converted to draft by @hoel-bagard on 2023-12-24 13:49_

---

_Comment by @codspeed-hq[bot] on 2023-12-24 13:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:blank_lines_non_logical)

### Merging #9266 will **degrade performances by 4.52%**

<sub>Comparing <code>hoel-bagard:blank_lines_non_logical</code> (aea3493) with <code>main</code> (fdb5eef)</sub>



### Summary

`⚡ 8` improvements
`❌ 1` regressions
`✅ 21` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:blank_lines_non_logical)._

### Benchmarks breakdown

|     | Benchmark | `main` | `hoel-bagard:blank_lines_non_logical` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[unicode/pypinyin.py]` | 571.5 µs | 545.2 µs | +4.84% |
| ⚡ | `lexer[numpy/globals.py]` | 223.5 µs | 159.9 µs | +39.75% |
| ⚡ | `parser[numpy/globals.py]` | 1,117.5 µs | 982 µs | +13.8% |
| ⚡ | `parser[large/dataset.py]` | 69.1 ms | 63.9 ms | +8.17% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.7 ms | +11.36% |
| ❌ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 21.6 ms | 22.7 ms | -4.52% |
| ⚡ | `parser[unicode/pypinyin.py]` | 4.3 ms | 4 ms | +8.26% |
| ⚡ | `parser[pydantic/types.py]` | 26.4 ms | 24.5 ms | +7.71% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 11.8 ms | 10.8 ms | +9.47% |


---

_Comment by @github-actions[bot] on 2023-12-24 14:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3951 -26 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/data/test_classes/nlu_component_skeleton.py#L11'>data/test_classes/nlu_component_skeleton.py:11:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/scripts/evaluate_release_tag.py#L41'>scripts/evaluate_release_tag.py:41:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/aio_pika/robust_channel.pyi#L10'>stubs/aio_pika/robust_channel.pyi:10:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L23'>stubs/sanic.pyi:23:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L25'>stubs/sanic.pyi:25:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L27'>stubs/sanic.pyi:27:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L30'>stubs/sanic.pyi:30:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/socketio.pyi#L49'>stubs/socketio.pyi:49:1:</a> E302 [*] Expected 2 blank lines, found 1
... 1 additional changes omitted for rule E302
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/compat/functools.pyi#L27'>airflow/compat/functools.pyi:27:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/decorators/__init__.pyi#L160'>airflow/decorators/__init__.pyi:160:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/decorators/__init__.pyi#L318'>airflow/decorators/__init__.pyi:318:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/decorators/__init__.pyi#L63'>airflow/decorators/__init__.pyi:63:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/decorators/__init__.pyi#L723'>airflow/decorators/__init__.pyi:723:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/utils/context.pyi#L106'>airflow/utils/context.pyi:106:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/utils/context.pyi#L108'>airflow/utils/context.pyi:108:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/utils/context.pyi#L47'>airflow/utils/context.pyi:47:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/utils/context.pyi#L51'>airflow/utils/context.pyi:51:1:</a> E302 [*] Expected 2 blank lines, found 1
... 3 additional changes omitted for rule E302
+ <a href='https://github.com/apache/airflow/blob/c297f55d7aa39fda69530887f1dcfcea8a951463/airflow/utils/context.pyi#L53'>airflow/utils/context.pyi:53:5:</a> E301 [*] Expected 1 blank line, found 0
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+106 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/Provided/main.py#L3'>tests/integration/testdata/buildcmd/Provided/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/custom_root_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/custom_root_dir_and_custom_makefile/custom_src_code/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir_and_custom_makefile/custom_src_code/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/custom_root_dir_custom_makefile_and_custom_working_dir/working_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir_custom_makefile_and_custom_working_dir/working_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/custom_working_dir_src_code/working_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_working_dir_src_code/working_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/buildcmd/provided_src_code_without_makefile/main.py#L3'>tests/integration/testdata/buildcmd/provided_src_code_without_makefile/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
... 101 additional changes omitted for rule E302
- <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/start_api/lambda_authorizers/app.py#L122'>tests/integration/testdata/start_api/lambda_authorizers/app.py:122:17:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/start_api/lambda_authorizers/app.py#L123'>tests/integration/testdata/start_api/lambda_authorizers/app.py:123:15:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/start_api/lambda_authorizers/app.py#L131'>tests/integration/testdata/start_api/lambda_authorizers/app.py:131:8:</a> E221 [*] Multiple spaces before operator
- <a href='https://github.com/aws/aws-sam-cli/blob/fefee10b10563d5e1e42dbec5d01597b8a04f3fa/tests/integration/testdata/start_api/lambda_authorizers/app.py#L132'>tests/integration/testdata/start_api/lambda_authorizers/app.py:132:9:</a> E221 [*] Multiple spaces before operator
... 121 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3280 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L230'>docs/bokeh/source/conf.py:230:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L102'>examples/advanced/extensions/gears/gears.py:102:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L13'>examples/advanced/extensions/gears/gears.py:13:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L16'>examples/advanced/extensions/gears/gears.py:16:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L21'>examples/advanced/extensions/gears/gears.py:21:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L34'>examples/advanced/extensions/gears/gears.py:34:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/gears/gears.py#L62'>examples/advanced/extensions/gears/gears.py:62:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L109'>examples/advanced/extensions/parallel_plot/parallel_plot.py:109:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/putting_together.py#L85'>examples/advanced/extensions/putting_together.py:85:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/putting_together.py#L93'>examples/advanced/extensions/putting_together.py:93:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L74'>examples/advanced/extensions/tool.py:74:1:</a> E302 [*] Expected 2 blank lines, found 1
... 2442 additional changes omitted for rule E302
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/tool.py#L78'>examples/advanced/extensions/tool.py:78:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/widget.py#L52'>examples/advanced/extensions/widget.py:52:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L19'>examples/basic/annotations/colorbar_log.py:19:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
... 324 additional changes omitted for rule E305
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L25'>examples/server/app/clustering/main.py:25:1:</a> E303 [*] Too many blank lines (3)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/deploy.py#L55'>release/deploy.py:55:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L366'>src/bokeh/client/connection.py:366:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/connection.py#L366'>src/bokeh/client/connection.py:366:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L110'>src/bokeh/client/states.py:110:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L234'>src/bokeh/document/events.py:234:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/events.py#L518'>src/bokeh/document/events.py:518:9:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L114'>src/bokeh/driving.py:114:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L170'>src/bokeh/driving.py:170:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L189'>src/bokeh/driving.py:189:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/driving.py#L91'>src/bokeh/driving.py:91:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/__init__.py#L54'>src/bokeh/embed/__init__.py:54:1:</a> E303 [*] Too many blank lines (3)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L323'>src/bokeh/layouts.py:323:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
... 186 additional changes omitted for rule E306
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L559'>src/bokeh/model/model.py:559:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/model.py#L79'>src/bokeh/model/model.py:79:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L907'>src/bokeh/models/plots.py:907:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/ui/dialogs.py#L58'>src/bokeh/models/ui/dialogs.py:58:5:</a> E303 [*] Too many blank lines (2)
... 37 additional changes omitted for rule E303
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/server.py#L283'>src/bokeh/server/server.py:283:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/session.py#L231'>src/bokeh/server/session.py:231:9:</a> E301 [*] Expected 1 blank line, found 0
... 260 additional changes omitted for rule E301
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/test_server__server.py#L469'>tests/unit/bokeh/server/test_server__server.py:469:1:</a> E304 [*] Blank lines found after function decorator (1)
... 3244 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+458 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/.github/build.py#L30'>.github/build.py:30:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/.github/build.py#L9'>.github/build.py:9:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/api/__init__.py#L10'>common/api/__init__.py:10:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/conversions.py#L3'>common/conversions.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/gpio.py#L12'>common/gpio.py:12:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/gpio.py#L19'>common/gpio.py:19:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/gpio.py#L29'>common/gpio.py:29:1:</a> E302 [*] Expected 2 blank lines, found 1
... 349 additional changes omitted for rule E302
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/logging_extra.py#L216'>common/logging_extra.py:216:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/tests/test_params.py#L87'>common/tests/test_params.py:87:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/commaai/openpilot/blob/ec9f3dcef367e6c29756ededa7e83783ef280cfa/common/tests/test_params.py#L87'>common/tests/test_params.py:87:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
... 448 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/d19958c1b62937ad06b07f083fd1ca84a5970acb/molecule/testinfra/app/test_tor_hidden_services.py#L11'>molecule/testinfra/app/test_tor_hidden_services.py:11:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/freedomofpress/securedrop/blob/d19958c1b62937ad06b07f083fd1ca84a5970acb/redwood/redwood/__init__.pyi#L6'>redwood/redwood/__init__.pyi:6:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinksync/forms.py#L56'>blinksync/forms.py:56:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinksync/forms.py#L71'>blinksync/forms.py:71:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinksync/forms.py#L77'>blinksync/forms.py:77:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/fronzbot/blinkpy/blob/0ec0346f776952f7abe5b9feb53200e60e488bf6/blinksync/forms.py#L9'>blinksync/forms.py:9:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+56 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/_version_helper.py#L51'>_version_helper.py:51:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/_version_helper.py#L53'>_version_helper.py:53:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/bfloat16_example.py#L17'>examples/bfloat16_example.py:17:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/bfloat16_example.py#L28'>examples/bfloat16_example.py:28:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/bfloat16_example.py#L71'>examples/bfloat16_example.py:71:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/binary_example.py#L16'>examples/binary_example.py:16:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/example_bulkinsert_json.py#L116'>examples/example_bulkinsert_json.py:116:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/example_bulkinsert_json.py#L131'>examples/example_bulkinsert_json.py:131:1:</a> E302 [*] Expected 2 blank lines, found 1
... 40 additional changes omitted for rule E302
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/example_bulkinsert_json.py#L339'>examples/example_bulkinsert_json.py:339:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/milvus-io/pymilvus/blob/24558a34fab812cc742b7885ed3296caa2c25ca3/examples/example_bulkinsert_json.py#L378'>examples/example_bulkinsert_json.py:378:5:</a> E303 [*] Too many blank lines (2)
... 46 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (11 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E302 | 2985 | 2985 | 0 | 0 | 0 |
| E305 | 402 | 402 | 0 | 0 | 0 |
| E301 | 278 | 278 | 0 | 0 | 0 |
| E306 | 202 | 202 | 0 | 0 | 0 |
| E303 | 82 | 82 | 0 | 0 | 0 |
| E203 | 16 | 0 | 16 | 0 | 0 |
| E221 | 7 | 0 | 7 | 0 | 0 |
| PLR0917 | 2 | 1 | 1 | 0 | 0 |
| E304 | 1 | 1 | 0 | 0 | 0 |
| I001 | 1 | 0 | 1 | 0 | 0 |
| PLR6301 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @hoel-bagard on 2023-12-29 04:22_

---

_Comment by @akx on 2024-01-05 09:18_

Pinging @MichaReiser since you'd reviewed the earlier version #8720 previously 😄 

---

_Comment by @MichaReiser on 2024-01-08 06:49_

Hey @hoel-bagard 

Sorry for the delay. I've been on PTO for the last two weeks but moving this PR forward is one of my goals for this week. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:325 on 2024-01-10 07:18_

This comment is misleading. It returns `true` when the token is `class` or `def` and not `Async`

I recommend removing the comment and replace it with an explanation why testing for `Class` and `Def` is sufficient to know if we're at a top level.

---

_@MichaReiser reviewed on 2024-01-10 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:346 on 2024-01-10 07:22_

`TextSize` is commonly used to represent a byte offset or byte count, but not a character length or count. I recommend renaming the field if the unit is in bytes or changing the type to `usize` if the unit is characters.

---

_@MichaReiser reviewed on 2024-01-10 07:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:337 on 2024-01-10 07:25_

We can speed up the implementation by changing the type here (and for `last_token`) to `TokenKind`. `TokenKind` is a smaller type than `Token` that only represents the kind of the token, without its data. I did a quick check and only knowning the type seems sufficient. 

Using the `TokenKind` has the added advantage that it implements `Copy` and doesn't require allocating/deallocating (cloning)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:390 on 2024-01-10 07:32_

Nit: 
```suggestion
            if !token.is_newline() {
```

The same applies for other places where you match for a single token kind.

Alternatively. You can perform an early conversion of `token` to `TokenKind` and then use `==` comparison (or the is methods as well). 

```suggestion
            let kind = TokenKind::from_token(token);

            if token != TokenKind::Newline {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:382 on 2024-01-10 07:36_

We should change `first_token` and `last_token` to `TokenKind` to remove the need for `cloning` the tokens (will help to speed up the implementation)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:381 on 2024-01-10 07:38_

Can there be situation where `first_token` or `first_token_range` are not both either `None` or `Some`? E.g. can it happen that `first_token` is `Some` but `first_token_range` is None or the other way? If not, then I recommend changing `first_token` to `Option<(TokenKind, TextRange)>` to encode this constraint in your types.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:403 on 2024-01-10 07:49_

I think it would help readability if we can reduce the nesting here. One way to accomplish this is by using `TokenKind::is_trivia` which returns `true` for tokens that are no actual content (comments, newlines, indent, dedent etc).

That allows us to move setting `line_is_comment_only` out of the nested loop

```
if !kind.is_trivia() {
    line_is_comment_only = false;
}
```

We may want to rename `line_is_comment_only`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:432 on 2024-01-10 08:02_

I think the `first_token` == `Indent` branch is dead because we always skip over indents and dedents. 
```suggestion
                    let range = 
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:472 on 2024-01-10 08:19_

I restructured the code here by pulling out the empty line case. It allows us to use local variables for `current_blank_lines` and `current_blank_characters` which, in my view, helps readability



```suggestion
        // Number of consecutive blank lines.
        let mut current_blank_lines = 0u32;
        // Number of blank characters in the blank lines (\n vs \r\n for example).
        let mut current_blank_characters = TextSize::default();
        let mut first_token: Option<Tok> = None;
        let mut first_token_range: Option<TextRange> = None;
        let mut last_token: Option<Tok> = None;
        let mut parens = 0u32;

        while let Some((token, range)) = self.tokens.next() {
            if matches!(token, Tok::Indent | Tok::Dedent) {
                continue;
            }

            let kind = TokenKind::from_token(token);

            // At the start of the line...
            if first_token.is_none() {
                // An empty line
                if kind == TokenKind::NonLogicalNewline {
                    current_blank_lines += 1;
                    current_blank_characters += range.len();
                    continue;
                }

                if !kind.is_newline() {
                    // Ideally, we would like to have a "async def" token since we care about the "def" part.
                    // As a work around, we ignore the first token if it is "async".
                    if kind != TokenKind::Async {
                        first_token = Some(token.clone());
                    }

                    if first_token_range.is_none() {
                        first_token_range = Some(*range);
                    }
                }
            }

            if !kind.is_trivia() {
                line_is_comment_only = false;
            }

            if !kind.is_newline() && !matches!(token, Tok::String { .. } | Tok::Comment(_)) {
                is_docstring = false;
            }

            match token {
                Tok::Lbrace | Tok::Lpar | Tok::Lsqb => {
                    parens = parens.saturating_add(1);
                }
                Tok::Rbrace | Tok::Rpar | Tok::Rsqb => {
                    parens = parens.saturating_sub(1);
                }
                Tok::Newline | Tok::NonLogicalNewline if parens == 0 => {
                    let last_token_range = *range;

                    if !matches!(first_token, Some(Tok::String { .. })) {
                        is_docstring = false;
                    }

                    let first_range = first_token_range.unwrap();

                    let range = TextRange::new(
                        self.locator.line_start(first_range.start()),
                        first_range.start(),
                    );
                    let indent_level = expand_indent(self.locator.slice(range));

                    if self.previous_blank_lines < current_blank_lines {
                        self.previous_blank_lines = current_blank_lines;
                    }

                    let logical_line = LogicalLineInfo {
                        first_token: first_token.clone().unwrap(),
                        first_token_range: first_range,
                        last_token: last_token.unwrap(),
                        last_token_range,
                        is_comment_only: line_is_comment_only,
                        is_docstring,
                        indent_level,
                        blank_lines: current_blank_lines,
                        preceding_blank_lines: self.previous_blank_lines,
                        preceding_blank_characters: current_blank_characters,
                    };

                    if !line_is_comment_only {
                        self.previous_blank_lines = 0;
                    }

                    return Some(logical_line);
                }
                _ => {}
            }

            last_token = Some(token.clone());
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:89 on 2024-01-10 08:23_

Nit: 

We can make the field private. 

I think it would help readability if we named the field. I otherwise need to look at the usages to understand what the `u32` represents


```suggestion
pub struct BlankLineBetweenMethods {
    actual_blank_lines: u32
}
```

The same applies to the other violation types.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:450 on 2024-01-10 08:25_

Would you mind adding a comment explaining why `previous_blank_lines` is set to `current_blank_lines` but only when `current_blank_lines` is larger? I don't understand the reason and find it confusing, because it means that `previous` and `current_blank_lines` have the same value if the condition is true.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:408 on 2024-01-10 08:29_

It's unclear to me how this check ensures that we allow comments to follow a docstring.

I would expect the check to be something along

```
if first_token != Some(TokenKind::String) && token != TokenKind::Comment {
    is_docstring = false
}
```

Which, I think, should remove the need for the check on line 423.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:428 on 2024-01-10 08:31_

It would be helpful to add a comment here describing why unwrapping `first_token_range` (and further below `first_token`) is safe. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:483 on 2024-01-10 08:32_

Nit
```suggestion
    pub(crate) fn check_lines(
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:33 on 2024-01-10 08:33_

Nit: Let's move `BlankLinesChecker` (together with `Follows` and `Status`) closer to its implementation block. It reduces the amount of scrolling necessary when reading the code.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:22 on 2024-01-10 08:37_

This `allow` seems unnecessary 
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:482 on 2024-01-10 08:38_

It seems that this `allow` isn't needed (clippy seems happy without it)

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:32 on 2024-01-10 08:39_

Let's use `TokenKind` for `previous_unindented_token` to avoid cloning (and speed up the rule)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:536 on 2024-01-10 08:45_

How does this handle nested classes and functions? Like, how does it remember the indent level of nested classes and functions? 


```python
class Test:
    def test():
        class Inner:
            def inner():
                pass
    def no_spacing():
        pass    
    
```

I fear that we need to keep a stack where each `Stack` entry is either a `Function` or `Class`, with the current indent level. 

The advantage of using a Stack is that it probably helps to remove some of the duplication (we now have code that handles the class and almost the same code that handles the function cases)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:555 on 2024-01-10 08:51_

IMO, it helps readability if we match on Status first. It makes it clear, that these two blocks handle two different statuses (I first worried that the `!line.is_comment_only` path might override some of the changes made above, but that's not the case)


```suggestion
        match self.class_status {
            Status::Inside(nesting_indent) => {
                if line.indent_level < nesting_indent {
                    if line.is_comment_only {
                        self.class_status = Status::CommentAfter(nesting_indent);
                    } else {
                        self.class_status = Status::Outside;
                    }
                }
            }
            Status::CommentAfter(indent) => {
                if !line.is_comment_only {
                    if line.indent_level >= indent {
                        self.class_status = Status::Inside(indent);
                    }
                    self.class_status = Status::Outside;
                }
            }
            Status::Outside => {
                // Nothing to do
            }
        }

        if let Status::Inside(nesting_indent) = self.fn_status {
            if line.indent_level < nesting_indent {
                if line.is_comment_only {
                    self.fn_status = Status::CommentAfter(nesting_indent);
                } else {
                    self.fn_status = Status::Outside;
                }
            }
        }

        // A comment can be de-indented while still being in a class/function, in that case
        // we need to revert the variables.
        if !line.is_comment_only {
            if let Status::CommentAfter(indent) = self.fn_status {
                if line.indent_level >= indent {
                    self.fn_status = Status::Inside(indent);
                }
                self.fn_status = Status::Outside;
            }
        }

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:545 on 2024-01-10 08:52_

Should this line be inside an `else` branch? It otherwise always overrides the status set inside the `if line.indent_level >= indent` branch.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:344 on 2024-01-10 08:54_

Would you mind adding a comment explaining the difference between `blank_lines` and `preceding_blank_lines`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:602 on 2024-01-10 08:56_

What happens if there are more blank lines than `BLANK_LINES_TOP_LEVEL`? Will there be no fix? How do we ensure the subtraction doesn't underflow in this case.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:704 on 2024-01-10 09:07_

As far as I understand it's impossible for `first_token` to be the `Async` token because we explicitly skip it in the `LinePreprocessor`. 

```suggestion
            Tok::Def => {
```

It could make sense to have a custom `FirstToken` enum that only has the variants that we care about in the `BlankLines` implementation (could it be the same as `Follows`?). It would move the detection of the supported kinds into the `LinePreprocessor` and make it easier to reason about if all relevant tokens have been handled in the `check_line` function

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:393 on 2024-01-10 09:08_

If we don't have it already. Can we add a test for `async with ` to verify that async with statements aren't incorrectly detected as async function definitions.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:719 on 2024-01-10 09:13_

Setting a boolean is fairly cheap. We can just always set it (branching may easily be more expensive when the branch predictor fails to predict the right branch. That's why writing the boolean is probably always faster and it's less code)
```suggestion
            self.is_not_first_logical_line = true;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:723 on 2024-01-10 09:15_

Nit: We can move this out of the `!line.is_comment_only` block to reduce some nesting (which is safe, because `is_docstring` can never be true for `is_comment_only`)

```rust
if line.is_docstring {
    self.follows = Follows::Docstring
}

if !line.is_comment_only {
    ...
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:340 on 2024-01-10 09:16_

It seems to be sufficient to only store the end offset of the last token.
```suggestion
    last_token_end: TextSize,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:727 on 2024-01-10 09:21_

Testing for comments here seems unnecessary because a logical line starting with a comment is guaranteed to be a comment only line
```suggestion
            if line.indent_level == 0 {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:343 on 2024-01-10 09:24_

Calling this field `indent_level` can be misleading because, from what I understand from the code, it stores the indentation length in columns. 

To explain the difference, let's take a look at the following example:

```python
def test(a):
    if a == 1:
        pass
```

The indentation of `pass` is 8 spaces but the indent-level is 2. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:438 on 2024-01-10 09:25_

I just noticed that `expand_indent` hardcodes the size of tabs to 8, which is incorrect. It should respect the `LinterSettings::tab_size` setting

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:687 on 2024-01-10 09:30_

Simply adding `indent_size` (which is always 4) won't produce the right result if a user uses e.g. 2 spaces indentation. 

I recommend changing `Inside` to store the indentation of the Class rather than the indentation of its body (removing the need to add `indent_size`). 

We know we're outside of the class if the `actual_indent <= class_indent`. 

Let's also add some tests with an indent other than 4 (2 spaces, tabs, mixed spaces and tabs)



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:660 on 2024-01-10 09:32_

Nit: 

```suggestion
                && line.first_token == Tok::Def
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:589 on 2024-01-10 09:32_

Nit
```suggestion
                // Only apply to functions or classes.
                && is_top_level_token_or_decorator(&line.first_token)
```

---

_@MichaReiser requested changes on 2024-01-10 09:36_

Thank you for reworking your implementation and extracting it from logical lines. I like the outcome and don't mind that it duplicates some of the logical line detection (which isn't much).

There are a few improvements/clarifications that we should follow up before I do a second review.  

There's an opportunity to speed up the implementation by using `TokenKind` over `Tok`. 

It's unclear to me if nested classes and functions are handled correctly. I would appreciate you having a second look and/or an explanation why it works as expected

I haven't reviewed the fix logic in detail yet. I'll do that once I have a better understanding for the code, thanks to your explanations. 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/logical_lines.rs`:41 on 2024-01-10 09:37_

Would you mind reverting this unrelated change?

---

_@MichaReiser reviewed on 2024-01-10 09:37_

---

_Label `rule` added by @MichaReiser on 2024-01-10 09:38_

---

_@hoel-bagard reviewed on 2024-01-10 11:48_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:390 on 2024-01-10 11:48_

I've done the changes in f3305841b.
Is it faster to do a `==` comparison than a `matches!` ? Or is it for readability reasons ?

---

_@hoel-bagard reviewed on 2024-01-10 11:52_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:381 on 2024-01-10 11:52_

This can happen yes. I want `first_token_range` to be the start of the line, however `first_token` is not necessarily the first token of the line, it is the first token that I care about later on (for example indents or `async` are ignored). So it can happen that `first_token_range` is `Some` while `first_token` is `None`.

---

_@hoel-bagard reviewed on 2024-01-10 12:01_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:403 on 2024-01-10 12:01_

I could rename `line_is_comment_only` to `line_is_trivia`. However, since new lines and indents are skipped, a line can have `line_is_comment_only` set to `true` if and only if it contains a comment. Therefore I think it is more helpful to have `line_is_comment_only` as the variable name.

---

_@hoel-bagard reviewed on 2024-01-10 12:37_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:450 on 2024-01-10 12:37_

To be honest I remember being extremely confused too, and only briefly understood this. This exists to match the pycodestyle results (especially when it comes to comments iirc).

I'll try to understand this again and write it down this time.

---

_@hoel-bagard reviewed on 2024-01-10 13:01_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:408 on 2024-01-10 13:01_

I've tried to modify/explain this part in 22cd072df. 

~~The check (that used to be, is now below) on line 423 could be removed if we allow a comment only line to be considered a docstring line. It does not break anything, but it feels like it could lead to confusion later on.~~
Edit: it would break things.

```rust
                    // For a line to be a docstring, the first token must be a string (since indents are ignored)
                    if first_token != Some(TokenKind::String) {
                        is_docstring = false;
                    }
```

---

_@hoel-bagard reviewed on 2024-01-10 13:11_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:428 on 2024-01-10 13:11_

Done in 3f3ccfa8f.

---

_@hoel-bagard reviewed on 2024-01-10 13:12_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:483 on 2024-01-10 13:12_

Done in 12d80ed6b.

---

_@hoel-bagard reviewed on 2024-01-10 13:15_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:33 on 2024-01-10 13:15_

Done in 86c473cbf

---

_@hoel-bagard reviewed on 2024-01-10 13:18_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:22 on 2024-01-10 13:18_

Done in c347761ef

---

_@hoel-bagard reviewed on 2024-01-10 13:24_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:482 on 2024-01-10 13:24_

Yes, sorry. That was a left-over from an earlier version.

Removed in 3df1f1b01

---

_@hoel-bagard reviewed on 2024-01-10 13:25_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:32 on 2024-01-10 13:25_

Done in ede587a.

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:536 on 2024-01-10 13:32_

It only keeps track of the first one (be it a `Class` of a `Def`), since we only care about whether we are indented or not (see snippet below).

```rust

        match line.first_token {
            TokenKind::Class => {
                if matches!(self.class_status, Status::Outside) {
                    self.class_status = Status::Inside(line.indent_level + indent_size);
                }
                self.follows = Follows::Other;
            }
            TokenKind::At => {
                self.follows = Follows::Decorator;
            }
            TokenKind::Def | TokenKind::Async => {
                if matches!(self.fn_status, Status::Outside) {
                    self.fn_status = Status::Inside(line.indent_level + indent_size);
                }
                self.follows = Follows::Def;
            }
            TokenKind::Comment => {}
            _ => {
                self.follows = Follows::Other;
            }
        }
```

Following [this comment](https://github.com/astral-sh/ruff/pull/8720#issuecomment-1821046269), it might even be possible to ignore classes and functions entirely, and only look at the indentation. However some rule descriptions talk about classes and functions, so I kept the inside/outside statuses.

---

_@hoel-bagard reviewed on 2024-01-10 13:32_

---

_@hoel-bagard reviewed on 2024-01-10 13:42_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:343 on 2024-01-10 13:42_

I looked at the `indent_level` used when instantiating  the `LogicalLineInfo`, and it is 8 for `pass`. Where is it 2 ?

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:438 on 2024-01-10 13:44_

This isn't something I added in this PR, so I will open another PR for this if that's ok.

---

_@hoel-bagard reviewed on 2024-01-10 13:44_

---

_@hoel-bagard reviewed on 2024-01-10 13:48_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:660 on 2024-01-10 13:48_

I assumed you meant that the visual indent was wrong and fixed it in 3941e8f99.

---

_@hoel-bagard reviewed on 2024-01-10 13:51_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:589 on 2024-01-10 13:51_

I also assumed this was about visual indentation, since the suggestion doesn't make sense (I think ?)

1831879a2

---

_@hoel-bagard reviewed on 2024-01-10 13:54_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/checkers/logical_lines.rs`:41 on 2024-01-10 13:54_

This happened because at some point I tracked previous lines (including comments) in this file, sorry for the left-over.

Done in 52589e433.

---

_@hoel-bagard reviewed on 2024-01-10 14:22_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:687 on 2024-01-10 14:22_

I've removed the hard coded indent size in 94c9babff.

I've added a few tests in 4f2c318a7, but they all passed from the start so that might not be enough.

---

_@hoel-bagard reviewed on 2024-01-10 14:24_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:545 on 2024-01-10 14:24_

Fixed in da11f0284

---

_@hoel-bagard reviewed on 2024-01-10 14:29_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:602 on 2024-01-10 14:29_

> What happens if there are more blank lines than BLANK_LINES_TOP_LEVEL?

There is a if before that line that includes `line.preceding_blank_lines < BLANK_LINES_TOP_LEVEL` as a condition, so this cannot happen.

> Will there be no fix? 

`E303` (a different violation) is in charge of handling too many blank lines.

---

_@hoel-bagard reviewed on 2024-01-10 14:36_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:704 on 2024-01-10 14:36_

I've removed the `Async` in 531ce0ed5. It's a left-over from before I realized that `with async` would cause trouble and started to ignore asyncs.

> It could make sense to have a custom FirstToken enum that only has the variants that we care about in the BlankLines implementation (could it be the same as Follows?). It would move the detection of the supported kinds into the LinePreprocessor and make it easier to reason about if all relevant tokens have been handled in the check_line function

I don't think it would make `check_line` easier to reason about since it checks the violations one by one instead of matching on the `TokenKind` (that's probably not optimal in terms of speed, but it makes it much, much easier to see what leads to a given violation being raised).

---

_@hoel-bagard reviewed on 2024-01-10 14:38_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:719 on 2024-01-10 14:38_

Fixed in cc81cc361

---

_@hoel-bagard reviewed on 2024-01-10 14:41_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:393 on 2024-01-10 14:41_

There is already one, I noticed it thanks to the `ruff-ecosystem` results (this github action proved really useful to find all kinds of false positives).
The `Async` tokens are ignored precisely in order to not incorrectly detect `async with`  as `async def`.

---

_@MichaReiser reviewed on 2024-01-10 14:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/logical_lines.rs`:41 on 2024-01-10 14:43_

No worries, all good :) Thank you

---

_@hoel-bagard reviewed on 2024-01-10 14:43_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:723 on 2024-01-10 14:43_

Done in bdfb1e3ac

---

_@hoel-bagard reviewed on 2024-01-10 14:46_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:727 on 2024-01-10 14:46_

Done in 894bc21ef

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:340 on 2024-01-10 14:48_

Done in 0f659b965.

---

_@hoel-bagard reviewed on 2024-01-10 14:48_

---

_@hoel-bagard reviewed on 2024-01-10 14:50_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:382 on 2024-01-10 14:50_

Done in [ede587a](https://github.com/astral-sh/ruff/pull/9266/commits/ede587a6c14a8e1d8172579aadc8cfce598d387c)

---

_@MichaReiser reviewed on 2024-01-10 14:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:343 on 2024-01-10 14:50_

The level is two because:

```python
def test(a):  # Level 0, no indentation 
    if a == 1:  # Level 1, the if statement is indented
        pass	 # Level 2, one level deeper than the if statement
```

So the level is only concern with how nested the statement is, not with the length of the indentation. 

It's probably a very poor analogy: Assume you use an elevator to get to level 3. The button is labeled 3 and does not go to 20 meters above ground level. 

---

_Comment by @hoel-bagard on 2024-01-10 15:06_

@MichaReiser Thank you for your review.

I've started by replacing `Tok` by `TokenKind`, which made applying most suggestions directly from GitHub impossible. I'll finish doing them by hand later this week.

> It's unclear to me if nested classes and functions are handled correctly. I would appreciate you having a second look and/or an explanation why it works as expected

Based on comparison with pycodestyle and looking at the `ruff-ecosystem` results it looks fine.
The handling is fairly simple since we only care about whether we are inside a class or function, not how deeply nested  we are, so keeping track of the first indentation is enough. The one tricky part is handling poorly indented comments, `Status::CommentAfter` exists to handle this case.

> I haven't reviewed the fix logic in detail yet. I'll do that once I have a better understanding for the code, thanks to your explanations.

I'll do my best to explain things, but I must admit that things got quite convoluted when I tried to match the pycodestyle results instead of just implementing the rules as they were written. The comments handling lead to especially convoluted code (iirc that's why `current_blank_lines` and `previous_blank_lines` exist).

---

_@hoel-bagard reviewed on 2024-01-10 15:10_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:343 on 2024-01-10 15:10_

Thank you for the explanation, I first thought that the value was wrong :man_facepalming: 

Fixed in 0d3644445.

---

_Comment by @MichaReiser on 2024-01-11 07:34_

Thank you for all your work your putting in this. 

I'm a bit disappointed that changing to `TokenKind` didn't improve performance more. I'll have to look at the codspeed benchmarks when I have time to see where we spend the time. I hope that we can improve it a little more. 

Edit: I just saw that the benchmarks didn't run. There's still hope :laughing: 

>  which made applying most suggestions directly from GitHub impossible.

Oh I'm sorry for this. 

> Based on comparison with pycodestyle and looking at the ruff-ecosystem results it looks fine.

Thanks for explaining. I'll have a second look. Most importantly, we have tests for it, showing how we expect it to work. That will help future us when making changes, to ensure we don't break it accidentally. 

Would you mind requesting re-review when you think it's ready? It would help me to know when I should have another look.

---

_@hoel-bagard reviewed on 2024-01-13 12:46_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:472 on 2024-01-13 12:46_

Done in 605470728

---

_@hoel-bagard reviewed on 2024-01-13 12:59_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:438 on 2024-01-13 12:59_

PR opened: https://github.com/astral-sh/ruff/pull/9506

---

_@hoel-bagard reviewed on 2024-01-13 13:10_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:344 on 2024-01-13 13:10_

Done in 4c54d05a9

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:450 on 2024-01-13 13:24_

Done in c07ccdf9f

---

_@hoel-bagard reviewed on 2024-01-13 13:24_

---

_Comment by @hoel-bagard on 2024-01-13 13:45_

> I'll have to look at the codspeed benchmarks when I have time to see where we spend the time. I hope that we can improve it a little more.

The result from codespeed was fine until this commit [b7727c5](https://github.com/astral-sh/ruff/pull/9266/commits/b7727c5e73c646637883e8e24a3802575d09a9e8), but when I tried to get the changes from master it wasn't fine anymore. I wonder what changed.

> Thanks for explaining. I'll have a second look. Most importantly, we have tests for it, showing how we expect it to work. That will help future us when making changes, to ensure we don't break it accidentally.

We have all the tests from pycodestyle, plus I went through the ruff-ecosystem results a few times and every time I found a false-positive I added a test for it. I sadly can't guarantee that the two perfectly matches, but if there are discrepancies it should only be in corner cases. If this is merged and any issue arises later on, I would gladly fix it.

---

_Review requested from @MichaReiser by @hoel-bagard on 2024-01-13 13:45_

---

_@MichaReiser reviewed on 2024-01-15 09:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:390 on 2024-01-15 09:58_

It's mostly for readability. I find `matches!` rather verbose to read, and negating matches always breaks my flow (because there are two `!`, one because it is a macro, and the other for negating. 

One difference between `==` and `matches!` is that `matches!` works in `const` context whereas `==` does not because it calls the implementation of the non const `Eq::eq` function. 

The alternative is to create `is_*` methods on `TokenKind` and use `matches!` internally. That's a bit of boilerplate but I often find it worth adding to improve readability.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:401 on 2024-01-15 10:03_


```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:381 on 2024-01-15 10:04_

Nit: We use `range.len()`, suggesting that the unit is in bytes and not in characters (this happens to be the same for all python whitespace characters but I still recomment renaming it to `current_blank_len` and using `TextSize` to make it clear that the unit is bytes)
```suggestion
        let mut current_blank_characters: usize = 0;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:452 on 2024-01-15 10:10_

You mentioned in https://github.com/astral-sh/ruff/pull/9266#discussion_r1447356252 that changing the `is_docstring` check would break things but I'm unable to reproduce when running the test set locally. 

Please add a test that demonstrates why refactoring the code to the following implementation breaks the rule.
```suggestion
            // A docstring line is composed only of the docstring (TokenKind::String) and trivia tokens.
            // (If a comment follows a docstring, we still count the line as a docstring)
            if first_token != Some(TokenKind::String) && token_kind != TokenKind::Comment {
                is_docstring = false
            }

            match token_kind {
                TokenKind::Lbrace | TokenKind::Lpar | TokenKind::Lsqb => {
                    parens = parens.saturating_add(1);
                }
                TokenKind::Rbrace | TokenKind::Rpar | TokenKind::Rsqb => {
                    parens = parens.saturating_sub(1);
                }
                TokenKind::Newline | TokenKind::NonLogicalNewline if parens == 0 => {
                    let last_token_end = range.end();

                    let first_range = first_token_range
                        .expect("that Newline cannot be the first token of a line (there is at least a NonLogicalNewline token)");

                    let range = TextRange::new(
                        self.locator.line_start(first_range.start()),
                        first_range.start(),
                    );

                    let indent_length = expand_indent(self.locator.slice(range), self.indent_width);
```

Your check is accurate but I think we can improve readability a bit by:

1. Initialize `is_docstring` with `false`  
2. Inside of `if first_token.is_none()` assign `is_docstring = token_kind == TokenKind::String;`
3. Remove the `if first_token != Some(...)` in the newline | `NonLogicalLine` handling. 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:461 on 2024-01-15 10:46_

This panics for a line that only contains the `async` keyword because it gets skipped. Would it complicate the logic a lot if we wouldn't skip `async` here and instead handle it downstream the same as `def`? 


```python
class Test:
    async

    def a(self): pass

```

This is not valid python code but it's a valid token stream

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:459 on 2024-01-15 10:51_

The expect messages is inaccurate. We never initialize `first_token` with `NonLogicalNewline`. But we shouldn't make it hear if that's happening. 

---

_Comment by @MichaReiser on 2024-01-15 12:52_

pycodestyle seems to drip over this example:

```python
class Test:
    def another(self):
        pass

    # comment


    # another comment
    def test(self):
        pass
```

It reports that there's a missing empty line:

```
/home/micha/.config/JetBrains/RustRover2023.3/scratches/scratch_5.py:8:5: E303 too many blank lines (2)
/home/micha/.config/JetBrains/RustRover2023.3/scratches/scratch_5.py:9:5: E301 expected 1 blank line, found 0
```

Removing the two empty lines between the comments make it pass..

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:4 on 2024-01-15 13:22_

`pycodestyle` found(1)
```suggestion
E30.py:621:1: E305 [*] expected 2 blank lines after class or function definition, found (1)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:23 on 2024-01-15 13:23_

pycodestyle emits: `found(1)`
```suggestion
E30.py:632:1: E305 [*] expected 2 blank lines after class or function definition, found (1)
```

---

_Comment by @hoel-bagard on 2024-01-15 13:24_

@MichaReiser Matching pycodestyle as best as possible was one of the pain points in this PR...

If we follow the rules, I think pycodestyle is wrong on this example. But pycodestyle is the reference, so should ruff match the pycodestyle behavior on this example too ?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:470 on 2024-01-15 13:27_

`preceding_blank_characters` is confusing because it stores the blank characters of the current new line. The variable should either be renamed (to `current_blank_len`) OR store the length of the preceding blank lines. 

If it stores the `preceding_blank_lines`, than setting `self.preceding_blank_lines = current_blank_lines` is problematic because the length of the blank lines isn't updated.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:677 on 2024-01-15 13:34_

Subtracting BLANK_LINEs_METHOD_LEVEL from the blank lines count isn't safe on windows where new line characters use 2 bytes: `\r\n`. 

The safe fix here is to replace all blank lines and insert the right amount of newlines.

I think the safe thing here would be to track the range of the blank lines and replace the entire range with the right amount of newlines. This requires tracking the range of the blank lines. 

I started adding a helper struct for it 

```rust

#[derive(Clone, Debug)]
enum BlankLines {
    /// No blank lines
    Zero,

    /// One or more blank lines
    Many { count: NonZeroU32, range: TextRange },
}

impl Default for BlankLines {
    fn default() -> Self {
        BlankLines::Zero
    }
}

impl BlankLines {
    fn add(&mut self, line_range: TextRange) {
        match self {
            BlankLines::Zero => {
                *self = BlankLines::Many {
                    count: NonZeroU32::MIN,
                    range: line_range,
                }
            }
            BlankLines::Many { count, range } => {
                assert_eq!(range.end(), line_range.start());
                *count = count.saturating_add(1);
                *range = TextRange::new(range.start(), line_range.end());
            }
        }
    }

    fn count(&self) -> u32 {
        match self {
            BlankLines::Zero => 0,
            BlankLines::Many { count, .. } => count.get(),
        }
    }
}

```

Where you can write 

```rust
if let BlankLines::Many { count: blank_lines, range: blank_lines_range } = line.blank_lines {
    // add check here
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:694 on 2024-01-15 13:44_

`preceding_blank_characters` seems to be the length of `blank_lines`, subtracting it here is, therefore, not safe (you may start slicing arbitrary content.). Should this be using `blank_lines` too?



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:687 on 2024-01-15 13:54_

Let's add a test with a comment. Pycodestyle seems to accept this but I think it is incorrect


```python
@decorator

# comment
def function():
    pass
```

---

_@MichaReiser requested changes on 2024-01-15 14:06_

We don't need to match pycodestyle exactly if we have a good reason for it. 

I did another pass and made a few changes myself. I hope I didn't introduce any regressions. 

I found many cases where the conditions are testing `blank_lines` and the code fix / diagnostic than reference `preceding_blank_lines` or the other way round. I don't think that's intentional. We should review the diagnostics to ensure we use the right blank lines variable consistently. 

Related, some fixes use `blank_lines_len` (I renamed `preceding_blank_lines_characters` because it isn't the length of the preceding blank lines) but the rule tests for `preceding_blank_lines` which feels incorrect. 

There are also tests where `pycodestyle` gives me different results than what we see in the snapshot. 

that's why I think we need to do another pass on this PR and I recommend starting with the tests:

* Run each sample with pycodestyle and ensure our snapshots report the same errors as pycodestyle
* Carefully consider why some rules use `blank_lines` and other use `preceding_blank_lines`. 


---

_Comment by @sihil on 2024-01-17 14:51_

I'm very cheekily adding a comment here to see if there is interest in mopping up W391 (blank line at end of file) in this as it's so closely related. 😇 

---

_Comment by @MichaReiser on 2024-01-17 15:43_

> I'm very cheekily adding a comment here to see if there is interest in mopping up W391 (blank line at end of file) in this as it's so closely related. 😇

I recommend not increasing scope for now. The PR is already massive and it would be nice to get it merged. 

---

_Comment by @hoel-bagard on 2024-01-19 14:12_

Thank you for the review, I'll start by re-checking the tests.

As a first note, the following test (mixing spaces and tabs) causes pycodestyle to crash/exit, so the results can't be compared.

```python
async def function1():
	await function2()
    async with function3():
    	pass
```

---

_Comment by @MichaReiser on 2024-01-19 14:20_

> Thank you for the review, I'll start by re-checking the tests.
> 
> As a first note, the following test (mixing spaces and tabs) causes pycodestyle to crash/exit, so the results can't be compared.
> 
> ```python
> async def function1():
> 	await function2()
>     async with function3():
>     	pass
> ```

Oh interesting. Maybe split this test (or all messed up indent tests) into their own file to?

---

_Comment by @hoel-bagard on 2024-01-19 14:34_

I've compared the ruff and pycodestyle results and I found only one mismatch/error in terms of errors raised (I haven't checked the amount of lines detected yet). In the sample below, E304 was detected on both the comment and the def. I'll make it so that it's not detected on the comment.

```python
@decorator

# comment
def function():
    pass
```

Edit: the auto-fix requires a bit more changes to work, especially in order to support code like this:
```python
@decorator

# comment

# a second comment
def function():
    pass
```


---

_Comment by @hoel-bagard on 2024-01-19 14:35_

> Maybe split this test (or all messed up indent tests) into their own file to?

What do you mean ? Making a PR to pycodestyle to add this test to their tests ?

---

_Comment by @MichaReiser on 2024-01-19 14:47_

> > Maybe split this test (or all messed up indent tests) into their own file to?
> 
> What do you mean ? Making a PR to pycodestyle to add this test to their tests ?

Sorry no. I mean that you could split the problematic tests into a separate test file (in our repository). There's no rule enforcing that each rule only has one test file. We can have many test files per rule.

---

_Comment by @hoel-bagard on 2024-01-19 14:56_

@MichaReiser 
When comparing the number of lines detected, there are 3 differences with pycodestyle:

<table>
<tr>
<td> Snipet </td> <td> Ruff </td> <td> Pycodestyle </td>
</tr>

<tr>
<td>

```python
# E305:7:1
def fn():
    print()

    # comment

    # another comment
fn()
# end
```
</td>
<td> E305 expected 2 blank lines after class or function definition, found (0) </td>
<td> E305 expected 2 blank lines after class or function definition, found (1) </td>
</tr>

<tr>
<td>

```python
# E305
class Class():
    pass

    # comment

    # another comment
a = 1
# end
```
</td>
<td> E305 expected 2 blank lines after class or function definition, found (0) </td>
<td> E305 expected 2 blank lines after class or function definition, found (1) </td>
</tr>

<tr>
<td>

```python
# E305:5:1
def a():
    print

# Two spaces before comments, too.
if a():
    a()
# end
```
</td>
<td> E305 expected 2 blank lines after class or function definition, found (0) </td>
<td> E305 expected 2 blank lines after class or function definition, found (1) </td>
</tr>

</table>

Matching pycodestyle is very easy, we just need to use `preceding_blank_lines` instead of `blank_lines` when creating the error message, so I did that for now.

Ideally I would have liked to have the ruff output for the first two, and the pycodestyle output for the last one. But I think that would require tracking additional variables.

I thought about having the error on the comment line, but that would not match pycodestyle, and handling cases such as the one below would be quite annoying.
```
def fn():
    pass
# wrongly indented comment

    pass
```

---

_Comment by @hoel-bagard on 2024-01-19 15:02_

> Sorry no. I mean that you could split the problematic tests into a separate test file (in our repository). There's no rule enforcing that each rule only has one test file. We can have many test files per rule.

There's only one example that pycodestyle cannot handle, so I removed it from the test file while comparing  the outputs. Having a second test file just for that example seems a bit too much to me, especially since we won't compare the outputs with pycodestyle once the ruff test fixtures become the reference.
I can split the test file if you think it's a better idea though.

---

_@hoel-bagard reviewed on 2024-01-19 15:04_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:687 on 2024-01-19 15:04_

Done in https://github.com/astral-sh/ruff/pull/9266/commits/e739a445c89e884b07da6ac3330948c7e63c0088

---

_@hoel-bagard reviewed on 2024-01-20 03:52_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:677 on 2024-01-20 03:52_

Thank you for providing the enum and its `impl`.

I've made the changes in 2c08381d9 and fddc74013.  There were some snapshot changes, but they looked good.

---

_@hoel-bagard reviewed on 2024-01-20 05:42_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:694 on 2024-01-20 05:42_

I've tried to improve this in 7ee8946ad (also allowing to fix the test below).

```python
@decorator

# comment    E304 not expected


# comment    E304 not expected
def function():
    pass
```

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:470 on 2024-01-20 05:46_

Fixed in e739a445c89e884b07da6ac3330948c7e63c0088

---

_@hoel-bagard reviewed on 2024-01-20 05:46_

---

_@hoel-bagard reviewed on 2024-01-20 05:47_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:4 on 2024-01-20 05:47_

See https://github.com/astral-sh/ruff/pull/9266#issuecomment-1900573236

---

_@hoel-bagard reviewed on 2024-01-20 05:47_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E305_E30.py.snap`:23 on 2024-01-20 05:47_

See https://github.com/astral-sh/ruff/pull/9266#issuecomment-1900573236

---

_Comment by @MichaReiser on 2024-01-22 08:10_

@hoel-bagard I agree that what you're proposing would be ideal (diverting for 1 and 2, matching pycodestyle for 3) and also agree that matching pycodestyle is probably the best for now (we can experiment with an improved detection in a follow up PR). 

Let me know when this is ready for re-review or if you need any more help. 

---

_Comment by @hoel-bagard on 2024-01-22 08:41_

@MichaReiser I will add comments to explain why `blank_lines`/`preceding_blank_lines` is used for each rule, then the PR should be ready for re-review. I think I'll be able to do it today.

EDIT: since the explanation was essentially the same for every rule, I added a single comment in the struct definition.

---

_Review requested from @MichaReiser by @hoel-bagard on 2024-01-22 12:46_

---

_Comment by @MichaReiser on 2024-02-05 12:43_

@hoel-bagard is this PR ready for review or have you planned to make more changes? 

---

_Comment by @hoel-bagard on 2024-02-05 12:51_

@MichaReiser It's ready for review. I think I applied all the suggestions you made.

---

_Comment by @MichaReiser on 2024-02-05 13:00_

> @MichaReiser It's ready for review. I think I applied all the suggestions you made.

Ah perfect. Sorry for the long wait. I thought you wanted to make more changes. I'll have a look this week!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:628 on 2024-02-05 22:35_

This seems incorrect: It always overrides `self.class_status` with `Status::Outside` even when `line.indent_length > indent`

Should this be in an else branch? Can we add tests exercising this branch

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:625 on 2024-02-05 22:36_

Should this be `>=` to continue `Inside` a class if the `indent` remains unchanged. Do we have tests covering this branch?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:778 on 2024-02-05 23:00_

I don't think using `stylist.line_ending().text_len()` is safe in case the file uses mixed newlines because the first newline could be `\n` but the newline after the decorator might be `\r\n` (the problem we had with other blank line replacement bugs as well)

What are we trying to do here? 

Shouldn't `self.last_non_comment_line_end` already give you the end position of the decorator line? 

What seems to be the easiest fix is to use 

```rust
               diagnostic.set_fix(Fix::safe_edit(Edit::range_deletion(
                    without_blank_lines.to_string(),
                    TextRange::new(
                        self.last_non_comment_line_end,
                        line.first_token_range.start(),
                    ),
                )));
```

but it doesn't handle the case where you have a comment in the range between (which the old implementation handles). However, the old implementation only handles `\n` but not `\r` or `\r\n`. 
We would need to adopt your fix to handle any possible empty line sequence and replace it with `stylist.newline` (and not `\n`)

To help future readers understand this code better, it might help to add a example code snipped to the outer if branch: like

```rust
// Removes unnecessary empty lines between the decorator and a function or class definition
// ```python
// @decorator
// 
// def test(): pass
// ```
```

---

_@MichaReiser reviewed on 2024-02-05 23:23_

This is looking good. We're very close. 

There's one fix that we need to look into to properly support different line endings. 

The other nice addition would be if you could add some documentation with code snippets on top of the large fix-branches. It could help future reader better understand what's happening

---

_@MichaReiser reviewed on 2024-02-06 01:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:778 on 2024-02-06 01:24_

Okay. I pushed a version that addresses the line ending issue. A few inline documentation blocks examples with examples might still be useful

---

_@MichaReiser approved on 2024-02-06 01:26_

I pushed a change that avoids hardcoding the newline in the remaining fix and this now looks good to me. I've a few small questions about the correctness that I would be thankful if you could take a look and this is then ready to merge. 

I'll ask @charliermarsh to take a look at the rule, in case he wants to make any changes to the documentation

---

_Comment by @hoel-bagard on 2024-02-06 10:32_

@MichaReiser 

> The other nice addition would be if you could add some documentation with code snippets on top of the large fix-branches. 

By "large fix-branches", do you mean each `E30X` branch ? Wouldn't that be handled by the docstring of each error ?

---

_@hoel-bagard reviewed on 2024-02-06 10:39_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:628 on 2024-02-06 10:39_

Else branch added in 8392f204ccd1bd0b8ed5d870fdf406f8f47f1487. 2 tests added in 6820cbeb5.

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:625 on 2024-02-06 11:21_

Because we only enter `Status::CommentAfter` when encountering a comment that is "outside" the indented block,  `>` is correct. On the contrary, using `>=` will cause E306 false positives.

We have tests like the one below.

```python
# E301
class Class:

    def fn1():
        pass
    # comment
    def fn2():
        pass
# end
```

---

_@hoel-bagard reviewed on 2024-02-06 11:21_

---

_Comment by @MichaReiser on 2024-02-06 14:12_

@hoel-bagard I didn't notice that each branch has its own diagnostic. I agree, that that's sufficient. 

So this is ready to merge. Congrats on the work, your endurance, and your patience with us. This is excellent. I'll give @zanieb and @charliermarsh a day or two. They are the masters of our documentation. 

@zanieb @charliermarsh The implementation looks good on my end. You don't have to review its code, except if the preview gating is correct. I'm mainly interested to get a second opinion on the rule description

---

_Label `preview` added by @MichaReiser on 2024-02-06 14:13_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-06 14:13_

---

_Review requested from @zanieb by @MichaReiser on 2024-02-06 14:13_

---

_@zanieb reviewed on 2024-02-06 19:23_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:35 on 2024-02-06 19:23_

Why do we mention the recommended spacing between functions and classes if this rule is just for methods of a class? It might make sense to just say "PEP 8 recommends exactly one blank line between methods of a class". We should also not specify "missing" if this will also trigger for "too many" blank lines, instead we should say "Checks for the correct number of blank lines between methods of a class.

We could move all of this into "What it does" and have "Why is this bad" focus on consistency with PEP 8? I don't have strong feelings about that though.

---

_@zanieb reviewed on 2024-02-06 19:24_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:84 on 2024-02-06 19:24_

Same as https://github.com/astral-sh/ruff/pull/9266/files#r1480433058

---

_@zanieb reviewed on 2024-02-06 19:25_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:74 on 2024-02-06 19:25_

Isn't this always going to be a single blank line (for this rule)? Can't this suggest removing blank lines too?

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:179 on 2024-02-06 19:26_

Isn't this for extra blank lines after function decorators?

---

_@zanieb reviewed on 2024-02-06 19:26_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:214 on 2024-02-06 19:27_

We should capitalize these for consistency with the rest

---

_@zanieb reviewed on 2024-02-06 19:27_

---

_@hoel-bagard reviewed on 2024-02-06 23:39_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:35 on 2024-02-06 23:39_

> Why do we mention the recommended spacing between functions and classes if this rule is just for methods of a class? It might make sense to just say "PEP 8 recommends exactly one blank line between methods of a class".

Done in 3d5cc8455

> We should also not specify "missing" if this will also trigger for "too many" blank lines, instead we should say "Checks for the correct number of blank lines between methods of a class.

This rule does not trigger for too many blank lines.


---

_@hoel-bagard reviewed on 2024-02-06 23:43_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:84 on 2024-02-06 23:43_

Fixed in 0596eda06.

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:74 on 2024-02-06 23:45_

> Isn't this always going to be a single blank line (for this rule)? 

Yes, I've fixed this in d188e200e.

> Can't this suggest removing blank lines too?

This is handled by a different rule (`E303`).

---

_@hoel-bagard reviewed on 2024-02-06 23:45_

---

_@hoel-bagard reviewed on 2024-02-06 23:49_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:179 on 2024-02-06 23:49_

Yes, fixed in e89c20f86.

---

_@hoel-bagard reviewed on 2024-02-06 23:50_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:214 on 2024-02-06 23:50_

Done in fdf603542

---

_@hoel-bagard reviewed on 2024-02-06 23:55_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:35 on 2024-02-06 23:55_

> We could move all of this into "What it does" and have "Why is this bad" focus on consistency with PEP 8? I don't have strong feelings about that though.

I don't have strong feelings about this either, I can try to change it if you want.

---

_Review requested from @zanieb by @hoel-bagard on 2024-02-07 00:12_

---

_Merged by @MichaReiser on 2024-02-08 18:35_

---

_Closed by @MichaReiser on 2024-02-08 18:35_

---

_Comment by @akx on 2024-02-09 07:40_

Merged?! Yay! Thanks for the hard work @hoel-bagard!

---

_Comment by @zanieb on 2024-02-09 08:12_

Thanks for addressing my feedback!

---

_Comment by @spaceone on 2024-02-18 10:23_

I upgraded to ruff 0.2.2, where this is included. But it's still hidden behind some compiler flag? I can't test it without compiling ruff on my own?

---

_Comment by @hoel-bagard on 2024-02-18 10:27_

@spaceone You need to enable the preview mode, see [here](https://docs.astral.sh/ruff/preview/#enabling-preview-mode).

---

_Comment by @Sam1320 on 2024-02-21 16:52_

Great work @hoel-bagard ! I come from this [SO comment](https://stackoverflow.com/questions/76885582/setting-up-ruff-python-linter)  I followed all the previous PRs related to this without much hope. Amazing that I would have been blocked if I checked this only 2 weeks ago.

---
