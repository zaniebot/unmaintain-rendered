```yaml
number: 18092
title: "[ty] Do not look up `__init__` on instances"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-332
created_at: 2025-05-14T13:25:38Z
updated_at: 2025-05-14T13:33:44Z
url: https://github.com/astral-sh/ruff/pull/18092
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Do not look up `__init__` on instances

---

_Pull request opened by @sharkdp on 2025-05-14 13:25_

## Summary

Dunder methods are never looked up on instances. We do this implicitly in `try_call_dunder`, but the corresponding flag was missing in the instance-construction code where we use `member_lookup_with_policy` directly.

fixes https://github.com/astral-sh/ty/issues/322

## Test Plan

Added regression test.


---

_Review requested from @carljm by @sharkdp on 2025-05-14 13:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-14 13:25_

---

_Review requested from @dcreager by @sharkdp on 2025-05-14 13:25_

---

_Label `ty` added by @sharkdp on 2025-05-14 13:25_

---

_@sharkdp reviewed on 2025-05-14 13:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1471 on 2025-05-14 13:26_

This test was passing before the change here. I originally thought this might be the problem. Seems like a good thing to keep anyway.

---

_@AlexWaygood approved on 2025-05-14 13:28_

---

_Comment by @github-actions[bot] on 2025-05-14 13:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[too-many-positional-arguments] schema_salad/jsonld_context.py:189:52: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 401 diagnostics
+ Found 400 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[too-many-positional-arguments] tests/test_provenance.py:27:17: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:28:18: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:29:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:30:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:31:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:32:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:33:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:34:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_provenance.py:35:18: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 325 diagnostics
+ Found 316 diagnostics

altair (https://github.com/vega/altair)
- error[too-many-positional-arguments] tests/examples_arguments_syntax/bar_chart_faceted_compact.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/bar_chart_with_highlighted_segment.py:12:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/bar_chart_with_single_threshold.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/dendrogram.py:91:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/dendrogram.py:91:37: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/dendrogram.py:92:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/dendrogram.py:92:37: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/dendrogram.py:120:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/deviation_ellipses.py:59:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/deviation_ellipses.py:59:59: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/distributions_and_medians_of_likert_scale_ratings.py:13:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/distributions_and_medians_of_likert_scale_ratings.py:50:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/diverging_stacked_bar_chart.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/donut_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/falkensee.py:57:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/falkensee.py:58:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/gantt_chart.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/grouped_bar_chart2.py:11:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/grouped_bar_chart_overlapping_bars.py:11:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/horizon_graph.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/interactive_column_selection.py:20:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/interactive_column_selection.py:21:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/isotype.py:12:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/isotype_emoji.py:12:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/isotype_grid.py:10:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/layered_chart_bar_mark.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/layered_histogram.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/line_chart_with_arrows.py:16:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/multiline_tooltip.py:20:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/multiline_tooltip.py:21:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_arguments_syntax/multiline_tooltip.py:21:22: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/multiline_tooltip_standard.py:16:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/multiline_tooltip_standard.py:17:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_arguments_syntax/multiline_tooltip_standard.py:17:22: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/percentage_of_total.py:11:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/pie_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/pie_chart_with_labels.py:14:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/polar_bar_chart.py:15:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/polar_bar_chart.py:27:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/polar_bar_chart.py:37:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/poly_fit_regression.py:17:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/pyramid.py:12:19: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/radial_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_histogram.py:25:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_labels.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_layered_histogram.py:16:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_loess.py:15:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_shaded_area.py:13:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_shaded_area.py:18:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/scatter_with_shaded_area.py:24:19: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/select_detail.py:24:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/select_detail.py:31:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_arguments_syntax/select_detail.py:32:27: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_arguments_syntax/select_detail.py:33:27: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_arguments_syntax/simple_bar_chart.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/simple_heatmap.py:16:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/simple_line_chart.py:14:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/simple_scatter_with_errorbars.py:18:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/slider_cutoff.py:13:19: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/stem_and_leaf.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/top_k_letters.py:26:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/us_employment.py:12:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/waterfall_chart.py:28:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/wheat_wages.py:33:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/wilkinson-dot-plot.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_arguments_syntax/window_rank.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/bar_chart_faceted_compact.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/bar_chart_with_single_threshold.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/deviation_ellipses.py:59:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_methods_syntax/deviation_ellipses.py:59:59: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_methods_syntax/distributions_and_medians_of_likert_scale_ratings.py:13:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/distributions_and_medians_of_likert_scale_ratings.py:50:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/diverging_stacked_bar_chart.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/donut_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/falkensee.py:57:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/falkensee.py:58:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/grouped_bar_chart2.py:11:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/grouped_bar_chart_overlapping_bars.py:11:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/horizon_graph.py:10:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/interactive_column_selection.py:20:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_methods_syntax/interactive_column_selection.py:21:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_methods_syntax/isotype.py:12:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/isotype_emoji.py:12:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/isotype_grid.py:10:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/layered_histogram.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/multiline_tooltip.py:20:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_methods_syntax/multiline_tooltip.py:21:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_methods_syntax/multiline_tooltip.py:21:22: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_methods_syntax/multiline_tooltip_standard.py:16:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_methods_syntax/multiline_tooltip_standard.py:17:5: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_methods_syntax/multiline_tooltip_standard.py:17:22: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_methods_syntax/percentage_of_total.py:11:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/pie_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/pie_chart_with_labels.py:14:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/polar_bar_chart.py:15:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/polar_bar_chart.py:27:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/polar_bar_chart.py:37:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/poly_fit_regression.py:17:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/pyramid.py:12:19: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/radial_chart.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/scatter_with_layered_histogram.py:16:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/select_detail.py:24:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/select_detail.py:31:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/examples_methods_syntax/select_detail.py:32:27: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] tests/examples_methods_syntax/select_detail.py:33:27: Argument `index` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/examples_methods_syntax/simple_scatter_with_errorbars.py:18:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/stem_and_leaf.py:13:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/top_k_letters.py:26:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/us_employment.py:12:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/wheat_wages.py:31:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/wilkinson-dot-plot.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/examples_methods_syntax/window_rank.py:12:5: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/test_jupyter_chart.py:54:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_core.py:77:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_core.py:182:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_core.py:182:64: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_core.py:193:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_schemapi.py:544:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_schemapi.py:593:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_schemapi.py:945:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_to_values_narwhals.py:40:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:28:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:40:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:46:28: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:46:43: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:47:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:47:55: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:48:28: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:95:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:101:28: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:101:43: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:128:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:134:28: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:134:43: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:172:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:185:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:191:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:203:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:205:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:205:52: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:206:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] tests/utils/test_utils.py:206:64: Argument `dtype` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] tests/utils/test_utils.py:233:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/utils/test_utils.py:259:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/test_common.py:34:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:56:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:91:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:103:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:215:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:234:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:326:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:339:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:539:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:962:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:1392:38: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:1543:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:1552:40: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_api.py:1574:40: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_data.py:11:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_params.py:14:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_params.py:57:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] tests/vegalite/v5/test_params.py:122:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 1512 diagnostics
+ Found 1350 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 650 diagnostics

colour (https://github.com/colour-science/colour)
- error[unknown-argument] colour/continuous/multi_signals.py:1646:13: Argument `data` does not match any known parameter of bound method `__init__`
- error[unknown-argument] colour/continuous/multi_signals.py:1647:13: Argument `index` does not match any known parameter of bound method `__init__`
- error[unknown-argument] colour/continuous/multi_signals.py:1648:13: Argument `columns` does not match any known parameter of bound method `__init__`
- error[unknown-argument] colour/continuous/signal.py:1315:23: Argument `data` does not match any known parameter of bound method `__init__`
- error[unknown-argument] colour/continuous/signal.py:1315:41: Argument `index` does not match any known parameter of bound method `__init__`
- error[unknown-argument] colour/continuous/signal.py:1315:61: Argument `name` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] colour/continuous/tests/test_multi_signal.py:385:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_multi_signal.py:391:52: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] colour/continuous/tests/test_multi_signal.py:1018:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_multi_signal.py:1026:27: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] colour/continuous/tests/test_multi_signal.py:1115:35: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] colour/continuous/tests/test_signal.py:283:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_signal.py:709:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_signal.py:709:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_signal.py:709:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] colour/continuous/tests/test_signal.py:758:27: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 648 diagnostics
+ Found 632 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[too-many-positional-arguments] sphinx/__init__.py:37:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/application.py:206:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/application.py:207:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/application.py:208:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/application.py:265:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/builders/html/__init__.py:1040:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/cmd/build.py:315:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/cmd/build.py:316:21: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sphinx/cmd/make_mode.py:67:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/cmd/make_mode.py:68:35: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/directives/code.py:206:34: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/directives/code.py:222:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/environment/__init__.py:631:67: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/environment/adapters/asset.py:19:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/environment/adapters/asset.py:20:57: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/ext/autosummary/generate.py:78:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/ext/graphviz.py:297:22: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sphinx/project.py:30:32: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/project.py:127:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/project.py:128:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/pycode/__init__.py:48:71: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/pycode/__init__.py:64:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/pycode/__init__.py:93:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/registry.py:520:43: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/search/__init__.py:576:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/search/__init__.py:577:26: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/theming.py:164:43: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/theming.py:172:47: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/transforms/post_transforms/images.py:99:50: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/transforms/post_transforms/images.py:110:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/transforms/post_transforms/images.py:120:50: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/util/_files.py:77:34: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sphinx/util/_pathlib.py:173:55: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/util/i18n.py:69:34: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sphinx/util/i18n.py:131:33: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 712 diagnostics
+ Found 677 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[too-many-positional-arguments] misc/python/materialize/buildkite_insights/step_analysis/analysis.py:105:24: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] misc/python/materialize/scalability/executor/benchmark_executor.py:228:34: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] misc/python/materialize/scalability/executor/benchmark_executor.py:237:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] test/scalability/mzcompose.py:241:33: Argument `data` does not match any known parameter of bound method `__init__`
- Found 3291 diagnostics
+ Found 3287 diagnostics

sympy (https://github.com/sympy/sympy)
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1078:25: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1080:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1146:67: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1161:58: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1171:49: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1185:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1185:64: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1192:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1192:50: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1319:51: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1328:45: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1333:44: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1337:48: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1342:50: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1351:48: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1355:49: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1415:50: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1423:54: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1432:52: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1444:42: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1449:42: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1454:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1458:50: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1463:45: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1467:49: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1471:42: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1476:39: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1481:49: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1486:46: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1491:41: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1496:41: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1501:46: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1506:47: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1511:52: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1516:49: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1521:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1526:43: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1531:48: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1536:41: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1540:42: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1544:44: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1548:47: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1552:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1556:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1560:45: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1564:44: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1568:47: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1572:47: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1576:45: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1580:41: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1584:43: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1588:41: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1592:44: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1597:42: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1601:51: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1606:42: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1610:49: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1614:46: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1618:48: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1622:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1626:46: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1630:51: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1634:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1638:47: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1642:46: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1647:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1652:46: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1657:44: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1662:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1667:52: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1672:45: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1676:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1680:47: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1685:52: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1689:47: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1693:43: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1698:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1703:45: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1708:40: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1713:41: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1719:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1723:55: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1728:49: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1733:47: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1738:67: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1742:52: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1746:53: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1750:47: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1754:55: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1772:102: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1898:28: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1904:42: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1909:47: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1914:43: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1922:48: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/core/tests/test_args.py:1931:50: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/stats/compound_rv.py:90:79: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/compound_rv.py:92:75: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/compound_rv.py:95:71: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/crv.py:430:55: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] sympy/stats/crv.py:430:61: Argument `set` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] sympy/stats/crv_types.py:164:62: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/drv.py:245:53: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/drv_types.py:59:62: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/frv_types.py:63:60: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/joint_rv_types.py:141:38: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/joint_rv_types.py:435:46: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/rv.py:488:51: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/rv.py:495:49: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:278:42: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:1714:42: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:1715:38: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:1870:46: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2232:40: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2233:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2301:39: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2302:35: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2374:38: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/stochastic_process_types.py:2375:34: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/symbolic_probability.py:92:46: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:111:28: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:112:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:117:48: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:117:67: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:119:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:120:62: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:123:31: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:124:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:133:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:134:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:145:28: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:146:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:147:31: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:154:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:158:58: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_compound_rv.py:158:77: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_continuous_rv.py:675:59: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_continuous_rv.py:1369:29: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_continuous_rv.py:1376:54: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_continuous_rv.py:1385:54: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_continuous_rv.py:1567:43: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[unknown-argument] sympy/stats/tests/test_continuous_rv.py:1567:49: Argument `set` does not match any known parameter of bound method `__init__`
- error[too-many-positional-arguments] sympy/stats/tests/test_discrete_rv.py:36:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_discrete_rv.py:49:46: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_discrete_rv.py:82:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:136:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:309:60: Too many positional arguments to bound method `__init__`: expected 0, got 4
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:374:55: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:375:73: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:484:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_finite_rv.py:496:45: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_joint_rv.py:60:57: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_matrix_distributions.py:17:33: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_mix.py:33:69: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_random_matrix.py:116:31: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_rv.py:315:13: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_rv.py:325:40: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_rv.py:387:39: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:100:66: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:481:74: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:485:71: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:514:55: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:544:53: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:551:74: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:554:68: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:660:52: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:668:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:670:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:713:51: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[too-many-positional-arguments] sympy/stats/tests/test_stochastic_process.py:716:71: Too many positional arguments to bound method `__init__`: expected 0, got 1
- Found 17346 diagnostics
+ Found 17176 diagnostics

```
</details>


---

_Merged by @sharkdp on 2025-05-14 13:33_

---

_Closed by @sharkdp on 2025-05-14 13:33_

---

_Branch deleted on 2025-05-14 13:33_

---
