```yaml
number: 14407
title: "Implement black's `bad-numeric-literal-format`"
type: pull_request
state: closed
author: Lokejoke
labels: []
assignees: []
base: main
head: bad-numeric-literal-format
created_at: 2024-11-17T19:22:30Z
updated_at: 2024-11-19T12:31:18Z
url: https://github.com/astral-sh/ruff/pull/14407
synced_at: 2026-01-12T15:55:47Z
```

# Implement black's `bad-numeric-literal-format`

---

_@Lokejoke_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements "ports" [black's](https://github.com/psf/black/blob/main/src/black/numerics.py) numeric formatting.
Does not enforce any underscore formatting.

## Test Plan

`cargo test`

<!-- How was it tested? -->

## Notes
- Not sure what plugin, name and code to use:
  - Temporarily [`wps-light`] `bad-numeric-literal-format` (`WPS987`)
- Add formatting style settings?
- Split the rule and address each as a separate violation?


---

_Renamed from "Implement `Bad numeric literal format`" to "Implement black's `bad-numeric-literal-format`" by @Lokejoke on 2024-11-17 19:28_

---

_Comment by @github-actions[bot] on 2024-11-17 19:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+124 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+123 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L23'>examples/advanced/integration/d3-voronoi.py:23:45:</a> WPS987 [*] The numeric literal `.5` has a bad format. Consider replacing it with `0.5`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/dashboard.py#L37'>examples/basic/layouts/dashboard.py:37:51:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/dashboard.py#L38'>examples/basic/layouts/dashboard.py:38:52:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/dashboard.py#L39'>examples/basic/layouts/dashboard.py:39:52:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/dashboard.py#L40'>examples/basic/layouts/dashboard.py:40:52:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/sizing_mode_multiple.py#L16'>examples/basic/layouts/sizing_mode_multiple.py:16:47:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/sizing_mode_multiple.py#L17'>examples/basic/layouts/sizing_mode_multiple.py:17:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/sizing_mode_multiple.py#L18'>examples/basic/layouts/sizing_mode_multiple.py:18:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/sizing_mode_multiple.py#L19'>examples/basic/layouts/sizing_mode_multiple.py:19:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L6'>examples/integration/glyphs/categorical_multi_glyphs.py:6:13:</a> WPS987 [*] The numeric literal `1.` has a bad format. Consider replacing it with `1.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L6'>examples/integration/glyphs/categorical_multi_glyphs.py:6:17:</a> WPS987 [*] The numeric literal `2.` has a bad format. Consider replacing it with `2.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L6'>examples/integration/glyphs/categorical_multi_glyphs.py:6:21:</a> WPS987 [*] The numeric literal `3.` has a bad format. Consider replacing it with `3.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L6'>examples/integration/glyphs/categorical_multi_glyphs.py:6:25:</a> WPS987 [*] The numeric literal `4.` has a bad format. Consider replacing it with `4.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L7'>examples/integration/glyphs/categorical_multi_glyphs.py:7:13:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L7'>examples/integration/glyphs/categorical_multi_glyphs.py:7:17:</a> WPS987 [*] The numeric literal `.2` has a bad format. Consider replacing it with `0.2`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L7'>examples/integration/glyphs/categorical_multi_glyphs.py:7:21:</a> WPS987 [*] The numeric literal `.3` has a bad format. Consider replacing it with `0.3`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/integration/glyphs/categorical_multi_glyphs.py#L7'>examples/integration/glyphs/categorical_multi_glyphs.py:7:25:</a> WPS987 [*] The numeric literal `.4` has a bad format. Consider replacing it with `0.4`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_widgets.py#L21'>examples/interaction/js_callbacks/customjs_for_widgets.py:21:49:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_change.py#L21'>examples/interaction/js_callbacks/js_on_change.py:21:49:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider.py#L25'>examples/interaction/js_callbacks/slider.py:25:47:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider.py#L26'>examples/interaction/js_callbacks/slider.py:26:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider.py#L27'>examples/interaction/js_callbacks/slider.py:27:51:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/slider.py#L28'>examples/interaction/js_callbacks/slider.py:28:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/widgets/range_slider.py#L4'>examples/interaction/widgets/range_slider.py:4:63:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/widgets/slider.py#L4'>examples/interaction/widgets/slider.py:4:48:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/dateaxis.py#L28'>examples/models/dateaxis.py:28:69:</a> WPS987 [*] The numeric literal `5000.` has a bad format. Consider replacing it with `5000.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L19'>examples/models/glyphs.py:19:19:</a> WPS987 [*] The numeric literal `.09` has a bad format. Consider replacing it with `0.09`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L19'>examples/models/glyphs.py:19:25:</a> WPS987 [*] The numeric literal `.12` has a bad format. Consider replacing it with `0.12`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L19'>examples/models/glyphs.py:19:30:</a> WPS987 [*] The numeric literal `.0` has a bad format. Consider replacing it with `0.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L19'>examples/models/glyphs.py:19:34:</a> WPS987 [*] The numeric literal `.12` has a bad format. Consider replacing it with `0.12`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L19'>examples/models/glyphs.py:19:39:</a> WPS987 [*] The numeric literal `.09` has a bad format. Consider replacing it with `0.09`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L20'>examples/models/glyphs.py:20:19:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L20'>examples/models/glyphs.py:20:23:</a> WPS987 [*] The numeric literal `.02` has a bad format. Consider replacing it with `0.02`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L20'>examples/models/glyphs.py:20:28:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L20'>examples/models/glyphs.py:20:32:</a> WPS987 [*] The numeric literal `.02` has a bad format. Consider replacing it with `0.02`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/glyphs.py#L20'>examples/models/glyphs.py:20:38:</a> WPS987 [*] The numeric literal `.1` has a bad format. Consider replacing it with `0.1`.
+ examples/models/structure/ModelStructureExample.ipynb:cell 4:5:50: WPS987 [*] The numeric literal `.5` has a bad format. Consider replacing it with `0.5`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/components_themed.py#L44'>examples/output/apis/components_themed.py:44:32:</a> WPS987 [*] The numeric literal `.3` has a bad format. Consider replacing it with `0.3`.
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 7:6:18: WPS987 [*] The numeric literal `0.` has a bad format. Consider replacing it with `0.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L18'>examples/output/webgl/clustering.py:18:59:</a> WPS987 [*] The numeric literal `.5` has a bad format. Consider replacing it with `0.5`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L18'>examples/output/webgl/clustering.py:18:69:</a> WPS987 [*] The numeric literal `.04` has a bad format. Consider replacing it with `0.04`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L19'>examples/output/webgl/clustering.py:19:54:</a> WPS987 [*] The numeric literal `.05` has a bad format. Consider replacing it with `0.05`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L28'>examples/output/webgl/clustering.py:28:31:</a> WPS987 [*] The numeric literal `.2` has a bad format. Consider replacing it with `0.2`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/webgl/clustering.py#L32'>examples/output/webgl/clustering.py:32:48:</a> WPS987 [*] The numeric literal `.9` has a bad format. Consider replacing it with `0.9`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L21'>examples/plotting/glyphs.py:21:19:</a> WPS987 [*] The numeric literal `.09` has a bad format. Consider replacing it with `0.09`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L21'>examples/plotting/glyphs.py:21:25:</a> WPS987 [*] The numeric literal `.12` has a bad format. Consider replacing it with `0.12`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L21'>examples/plotting/glyphs.py:21:30:</a> WPS987 [*] The numeric literal `.0` has a bad format. Consider replacing it with `0.0`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L21'>examples/plotting/glyphs.py:21:34:</a> WPS987 [*] The numeric literal `.12` has a bad format. Consider replacing it with `0.12`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/glyphs.py#L21'>examples/plotting/glyphs.py:21:39:</a> WPS987 [*] The numeric literal `.09` has a bad format. Consider replacing it with `0.09`.
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/corporate/lib/stripe.py#L351'>corporate/lib/stripe.py:351:23:</a> WPS987 [*] The numeric literal `100.` has a bad format. Consider replacing it with `100.0`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| WPS987 | 124 | 124 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-11-18 02:04_

I think we support this formatting in `ruff format` already. What's the motivation for introducing a separate lint rule?

---

_Closed by @Lokejoke on 2024-11-18 08:19_

---

_Branch deleted on 2024-11-19 12:30_

---

_Branch restored on 2024-11-19 12:31_

---
