```yaml
number: 7229
title: "[Autofix error] `pyvista/tests/plotting/test_charts.py`"
type: issue
state: closed
author: eggplants
labels:
  - bug
assignees: []
created_at: 2023-09-07T21:30:24Z
updated_at: 2023-09-07T23:20:58Z
url: https://github.com/astral-sh/ruff/issues/7229
synced_at: 2026-01-10T11:09:49Z
```

# [Autofix error] `pyvista/tests/plotting/test_charts.py`

---

_Issue opened by @eggplants on 2023-09-07 21:30_

I encountered an autofix error when I try to introduce ruff into [pyvista](https://github.com/pyvista/pyvista).

Problematic file: [`tests/plotting/test_charts.py`](https://github.com/pyvista/pyvista/blob/main/tests/plotting/test_charts.py)

---

## The current Ruff settings

`pyproject.toml`:

```toml
...
[tool.ruff]
select = ["ALL"]
ignore = ["D"]
line-length = 100

[tool.ruff.per-file-ignores]
"tests/**/test_*.py" = [
  "S101", # Use of assert detected
]
...
```

`.pre-commit-config.yaml`:

```yaml
repos:
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.0.287
  hooks:
  - id: ruff
    args: [--fix]
```

<details>

<summary><code>pre-commit run ruff --files tests/plotting/test_charts.py</code></summary>

```shellsession
$ pre-commit run ruff --files tests/plotting/test_charts.py
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `tests/plotting/test_charts.py`, the rule codes ANN204, COM812, ERA001, PIE790, PT001, PT018, PT023, Q000, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

tests/plotting/test_charts.py:1:1: INP001 File `tests/plotting/test_charts.py` is part of an implicit namespace package. Add an `__init__.py`.
tests/plotting/test_charts.py:17:26: Q000 Single quotes found but double quotes preferred
tests/plotting/test_charts.py:17:43: Q000 Single quotes found but double quotes preferred
tests/plotting/test_charts.py:17:85: COM812 Trailing comma missing
tests/plotting/test_charts.py:22:5: PT004 Fixture `skip_check_gc` does not return anything, add leading underscore
tests/plotting/test_charts.py:22:5: ANN201 Missing return type annotation for public function `skip_check_gc`
tests/plotting/test_charts.py:22:19: ANN001 Missing type annotation for function argument `skip_check_gc`
tests/plotting/test_charts.py:22:19: ARG001 Unused function argument: `skip_check_gc`
tests/plotting/test_charts.py:24:5: PIE790 Unnecessary `pass` statement
tests/plotting/test_charts.py:32:5: ANN201 Missing return type annotation for public function `vtk_array_to_tuple`
tests/plotting/test_charts.py:32:24: ANN001 Missing type annotation for function argument `arr`
tests/plotting/test_charts.py:36:5: ANN201 Missing return type annotation for public function `to_vtk_scientific`
tests/plotting/test_charts.py:36:23: ANN001 Missing type annotation for function argument `val`
tests/plotting/test_charts.py:37:23: Q000 Single quotes found but double quotes preferred
tests/plotting/test_charts.py:49:9: ANN204 Missing return type annotation for special method `__init__`
tests/plotting/test_charts.py:49:18: ANN101 Missing type annotation for `self` in method
tests/plotting/test_charts.py:49:24: ANN001 Missing type annotation for function argument `plotter`
tests/plotting/test_charts.py:53:9: ANN202 Missing return type annotation for private function `_capture`
tests/plotting/test_charts.py:53:18: ANN101 Missing type annotation for `self` in method
tests/plotting/test_charts.py:57:9: ANN204 Missing return type annotation for special method `__call__`
tests/plotting/test_charts.py:57:18: ANN101 Missing type annotation for `self` in method
tests/plotting/test_charts.py:64:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:65:5: ANN201 Missing return type annotation for public function `pl`
tests/plotting/test_charts.py:67:26: Q000 Single quotes found but double quotes preferred
tests/plotting/test_charts.py:71:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:72:5: ANN201 Missing return type annotation for public function `chart_2d`
tests/plotting/test_charts.py:76:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:77:5: ANN201 Missing return type annotation for public function `chart_box`
tests/plotting/test_charts.py:81:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:82:5: ANN201 Missing return type annotation for public function `chart_pie`
tests/plotting/test_charts.py:86:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:87:5: ANN201 Missing return type annotation for public function `chart_mpl`
tests/plotting/test_charts.py:95:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:96:5: ANN201 Missing return type annotation for public function `line_plot_2d`
tests/plotting/test_charts.py:96:18: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:100:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:101:5: ANN201 Missing return type annotation for public function `scatter_plot_2d`
tests/plotting/test_charts.py:101:21: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:105:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:106:5: ANN201 Missing return type annotation for public function `area_plot`
tests/plotting/test_charts.py:106:15: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:110:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:111:5: ANN201 Missing return type annotation for public function `bar_plot`
tests/plotting/test_charts.py:111:14: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:115:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:116:5: ANN201 Missing return type annotation for public function `stack_plot`
tests/plotting/test_charts.py:116:16: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:117:12: PD013 `.melt` is preferred to `.stack`; provides same functionality
tests/plotting/test_charts.py:120:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:121:5: ANN201 Missing return type annotation for public function `box_plot`
tests/plotting/test_charts.py:121:14: ANN001 Missing type annotation for function argument `chart_box`
tests/plotting/test_charts.py:125:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:126:5: ANN201 Missing return type annotation for public function `pie_plot`
tests/plotting/test_charts.py:126:14: ANN001 Missing type annotation for function argument `chart_pie`
tests/plotting/test_charts.py:130:1: PT001 Use `@pytest.fixture()` over `@pytest.fixture`
tests/plotting/test_charts.py:131:5: ANN201 Missing return type annotation for public function `axis`
tests/plotting/test_charts.py:131:10: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:138:5: ANN201 Missing return type annotation for public function `test_pen`
tests/plotting/test_charts.py:165:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:169:5: ANN201 Missing return type annotation for public function `test_wrapping`
tests/plotting/test_charts.py:173:5: N806 Variable `wrappedPen` in function should be lowercase
tests/plotting/test_charts.py:182:5: ANN201 Missing return type annotation for public function `test_brush`
tests/plotting/test_charts.py:207:5: N806 Variable `NEAREST` in function should be lowercase
tests/plotting/test_charts.py:212:5: N806 Variable `REPEAT` in function should be lowercase
tests/plotting/test_charts.py:216:5: ANN201 Missing return type annotation for public function `test_axis_init`
tests/plotting/test_charts.py:223:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:227:5: ANN201 Missing return type annotation for public function `test_axis_label`
tests/plotting/test_charts.py:227:21: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:238:5: ANN201 Missing return type annotation for public function `test_axis_range`
tests/plotting/test_charts.py:238:21: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:253:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:257:5: ANN201 Missing return type annotation for public function `test_axis_margin`
tests/plotting/test_charts.py:257:22: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:264:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:265:5: ANN201 Missing return type annotation for public function `test_axis_scale`
tests/plotting/test_charts.py:265:21: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:265:31: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:267:101: E501 Line too long (130 > 100 characters)
tests/plotting/test_charts.py:275:101: E501 Line too long (107 > 100 characters)
tests/plotting/test_charts.py:284:5: ANN201 Missing return type annotation for public function `test_axis_grid`
tests/plotting/test_charts.py:284:20: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:290:5: ANN201 Missing return type annotation for public function `test_axis_visible`
tests/plotting/test_charts.py:290:23: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:299:5: ANN201 Missing return type annotation for public function `test_axis_tick_count`
tests/plotting/test_charts.py:299:26: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:313:5: ANN201 Missing return type annotation for public function `test_axis_tick_locations`
tests/plotting/test_charts.py:313:30: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:313:40: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:330:35: PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
tests/plotting/test_charts.py:340:35: PLR2004 Magic value used in comparison, consider replacing 4 with a constant variable
tests/plotting/test_charts.py:350:5: ANN201 Missing return type annotation for public function `test_axis_tick_size`
tests/plotting/test_charts.py:350:25: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:357:5: ANN201 Missing return type annotation for public function `test_axis_tick_offset`
tests/plotting/test_charts.py:357:27: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:364:5: ANN201 Missing return type annotation for public function `test_axis_tick_labels_visible`
tests/plotting/test_charts.py:364:35: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:371:5: ANN201 Missing return type annotation for public function `test_axis_tick_visible`
tests/plotting/test_charts.py:371:28: ANN001 Missing type annotation for function argument `axis`
tests/plotting/test_charts.py:377:5: ANN201 Missing return type annotation for public function `test_axis_label_font_size`
tests/plotting/test_charts.py:377:31: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:391:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:392:37: PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
tests/plotting/test_charts.py:393:5: ANN201 Missing return type annotation for public function `test_chart_common`
tests/plotting/test_charts.py:393:23: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:393:27: ANN001 Missing type annotation for function argument `chart_f`
tests/plotting/test_charts.py:393:36: ANN001 Missing type annotation for function argument `request`
tests/plotting/test_charts.py:402:12: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:403:12: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:405:12: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:405:28: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:405:28: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:406:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:406:12: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:406:47: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:406:66: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:406:66: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:419:5: SLF001 Private member accessed: `_render_event`
tests/plotting/test_charts.py:420:12: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:427:5: SLF001 Private member accessed: `_render_event`
tests/plotting/test_charts.py:428:12: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:436:12: SLF001 Private member accessed: `_is_within`
tests/plotting/test_charts.py:437:89: COM812 Trailing comma missing
tests/plotting/test_charts.py:439:16: SLF001 Private member accessed: `_is_within`
tests/plotting/test_charts.py:440:16: SLF001 Private member accessed: `_is_within`
tests/plotting/test_charts.py:441:16: SLF001 Private member accessed: `_is_within`
tests/plotting/test_charts.py:471:5: PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
tests/plotting/test_charts.py:481:5: ANN201 Missing return type annotation for public function `test_plot_common`
tests/plotting/test_charts.py:481:22: ANN001 Missing type annotation for function argument `plot_f`
tests/plotting/test_charts.py:481:30: ANN001 Missing type annotation for function argument `request`
tests/plotting/test_charts.py:487:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:515:36: PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
tests/plotting/test_charts.py:516:5: ANN201 Missing return type annotation for public function `test_multicomp_plot_common`
tests/plotting/test_charts.py:516:32: ANN001 Missing type annotation for function argument `plot_f`
tests/plotting/test_charts.py:516:40: ANN001 Missing type annotation for function argument `request`
tests/plotting/test_charts.py:534:12: SLF001 Private member accessed: `_color_series`
tests/plotting/test_charts.py:537:18: SLF001 Private member accessed: `_color_series`
tests/plotting/test_charts.py:540:22: SLF001 Private member accessed: `_lookup_table`
tests/plotting/test_charts.py:551:18: SLF001 Private member accessed: `_color_series`
tests/plotting/test_charts.py:554:22: SLF001 Private member accessed: `_lookup_table`
tests/plotting/test_charts.py:560:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:578:5: ANN201 Missing return type annotation for public function `test_lineplot2d`
tests/plotting/test_charts.py:578:21: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:578:31: ANN001 Missing type annotation for function argument `line_plot_2d`
tests/plotting/test_charts.py:584:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:588:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:602:5: ANN201 Missing return type annotation for public function `test_scatterplot2d`
tests/plotting/test_charts.py:602:24: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:602:34: ANN001 Missing type annotation for function argument `scatter_plot_2d`
tests/plotting/test_charts.py:608:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:615:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:637:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:641:5: ANN201 Missing return type annotation for public function `test_areaplot`
tests/plotting/test_charts.py:641:19: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:641:29: ANN001 Missing type annotation for function argument `area_plot`
tests/plotting/test_charts.py:646:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:650:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:664:5: ANN201 Missing return type annotation for public function `test_barplot`
tests/plotting/test_charts.py:664:18: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:664:28: ANN001 Missing type annotation for function argument `bar_plot`
tests/plotting/test_charts.py:669:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:674:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:683:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:691:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:693:5: ERA001 Found commented-out code
tests/plotting/test_charts.py:694:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:696:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:707:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:711:5: ANN201 Missing return type annotation for public function `test_stackplot`
tests/plotting/test_charts.py:711:20: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:711:30: ANN001 Missing type annotation for function argument `stack_plot`
tests/plotting/test_charts.py:715:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:719:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:727:12: SLF001 Private member accessed: `_chart`
tests/plotting/test_charts.py:734:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:736:5: ERA001 Found commented-out code
tests/plotting/test_charts.py:737:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:739:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:748:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:749:5: PLR0915 Too many statements (127 > 50)
tests/plotting/test_charts.py:749:5: ANN201 Missing return type annotation for public function `test_chart_2d`
tests/plotting/test_charts.py:749:19: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:749:23: ANN001 Missing type annotation for function argument `chart_2d`
tests/plotting/test_charts.py:768:43: FBT003 Boolean positional value in function call
tests/plotting/test_charts.py:777:16: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:779:24: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:783:9: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:783:100: COM812 Trailing comma missing
tests/plotting/test_charts.py:790:13: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:793:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:794:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:795:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:796:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:797:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:798:38: SLF001 Private member accessed: `_parse_format`
tests/plotting/test_charts.py:801:8: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:802:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:804:8: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:805:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:809:8: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:810:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:814:8: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:815:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:816:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:831:5: E741 Ambiguous variable name: `l`
tests/plotting/test_charts.py:859:9: PD013 `.melt` is preferred to `.stack`; provides same functionality
tests/plotting/test_charts.py:872:40: PLR2004 Magic value used in comparison, consider replacing 5 with a constant variable
tests/plotting/test_charts.py:875:24: PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
tests/plotting/test_charts.py:898:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:902:9: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:911:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:912:5: ANN201 Missing return type annotation for public function `test_chart_box`
tests/plotting/test_charts.py:912:20: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:912:24: ANN001 Missing type annotation for function argument `chart_box`
tests/plotting/test_charts.py:912:35: ANN001 Missing type annotation for function argument `box_plot`
tests/plotting/test_charts.py:930:16: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:932:24: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:936:9: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:936:100: COM812 Trailing comma missing
tests/plotting/test_charts.py:947:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:948:5: ANN201 Missing return type annotation for public function `test_chart_pie`
tests/plotting/test_charts.py:948:20: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:948:24: ANN001 Missing type annotation for function argument `chart_pie`
tests/plotting/test_charts.py:948:35: ANN001 Missing type annotation for function argument `pie_plot`
tests/plotting/test_charts.py:965:16: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:967:24: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:971:9: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:971:100: COM812 Trailing comma missing
tests/plotting/test_charts.py:981:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:982:5: ANN201 Missing return type annotation for public function `test_chart_mpl`
tests/plotting/test_charts.py:982:20: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:982:24: ANN001 Missing type annotation for function argument `chart_mpl`
tests/plotting/test_charts.py:982:24: ARG001 Unused function argument: `chart_mpl`
tests/plotting/test_charts.py:996:16: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:998:24: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:1000:24: SLF001 Private member accessed: `_canvas`
tests/plotting/test_charts.py:1004:9: SLF001 Private member accessed: `_geometry`
tests/plotting/test_charts.py:1004:100: COM812 Trailing comma missing
tests/plotting/test_charts.py:1007:24: SLF001 Private member accessed: `_canvas`
tests/plotting/test_charts.py:1014:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:1015:5: ANN201 Missing return type annotation for public function `test_chart_mpl_update`
tests/plotting/test_charts.py:1015:27: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:1043:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:1044:5: ANN201 Missing return type annotation for public function `test_charts`
tests/plotting/test_charts.py:1044:17: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:1051:36: SLF001 Private member accessed: `_renderer`
tests/plotting/test_charts.py:1052:12: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1052:12: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1052:51: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1054:16: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1054:40: PLR2004 Magic value used in comparison, consider replacing 2 with a constant variable
tests/plotting/test_charts.py:1058:10: SLF001 Private member accessed: `_get_charts_by_pos`
tests/plotting/test_charts.py:1061:10: SLF001 Private member accessed: `_get_charts_by_pos`
tests/plotting/test_charts.py:1066:16: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1067:12: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1068:24: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1070:16: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1075:16: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1076:12: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1076:12: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1080:16: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1081:12: SLF001 Private member accessed: `_charts`
tests/plotting/test_charts.py:1081:12: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1084:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:1085:5: ANN201 Missing return type annotation for public function `test_iren_context_style`
tests/plotting/test_charts.py:1085:29: ANN001 Missing type annotation for function argument `pl`
tests/plotting/test_charts.py:1090:13: SLF001 Private member accessed: `_style`
tests/plotting/test_charts.py:1091:19: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1094:5: SLF001 Private member accessed: `_mouse_left_button_click`
tests/plotting/test_charts.py:1096:12: SLF001 Private member accessed: `_style`
tests/plotting/test_charts.py:1097:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1097:36: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1098:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1098:58: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1101:5: SLF001 Private member accessed: `_mouse_left_button_click`
tests/plotting/test_charts.py:1103:12: SLF001 Private member accessed: `_style`
tests/plotting/test_charts.py:1104:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1105:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1108:1: PT023 Use `@pytest.mark.skip_plotting()` over `@pytest.mark.skip_plotting`
tests/plotting/test_charts.py:1110:99: COM812 Trailing comma missing
tests/plotting/test_charts.py:1112:5: PLR0915 Too many statements (55 > 50)
tests/plotting/test_charts.py:1112:5: ANN201 Missing return type annotation for public function `test_chart_interaction`
tests/plotting/test_charts.py:1119:41: FBT003 Boolean positional value in function call
tests/plotting/test_charts.py:1129:36: FBT003 Boolean positional value in function call
tests/plotting/test_charts.py:1130:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1131:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1132:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1132:36: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1133:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1133:49: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1134:36: FBT003 Boolean positional value in function call
tests/plotting/test_charts.py:1136:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1137:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1137:36: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1138:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1142:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1143:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1145:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1146:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1148:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1149:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1153:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1154:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1160:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1161:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1163:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1165:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1166:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1166:49: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1172:5: SLF001 Private member accessed: `_mouse_left_button_click`
tests/plotting/test_charts.py:1174:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1175:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1175:36: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1176:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1176:49: SLF001 Private member accessed: `_scene`
tests/plotting/test_charts.py:1178:5: SLF001 Private member accessed: `_mouse_left_button_click`
tests/plotting/test_charts.py:1180:5: PT018 Assertion should be broken down into multiple parts
tests/plotting/test_charts.py:1181:12: SLF001 Private member accessed: `_style_class`
tests/plotting/test_charts.py:1181:36: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1182:12: SLF001 Private member accessed: `_context_style`
tests/plotting/test_charts.py:1186:5: ANN201 Missing return type annotation for public function `test_get_background_texture`
tests/plotting/test_charts.py:1186:33: ANN001 Missing type annotation for function argument `chart_2d`
Found 321 errors.

```

</details>

---

_Comment by @zanieb on 2023-09-07 21:58_

Thanks for the details! Can you share the error? :)

---

_Comment by @eggplants on 2023-09-07 21:59_

Please see the folded codeblock.

![image](https://github.com/astral-sh/ruff/assets/42153744/38a8be00-3339-4ad7-adb9-394ddb9c8c23)


---

_Comment by @zanieb on 2023-09-07 23:13_

Thanks!

The issue can be shrunk to

```python
for axis in (chart_2d.x_axis, chart_2d.y_axis):
    assert not (
        axis.visible
        or axis.label_visible
        or axis.ticks_visible
        or axis.tick_labels_visible
        or axis.grid
    )
```

which is a `PT018` violation. The fix results in

```python
for axis in (chart_2d.x_axis, chart_2d.y_axis):
    assert not axis.visible
        or axis.label_visible
        or axis.ticks_visible
        or axis.tick_labels_visible
    assert not axis.grid
```

which is not valid syntax.

This is resolved on our `main` branch by https://github.com/astral-sh/ruff/pull/7151 — fixing results in

```python
for axis in (chart_2d.x_axis, chart_2d.y_axis):
    assert not (
        axis.visible
    )
    assert not (
        axis.label_visible
    )
    assert not (
        axis.ticks_visible
    )
    assert not (
        axis.tick_labels_visible
    )
    assert not (
        axis.grid
    )
```

It'll be available on our next release. Otherwise you can split the assertion or ignore the rule for now.

---

_Closed by @zanieb on 2023-09-07 23:13_

---

_Comment by @zanieb on 2023-09-07 23:14_

Thanks @qarmin for finding this bug by fuzzing!

---

_Label `bug` added by @zanieb on 2023-09-07 23:15_

---

_Comment by @eggplants on 2023-09-07 23:20_

@zanieb @qarmin Thank you for your investigating! I'll make sure this issue is fixed in the next release of astral-sh/ruff-pre-commit including e0939340e58c38bf4e3df4d91e58582ea691ee8f.

---
