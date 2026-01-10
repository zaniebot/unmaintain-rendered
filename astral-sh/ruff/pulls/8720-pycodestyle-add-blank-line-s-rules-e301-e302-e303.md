```yaml
number: 8720
title: "[`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)"
type: pull_request
state: closed
author: hoel-bagard
labels:
  - rule
assignees: []
draft: true
base: main
head: add_blank_lines_E30_V2
created_at: 2023-11-16T13:54:54Z
updated_at: 2024-02-18T10:14:06Z
url: https://github.com/astral-sh/ruff/pull/8720
synced_at: 2026-01-10T22:57:09Z
```

# [`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)

---

_Pull request opened by @hoel-bagard on 2023-11-16 13:54_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/2402, it adds the `E301`, `E302`, `E303`, `E304`, `E305`, `E306` error rules along with their fixes.

A first attempt at implementing `E305` using physical lines was done [here](https://github.com/charliermarsh/ruff/pull/4635), and a second attempt using logical lines [here](https://github.com/astral-sh/ruff/pull/4694). This version uses the fact that comment only lines are now counted as logical lines. 

## Test Plan
The test fixture uses [the one from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testsuite/E30.py
) with a few added tests.

---

_Comment by @github-actions[bot] on 2023-11-16 15:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3911 -26 violations, +0 -0 fixes in 15 projects; 26 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/disnake/utils.py#L1051'>disnake/utils.py:1051:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/DisnakeDev/disnake/blob/f780cf5d7320210f022eba0d0e4e8b2820db2baa/examples/basic_voice.py#L96'>examples/basic_voice.py:96:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/data/test_classes/nlu_component_skeleton.py#L11'>data/test_classes/nlu_component_skeleton.py:11:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/agent.py#L117'>rasa/core/agent.py:117:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/console.py#L144'>rasa/core/channels/console.py:144:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/console.py#L218'>rasa/core/channels/console.py:218:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/channels/rasa_chat.py#L46'>rasa/core/channels/rasa_chat.py:46:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/server.py#L579'>rasa/server.py:579:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/scripts/evaluate_release_tag.py#L41'>scripts/evaluate_release_tag.py:41:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/aio_pika/robust_channel.pyi#L10'>stubs/aio_pika/robust_channel.pyi:10:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L23'>stubs/sanic.pyi:23:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/stubs/sanic.pyi#L25'>stubs/sanic.pyi:25:5:</a> E301 [*] Expected 1 blank line, found 0
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+32 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/compat/functools.pyi#L27'>airflow/compat/functools.pyi:27:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/decorators/__init__.pyi#L158'>airflow/decorators/__init__.pyi:158:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/decorators/__init__.pyi#L314'>airflow/decorators/__init__.pyi:314:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/decorators/__init__.pyi#L61'>airflow/decorators/__init__.pyi:61:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/decorators/__init__.pyi#L642'>airflow/decorators/__init__.pyi:642:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/jobs/triggerer_job_runner.py#L601'>airflow/jobs/triggerer_job_runner.py:601:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/providers/amazon/aws/hooks/s3.py#L455'>airflow/providers/amazon/aws/hooks/s3.py:455:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/providers/amazon/aws/hooks/s3.py#L476'>airflow/providers/amazon/aws/hooks/s3.py:476:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/providers/amazon/aws/hooks/s3.py#L582'>airflow/providers/amazon/aws/hooks/s3.py:582:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/apache/airflow/blob/a68b4194fe7201bba0544856b60c7d6724da60b3/airflow/providers/amazon/aws/hooks/s3.py#L619'>airflow/providers/amazon/aws/hooks/s3.py:619:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+114 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/Provided/main.py#L3'>tests/integration/testdata/buildcmd/Provided/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/custom_root_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/custom_root_dir_and_custom_makefile/custom_src_code/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir_and_custom_makefile/custom_src_code/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/custom_root_dir_custom_makefile_and_custom_working_dir/working_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_root_dir_custom_makefile_and_custom_working_dir/working_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/custom_working_dir_src_code/working_dir/main.py#L3'>tests/integration/testdata/buildcmd/custom_working_dir_src_code/working_dir/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/buildcmd/provided_src_code_without_makefile/main.py#L3'>tests/integration/testdata/buildcmd/provided_src_code_without_makefile/main.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
... 108 additional changes omitted for rule E302
+ <a href='https://github.com/aws/aws-sam-cli/blob/4890f4f56fb70ec4e2e66fcc8e1f3fb5154a8ec7/tests/integration/testdata/start_api/lambda_authorizers/app.py#L159'>tests/integration/testdata/start_api/lambda_authorizers/app.py:159:5:</a> E303 [*] Too many blank lines (2)
... 107 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3208 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/docs/bokeh/source/conf.py#L230'>docs/bokeh/source/conf.py:230:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L102'>examples/advanced/extensions/gears/gears.py:102:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L13'>examples/advanced/extensions/gears/gears.py:13:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L16'>examples/advanced/extensions/gears/gears.py:16:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L21'>examples/advanced/extensions/gears/gears.py:21:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L34'>examples/advanced/extensions/gears/gears.py:34:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/gears/gears.py#L62'>examples/advanced/extensions/gears/gears.py:62:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/parallel_plot/parallel_plot.py#L109'>examples/advanced/extensions/parallel_plot/parallel_plot.py:109:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/putting_together.py#L85'>examples/advanced/extensions/putting_together.py:85:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/putting_together.py#L93'>examples/advanced/extensions/putting_together.py:93:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/tool.py#L74'>examples/advanced/extensions/tool.py:74:1:</a> E302 [*] Expected 2 blank lines, found 1
... 2390 additional changes omitted for rule E302
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/tool.py#L78'>examples/advanced/extensions/tool.py:78:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/widget.py#L52'>examples/advanced/extensions/widget.py:52:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/annotations/colorbar_log.py#L19'>examples/basic/annotations/colorbar_log.py:19:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
... 313 additional changes omitted for rule E305
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/autoload_static.py#L34'>examples/output/apis/autoload_static.py:34:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/output/apis/autoload_static.py#L43'>examples/output/apis/autoload_static.py:43:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/release/deploy.py#L55'>release/deploy.py:55:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/client/states.py#L110'>src/bokeh/client/states.py:110:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/document/events.py#L234'>src/bokeh/document/events.py:234:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/document/events.py#L518'>src/bokeh/document/events.py:518:9:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/driving.py#L114'>src/bokeh/driving.py:114:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/driving.py#L170'>src/bokeh/driving.py:170:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/driving.py#L189'>src/bokeh/driving.py:189:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/driving.py#L91'>src/bokeh/driving.py:91:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/__init__.py#L54'>src/bokeh/embed/__init__.py:54:1:</a> E303 [*] Too many blank lines (3)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L323'>src/bokeh/layouts.py:323:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L427'>src/bokeh/layouts.py:427:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
... 185 additional changes omitted for rule E306
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/model/model.py#L552'>src/bokeh/model/model.py:552:13:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/model/model.py#L72'>src/bokeh/model/model.py:72:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/plots.py#L907'>src/bokeh/models/plots.py:907:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/ui/dialogs.py#L58'>src/bokeh/models/ui/dialogs.py:58:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/widgets/tables.py#L154'>src/bokeh/models/widgets/tables.py:154:1:</a> E303 [*] Too many blank lines (3)
... 36 additional changes omitted for rule E303
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/server/server.py#L283'>src/bokeh/server/server.py:283:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/server/session.py#L231'>src/bokeh/server/session.py:231:9:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/typings/selenium/webdriver/common/action_chains.pyi#L10'>src/typings/selenium/webdriver/common/action_chains.pyi:10:5:</a> E301 [*] Expected 1 blank line, found 0
... 253 additional changes omitted for rule E301
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/server/test_server__server.py#L469'>tests/unit/bokeh/server/test_server__server.py:469:1:</a> E304 [*] blank lines found after function decorator
... 3172 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+394 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/api/__init__.py#L10'>common/api/__init__.py:10:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/conversions.py#L3'>common/conversions.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/ffi_wrapper.py#L14'>common/ffi_wrapper.py:14:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/ffi_wrapper.py#L8'>common/ffi_wrapper.py:8:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/file_helpers.py#L74'>common/file_helpers.py:74:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/gpio.py#L12'>common/gpio.py:12:1:</a> E302 [*] Expected 2 blank lines, found 1
... 305 additional changes omitted for rule E302
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/kalman/tests/test_simple_kalman.py#L86'>common/kalman/tests/test_simple_kalman.py:86:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/logging_extra.py#L216'>common/logging_extra.py:216:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (1)
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/transformations/camera.py#L73'>common/transformations/camera.py:73:1:</a> E303 [*] Too many blank lines (3)
+ <a href='https://github.com/commaai/openpilot/blob/ea7e701052f8da1fb562746c62bdb5a64410ea76/common/transformations/model.py#L14'>common/transformations/model.py:14:1:</a> E305 [*] expected 2 blank lines after class or function definition, found (0)
... 384 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/molecule/testinfra/app/test_tor_hidden_services.py#L11'>molecule/testinfra/app/test_tor_hidden_services.py:11:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/freedomofpress/securedrop/blob/531d08696a145b26dd918abbbc68b19d4ce505c0/redwood/redwood/__init__.pyi#L6'>redwood/redwood/__init__.pyi:6:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinkpy/blinkpy.py#L422'>blinkpy/blinkpy.py:422:17:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinkpy/camera.py#L415'>blinkpy/camera.py:415:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L56'>blinksync/forms.py:56:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L71'>blinksync/forms.py:71:5:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L77'>blinksync/forms.py:77:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L9'>blinksync/forms.py:9:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/commands.py#L498'>lnbits/commands.py:498:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/commands.py#L512'>lnbits/commands.py:512:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/commands.py#L547'>lnbits/commands.py:547:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L294'>lnbits/core/services.py:294:9:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L325'>lnbits/core/services.py:325:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L347'>lnbits/core/services.py:347:13:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L389'>lnbits/core/services.py:389:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L424'>lnbits/core/services.py:424:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L488'>lnbits/core/services.py:488:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
+ <a href='https://github.com/lnbits/lnbits/blob/e76ba62b46899832f36bdf1ffd85daae3c9b1898/lnbits/core/services.py#L574'>lnbits/core/services.py:574:5:</a> E306 [*] Expected 1 blank line before a nested definition, found 0
... 17 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed projected reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E302 | 2887 | 2887 | 0 | 0 | 0 |
| E305 | 388 | 388 | 0 | 0 | 0 |
| E301 | 270 | 270 | 0 | 0 | 0 |
| E306 | 265 | 265 | 0 | 0 | 0 |
| E303 | 74 | 74 | 0 | 0 | 0 |
| PLR0917 | 52 | 26 | 26 | 0 | 0 |
| E304 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @hoel-bagard on 2023-11-16 15:44_

---

_Label `rule` added by @charliermarsh on 2023-11-17 14:53_

---

_Comment by @hoel-bagard on 2023-11-19 12:16_

@charliermarsh I've looked at (most) of the ecosystem changes, and they look like desired violations.

However I noticed one big difference between what I did and the pycodestyle version.
I (at least at first) coded by looking at the pycodestyle rule definitions / PEP8 recommendations rather than the pycodestyle code. Because of this I actually check whether a line is within a function/class rather than just looking at the indentation (see for example how [E303](https://www.flake8rules.com/rules/E303.html) or [PEP8](https://peps.python.org/pep-0008/#blank-lines) formulate the rule).

This results in the snippet below being a violation for pycodestyle but not for what I did :
```python
if True:
    a = 1


    a = 2
```
There is no such test example within the pycodestyle tests, so it's not clear whether this is a desired behavior or not.

Matching the pycodestyle results (and modifying the rule definition) would be easy (and simplify the implementation), and I suppose it is the desired behavior to match the previous implementation, but I would like confirmation before doing it.

---

_Marked ready for review by @hoel-bagard on 2023-11-19 16:42_

---

_Comment by @allisonkarlitskaya on 2023-11-20 14:49_

> There is no such test example within the pycodestyle tests, so it's not clear whether this is a desired behavior or not.

This is a pretty close match: https://github.com/PyCQA/pycodestyle/blob/main/testing/data/E30.py#L65

It's not an `if:` at toplevel but rather a function, but it clearly indicates the E303 is intended to be used for more than just spacing between top-level items.

---

_Comment by @hoel-bagard on 2023-11-20 15:33_

Thank you for your answer.
It's a close match, yes, and what I implemented matches the pycodestyle results on that example.

My issue isn't whether E303 should apply only to top-level items or not (it applies everywhere). It's whether 2 blank lines should be allowed only to top-level items, or to any item that is not within a class/function.
([The E303 description](https://www.flake8rules.com/rules/E303.html) does not mention top-level)

This seems like a very arbitrary choice to me, and [PEP8](https://peps.python.org/pep-0008/#blank-lines) does not provide a recommendation on that front.

The situation can be sumed-up like this:
| Snipet                                                  | Pycodestyle | Current PR implementation |
|:-------------------------------------------------------:|:-----------:|:-------------------------:|
| <pre>if True:<br>    a = 1<br><br><br><br>    a = 2</pre> | Violation   | Violation                 |
| <pre>if True:<br>    a = 1<br><br><br>    a = 2</pre>     | Violation   | OK                        |
| <pre>if True:<br>    a = 1<br><br>    a = 2</pre>         | OK          | OK                        |

(if the `if` is replaced by a `def`, then both implementation would have identical results)

---

_Comment by @allisonkarlitskaya on 2023-11-21 10:07_

> My issue isn't whether E303 should apply only to top-level items or not (it applies everywhere). It's whether 2 blank lines should be allowed only to top-level items, or to any item that is not within a class/function.
> ([The E303 description](https://www.flake8rules.com/rules/E303.html) does not mention top-level)

The E303 description talks about spaces between functions and methods.  That's what I meant by top-level, although technically a method defined inside of a class isn't really top-level.

What really motivated me to use the "top level" language though is that when I went hunting for hints about the pycodestyle implementation and how it changed over time, I found this commit from a decade and a half ago by @florentx:

https://github.com/PyCQA/pycodestyle/commit/fc059fb7a07997df83a232018fcdc2907648aa99

```python
...
    elif (max_blank_lines > 2 or
         (indent_level and max_blank_lines == 2)):
       return 0, "E303 too many blank lines (%d)" % max_blank_lines
```

Which reads as E303 in case of "more than 2 lines anywhere, or 2 lines at any (non-zero-)indented level".  That meshes with the English language description of the change:

> Reserve "2 blank lines" for module-level logical blocks (E303)

Note: this happens to be the commit that also added the test case, linked above, in its original form:

```python
def a():
    print


    # comment

    print
```

So by the code, and by the description of the change, and by the added test case, it seems clear that the intent is that two newlines should only ever appear at top level (meaning: unintended), otherwise E303 is the correct error to issue for that.



---

_Comment by @allisonkarlitskaya on 2023-11-21 14:37_

With https://github.com/PyCQA/pycodestyle/pull/1215 merged, the "top-levelness instead of class/def" approach now seems indisputably correct.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:51 on 2023-11-28 02:55_

You can derive `Default` if you flip `is_first_logical_line` to `is_not_first_logical_line`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:68 on 2023-11-28 02:58_

I would either make these module level constants:

```
const BLANK_LINES_TOP_LEVEL: u32 = 2;
const BLANK_LINES_METHOD_LEVEL: u32 = 1;
```

or use an enum

```rust
#[derive(Copy, Clone, Debug)]
enum BlankLines {
	TopLevel,
	Method
}

impl BlankLines {
	const fn count(self) -> u32 {
		match self {
			BlankLines::TopLevel => 2,
			BlankLines::Method => 1
		}
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:356 on 2023-11-28 02:59_

Nit
```suggestion
    matches!(token, Some(TokenKind::Class | TokenKind::Def | TokenKind::Async))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:378 on 2023-11-28 03:01_

Nit: We could implement this as a method on `BlankLinesTrackingVars` instead (and rename the struct to `BlankLinesChecker`

```rust
impl BlankLinesChecker {
	pub(crate) fn check_line(&mut self, line: &LogicalLine, prev_indent_level: Option<usize>, indent_level: usize, ...) {
		...
	}
}
```

and you would use it

```
let mut blank_lines_checker = BlankLinesChecker::default();
...

for line in logical_lines {
	blank_lines_checker.check_line(line, ...);
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:384 on 2023-11-28 03:04_

Is my assumption correct that `is_in_class` and `follows_comments_after_class` should never be `true` at the same time? If so, could we use a single enum instead that has three states: `InClass(indent_level)`, `CommentAfterClass`, `OutsideClass` (default)? It may even be possible to store the `indent_level` inside of the state variable. 

Encoding the different states more explicitly helps to avoid invalid states when we forget to reset some variable 

The same applies for `is_in_fn`. We can reuse the same enum if you give it more generic names (`Inside(indent_level)`, `CommentAfter`, `Outside`)

We could even go one step further and have a `Follows` struct with a `FollowKind` because it is never possible to follow a class and function at the same time.

```rust
#[derive(Copy, Clone, Debug)]
enum Follows {
    Class(FollowKind),
    Function(FollowKind)
}

#[derive(Copy, Clone, Debug)]
enum FollowKind {
    Inside(indent_level),
    CommentAfter,
    Outside
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:344 on 2023-11-28 03:13_

This method seems to always be called with `Some(line)`. Thus, using `Option` is unnecessary
```suggestion
fn is_docstring(line: &LogicalLine) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:415 on 2023-11-28 03:18_

Nit: I think the code is easier to read when you remove the loop to reduce the nestin. 

```rust
let first_token = line.tokens().find(|token| !matches!(token.kind, TokenKind::Indent | TokenKind::Dedent));

// The body of the loop
```

It makes it clear which tokens are skipped and that the body itself is only executed once (it also removes the need for the `break` statements in the `match`` below)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:22 on 2023-11-28 03:29_

I think we can create an enum instead of using 3 boolean because it is only ever possible that one of those is true, but not all of them at once:

```rust
#[derive(Copy, Clone, Debug, Default)]
enum Follows {
    #[default]
    Other,
    Decorator,
    Def,
    Docstring
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:480 on 2023-11-28 03:37_

Would it be possible to compute the blank lines ad-hoc? Storing them for every line seems expensive, especially because they're only needed for fixes. 

---

_@MichaReiser reviewed on 2023-11-28 03:39_

Thanks for working on this (again). 

I left a few comments on how I think the state could be simplified which should make it easier to reason about and review the code changes. 

I still feel somewhat uncomfortable to integrate this into logical lines. Are there examples where it is important that the rule runs on logical lines? If so, then using logical lines is the right thing to do. If not, then it might be overkill and it might be easier to just copy over of what's needed from LogicalLine into a BlankLine checker. 

---

_Comment by @MichaReiser on 2023-11-28 04:52_

I guess I need to get a better understanding when this rule is supposed to run. What I'm wondering is if it is sufficient to only test for empty lines after each Newline (which indicates a new logical line) and use `Indent` and `Dedent` to detect the end of a class / function. If that happens to be sufficient, it might be easier to implement it without using logical lines. If there are cases where this is not sufficient, than using logical lines is the right choice. 

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:384 on 2023-11-28 05:46_

I think your assumption is correct, I'll try to switch to using an enum.

I'm not sure if class and function status can be condensed into a single enum (since it's possible to be in both a class and a function at the same time).

---

_@hoel-bagard reviewed on 2023-11-28 05:46_

---

_Comment by @hoel-bagard on 2023-11-28 06:00_

Thank you for your review.

I don't know if using logical lines is required. I was under the impression that I could only use physical or logical lines, and with physical lines ruled out there was only logical lines left. As long as it's possible to check for indents/detents, def/async def, class and decorator tokens, I suppose it might be possible to not use logical lines.
I don't have a good idea of what the alternative would be, so it's a bit hard to say.

---

_@hoel-bagard reviewed on 2023-11-28 06:04_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:480 on 2023-11-28 06:04_

There isn't a way to go back to previous logical lines, right ?
Then the only option (afaik) would be to work with text and to go back until finding the end of the previous non-blank line. My understanding is that it would be slow, however if that is run only for fixes, then maybe it's fine.

---

_Comment by @MichaReiser on 2023-12-01 02:11_

The alternative is that you create a function similar to `check_logical_lines` that accepts all tokens. It would be interesting to understand if doing so would require duplicating a lot of logic (in which case it isn't worth it) or if it may even be simpler because you don't need to make it fit into the logical lines structure. 

Do you want to play around with it to see how it feels? Feel free to publish it as a separate (or stacked) PR. I don't want that you have to undo the work if logical lines was the right place to begin with. 

See

https://github.com/astral-sh/ruff/blob/c38617fa27feff7e9024fbee72354bebc3759b47/crates/ruff_linter/src/checkers/logical_lines.rs#L33-L38

---

_Comment by @hoel-bagard on 2023-12-02 11:46_

Would the main goal be to not store the number of preceding blank lines/characters for each line ?

---

_Comment by @MichaReiser on 2023-12-04 03:45_

> Would the main goal be to not store the number of preceding blank lines/characters for each line ?

The main goal is to understand if the rules fit into the logical lines or not. To me, it seems they don't really re-use the logic of logical lines, but it adds complexity to the logical line data structures (making it harder to understand and maintain). Meaning, the overall implementation may be more complicated than it has to. It also has the downside that running the logical line rules only (excluding blank lines) now pays for some of the blank line's runtime cost without using it. . 

---

_Comment by @hoel-bagard on 2023-12-10 12:17_

Ok, I'll try to make the change.

---

_Comment by @minusf on 2023-12-13 16:26_

very happy to see E303 coming, thank you all! my ocd will thank you <3

---

_Comment by @Avasam on 2023-12-14 07:47_

As a note, autofixes for all these rules would cover `E3` fixes of Autopep8

---

_Comment by @hoel-bagard on 2023-12-16 08:03_

@MichaReiser I've made a version that does not use logical lines [here](https://github.com/astral-sh/ruff/pull/9266).

Overall I feel like the added logic isn't too complex, especially since it removes some functions from the logical lines version. I still need to operate on logical lines (since pycodestyle did), so I'm still building them, but I only keep track of what I actually need.

---

_Comment by @codspeed-hq[bot] on 2023-12-16 08:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:add_blank_lines_E30_V2)

### Merging #8720 will **not alter performance**

<sub>Comparing <code>hoel-bagard:add_blank_lines_E30_V2</code> (29849d5) with <code>main</code> (0029b4f)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Converted to draft by @hoel-bagard on 2023-12-25 00:17_

---

_Closed by @hoel-bagard on 2024-02-18 10:03_

---
