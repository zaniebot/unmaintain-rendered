```yaml
number: 9072
title: Add fix for unexpected-spaces-around-keyword-parameter-equals
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charkie/E251
created_at: 2023-12-09T18:04:53Z
updated_at: 2023-12-09T18:22:20Z
url: https://github.com/astral-sh/ruff/pull/9072
synced_at: 2026-01-10T23:40:55Z
```

# Add fix for unexpected-spaces-around-keyword-parameter-equals

---

_Pull request opened by @charliermarsh on 2023-12-09 18:04_

Closes https://github.com/astral-sh/ruff/issues/9066.

---

_Label `autofix` added by @charliermarsh on 2023-12-09 18:05_

---

_Merged by @charliermarsh on 2023-12-09 18:15_

---

_Closed by @charliermarsh on 2023-12-09 18:15_

---

_Branch deleted on 2023-12-09 18:15_

---

_Comment by @github-actions[bot] on 2023-12-09 18:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2174 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/ec451f2be7e4f9a8290f32f4c7a51ff19f757fce/tests/unit/commands/init/test_cli.py#L799'>tests/unit/commands/init/test_cli.py:799:34:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/aws/aws-sam-cli/blob/ec451f2be7e4f9a8290f32f4c7a51ff19f757fce/tests/unit/commands/init/test_cli.py#L799'>tests/unit/commands/init/test_cli.py:799:34:</a> E251 [*] Unexpected spaces around keyword / parameter equals
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2086 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/data/transform_markers.py#L22'>examples/basic/data/transform_markers.py:22:17:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/data/transform_markers.py#L22'>examples/basic/data/transform_markers.py:22:17:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/data/transform_markers.py#L22'>examples/basic/data/transform_markers.py:22:19:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/data/transform_markers.py#L22'>examples/basic/data/transform_markers.py:22:19:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/words_and_plots.py#L31'>examples/basic/layouts/words_and_plots.py:31:21:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/words_and_plots.py#L31'>examples/basic/layouts/words_and_plots.py:31:21:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/words_and_plots.py#L31'>examples/basic/layouts/words_and_plots.py:31:23:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/basic/layouts/words_and_plots.py#L31'>examples/basic/layouts/words_and_plots.py:31:23:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L10'>examples/integration/layout/axis_label_extra_x_ranges.py:10:59:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L10'>examples/integration/layout/axis_label_extra_x_ranges.py:10:59:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L10'>examples/integration/layout/axis_label_extra_x_ranges.py:10:61:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L10'>examples/integration/layout/axis_label_extra_x_ranges.py:10:61:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L13'>examples/integration/layout/axis_label_extra_x_ranges.py:13:59:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L13'>examples/integration/layout/axis_label_extra_x_ranges.py:13:59:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L13'>examples/integration/layout/axis_label_extra_x_ranges.py:13:61:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L13'>examples/integration/layout/axis_label_extra_x_ranges.py:13:61:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L16'>examples/integration/layout/axis_label_extra_x_ranges.py:16:59:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L16'>examples/integration/layout/axis_label_extra_x_ranges.py:16:59:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L16'>examples/integration/layout/axis_label_extra_x_ranges.py:16:61:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L16'>examples/integration/layout/axis_label_extra_x_ranges.py:16:61:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L7'>examples/integration/layout/axis_label_extra_x_ranges.py:7:57:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L7'>examples/integration/layout/axis_label_extra_x_ranges.py:7:57:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L7'>examples/integration/layout/axis_label_extra_x_ranges.py:7:59:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/integration/layout/axis_label_extra_x_ranges.py#L7'>examples/integration/layout/axis_label_extra_x_ranges.py:7:59:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/interaction/js_callbacks/color_sliders.py#L27'>examples/interaction/js_callbacks/color_sliders.py:27:19:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/interaction/js_callbacks/color_sliders.py#L27'>examples/interaction/js_callbacks/color_sliders.py:27:19:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/interaction/js_callbacks/color_sliders.py#L27'>examples/interaction/js_callbacks/color_sliders.py:27:21:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/interaction/js_callbacks/color_sliders.py#L27'>examples/interaction/js_callbacks/color_sliders.py:27:21:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L46'>examples/models/calendars.py:46:13:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L46'>examples/models/calendars.py:46:13:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L46'>examples/models/calendars.py:46:26:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L46'>examples/models/calendars.py:46:26:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L47'>examples/models/calendars.py:47:14:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L47'>examples/models/calendars.py:47:14:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L47'>examples/models/calendars.py:47:26:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L47'>examples/models/calendars.py:47:26:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L48'>examples/models/calendars.py:48:19:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L48'>examples/models/calendars.py:48:19:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L48'>examples/models/calendars.py:48:26:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L48'>examples/models/calendars.py:48:26:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L49'>examples/models/calendars.py:49:24:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L49'>examples/models/calendars.py:49:24:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L49'>examples/models/calendars.py:49:26:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L49'>examples/models/calendars.py:49:26:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L56'>examples/models/calendars.py:56:22:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L56'>examples/models/calendars.py:56:22:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L56'>examples/models/calendars.py:56:25:</a> E251 Unexpected spaces around keyword / parameter equals
... 2039 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+0 -0 violations, +38 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/__init__.py#L200'>selfdrive/car/__init__.py:200:49:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/__init__.py#L200'>selfdrive/car/__init__.py:200:49:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/__init__.py#L200'>selfdrive/car/__init__.py:200:51:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/__init__.py#L200'>selfdrive/car/__init__.py:200:51:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:108:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:108:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:110:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:110:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:133:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/commaai/openpilot/blob/aa744e84373ecacbbb45382e1a0e4cdd3d5c05fb/selfdrive/car/subaru/subarucan.py#L19'>selfdrive/car/subaru/subarucan.py:19:133:</a> E251 [*] Unexpected spaces around keyword / parameter equals
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L12'>blinksync/forms.py:12:91:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L12'>blinksync/forms.py:12:91:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L12'>blinksync/forms.py:12:93:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/fronzbot/blinkpy/blob/a8841612641f17527c5b3f43bbf034b6fe7ba743/blinksync/forms.py#L12'>blinksync/forms.py:12:93:</a> E251 [*] Unexpected spaces around keyword / parameter equals
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +44 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L164'>examples/example_bulkinsert_json.py:164:72:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L164'>examples/example_bulkinsert_json.py:164:72:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L164'>examples/example_bulkinsert_json.py:164:74:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L164'>examples/example_bulkinsert_json.py:164:74:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:43:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:43:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:45:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:45:</a> E251 [*] Unexpected spaces around keyword / parameter equals
- <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:69:</a> E251 Unexpected spaces around keyword / parameter equals
+ <a href='https://github.com/milvus-io/pymilvus/blob/f12777b845510baeb611e4e2f69582ab22a759a3/examples/example_bulkinsert_json.py#L284'>examples/example_bulkinsert_json.py:284:69:</a> E251 [*] Unexpected spaces around keyword / parameter equals
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E251 | 2174 | 0 | 0 | 2174 | 0 |

</p>
</details>




---
