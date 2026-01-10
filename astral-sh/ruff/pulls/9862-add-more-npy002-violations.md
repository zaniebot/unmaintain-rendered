```yaml
number: 9862
title: Add more NPY002 violations
type: pull_request
state: merged
author: jack-mcivor
labels:
  - rule
assignees: []
merged: true
base: main
head: add-more-npy002-violations
created_at: 2024-02-06T20:11:46Z
updated_at: 2024-02-07T14:54:12Z
url: https://github.com/astral-sh/ruff/pull/9862
synced_at: 2026-01-10T22:57:09Z
```

# Add more NPY002 violations

---

_Pull request opened by @jack-mcivor on 2024-02-06 20:11_

Fixes #9861

---

_Comment by @github-actions[bot] on 2024-02-06 20:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+63 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+63 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py#L7'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L10'>examples/advanced/integration/d3-voronoi.py:10:6:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L11'>examples/advanced/integration/d3-voronoi.py:11:6:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L18'>examples/basic/annotations/band.py:18:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L11'>examples/basic/axes/bounds.py:11:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L12'>examples/basic/axes/bounds.py:12:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L13'>examples/basic/axes/bounds.py:13:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L47'>examples/basic/data/ajax_source.py:47:22:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L53'>examples/basic/data/ajax_source.py:53:28:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/color_mappers.py#L17'>examples/basic/data/color_mappers.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L56'>examples/basic/data/server_sent_events_source.py:56:35:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L15'>examples/basic/scatters/color_scatter.py:15:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L16'>examples/basic/scatters/color_scatter.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L17'>examples/basic/scatters/color_scatter.py:17:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/image_url.py#L16'>examples/basic/scatters/image_url.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/image_url.py#L17'>examples/basic/scatters/image_url.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markers.py#L26'>examples/basic/scatters/markers.py:26:15:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markers.py#L26'>examples/basic/scatters/markers.py:26:30:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L10'>examples/interaction/js_callbacks/customjs_for_range_update.py:10:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L11'>examples/interaction/js_callbacks/customjs_for_range_update.py:11:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L9'>examples/interaction/js_callbacks/customjs_for_range_update.py:9:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L41'>examples/interaction/js_callbacks/js_on_event.py:41:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L42'>examples/interaction/js_callbacks/js_on_event.py:42:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L43'>examples/interaction/js_callbacks/js_on_event.py:43:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L7'>examples/interaction/tools/persistent_selections.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L8'>examples/interaction/tools/persistent_selections.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L9'>examples/interaction/tools/persistent_selections.py:9:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tap_inspect_keymod.py#L8'>examples/interaction/tools/tap_inspect_keymod.py:8:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tool_overlays.py#L6'>examples/interaction/tools/tool_overlays.py:6:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tool_overlays.py#L7'>examples/interaction/tools/tool_overlays.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L16'>examples/models/toolbars.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L17'>examples/models/toolbars.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L18'>examples/models/toolbars.py:18:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L15'>examples/models/toolbars2.py:15:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L16'>examples/models/toolbars2.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L17'>examples/models/toolbars2.py:17:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/transform_jitter.py#L17'>examples/models/transform_jitter.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L14'>examples/output/apis/autoload_static_flask.py:14:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L15'>examples/output/apis/autoload_static_flask.py:15:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L16'>examples/output/apis/autoload_static_flask.py:16:13:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L7'>examples/output/apis/server_session/bokeh_app.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L8'>examples/output/apis/server_session/bokeh_app.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L9'>examples/output/apis/server_session/bokeh_app.py:9:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/jupyter/ipywidgets/ipyvolume_camera.py#L10'>examples/output/jupyter/ipywidgets/ipyvolume_camera.py:10:11:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/jupyter/ipywidgets/ipyvolume_camera.py#L19'>examples/output/jupyter/ipywidgets/ipyvolume_camera.py:19:15:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/marker_compare.py#L16'>examples/output/webgl/marker_compare.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/marker_compare.py#L17'>examples/output/webgl/marker_compare.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/dynamic_color_mapping.py#L13'>examples/plotting/dynamic_color_mapping.py:13:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/dynamic_color_mapping.py#L14'>examples/plotting/dynamic_color_mapping.py:14:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY002 | 63 | 63 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+63 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+63 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py#L7'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color_and_size.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L10'>examples/advanced/integration/d3-voronoi.py:10:6:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L11'>examples/advanced/integration/d3-voronoi.py:11:6:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L18'>examples/basic/annotations/band.py:18:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L11'>examples/basic/axes/bounds.py:11:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L12'>examples/basic/axes/bounds.py:12:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/bounds.py#L13'>examples/basic/axes/bounds.py:13:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L47'>examples/basic/data/ajax_source.py:47:22:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L53'>examples/basic/data/ajax_source.py:53:28:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/color_mappers.py#L17'>examples/basic/data/color_mappers.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L56'>examples/basic/data/server_sent_events_source.py:56:35:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L15'>examples/basic/scatters/color_scatter.py:15:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L16'>examples/basic/scatters/color_scatter.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/color_scatter.py#L17'>examples/basic/scatters/color_scatter.py:17:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/image_url.py#L16'>examples/basic/scatters/image_url.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/image_url.py#L17'>examples/basic/scatters/image_url.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markers.py#L26'>examples/basic/scatters/markers.py:26:15:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/scatters/markers.py#L26'>examples/basic/scatters/markers.py:26:30:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L10'>examples/interaction/js_callbacks/customjs_for_range_update.py:10:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L11'>examples/interaction/js_callbacks/customjs_for_range_update.py:11:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_range_update.py#L9'>examples/interaction/js_callbacks/customjs_for_range_update.py:9:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L41'>examples/interaction/js_callbacks/js_on_event.py:41:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L42'>examples/interaction/js_callbacks/js_on_event.py:42:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L43'>examples/interaction/js_callbacks/js_on_event.py:43:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L7'>examples/interaction/tools/persistent_selections.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L8'>examples/interaction/tools/persistent_selections.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/persistent_selections.py#L9'>examples/interaction/tools/persistent_selections.py:9:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tap_inspect_keymod.py#L8'>examples/interaction/tools/tap_inspect_keymod.py:8:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tool_overlays.py#L6'>examples/interaction/tools/tool_overlays.py:6:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/tool_overlays.py#L7'>examples/interaction/tools/tool_overlays.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L16'>examples/models/toolbars.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L17'>examples/models/toolbars.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars.py#L18'>examples/models/toolbars.py:18:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L15'>examples/models/toolbars2.py:15:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L16'>examples/models/toolbars2.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/toolbars2.py#L17'>examples/models/toolbars2.py:17:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/transform_jitter.py#L17'>examples/models/transform_jitter.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L14'>examples/output/apis/autoload_static_flask.py:14:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L15'>examples/output/apis/autoload_static_flask.py:15:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/autoload_static_flask.py#L16'>examples/output/apis/autoload_static_flask.py:16:13:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L7'>examples/output/apis/server_session/bokeh_app.py:7:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L8'>examples/output/apis/server_session/bokeh_app.py:8:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_session/bokeh_app.py#L9'>examples/output/apis/server_session/bokeh_app.py:9:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/jupyter/ipywidgets/ipyvolume_camera.py#L10'>examples/output/jupyter/ipywidgets/ipyvolume_camera.py:10:11:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/jupyter/ipywidgets/ipyvolume_camera.py#L19'>examples/output/jupyter/ipywidgets/ipyvolume_camera.py:19:15:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/marker_compare.py#L16'>examples/output/webgl/marker_compare.py:16:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/marker_compare.py#L17'>examples/output/webgl/marker_compare.py:17:5:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/dynamic_color_mapping.py#L13'>examples/plotting/dynamic_color_mapping.py:13:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/dynamic_color_mapping.py#L14'>examples/plotting/dynamic_color_mapping.py:14:9:</a> NPY002 Replace legacy `np.random.random` call with `np.random.Generator`
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY002 | 63 | 63 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-07 14:51_

---

_@charliermarsh approved on 2024-02-07 14:54_

---

_Label `rule` added by @charliermarsh on 2024-02-07 14:54_

---

_Merged by @charliermarsh on 2024-02-07 14:54_

---

_Closed by @charliermarsh on 2024-02-07 14:54_

---
