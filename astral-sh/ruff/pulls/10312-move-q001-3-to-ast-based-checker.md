```yaml
number: 10312
title: "Move `Q001-3` to AST based checker"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/move-flake8-quote-rules-to-ast-checker
created_at: 2024-03-09T12:03:49Z
updated_at: 2024-03-23T17:29:51Z
url: https://github.com/astral-sh/ruff/pull/10312
synced_at: 2026-01-12T15:55:31Z
```

# Move `Q001-3` to AST based checker

---

_@dhruvmanila_

## Summary

Continuing with https://github.com/astral-sh/ruff/issues/7595, this PR moves the `Q001`, `Q002`, `Q003` rules to the AST based checker.

## Test Plan

Make sure all of the existing test cases pass and verify there are no ecosystem changes.


---

_Label `internal` added by @dhruvmanila on 2024-03-09 12:03_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-03-09 12:06_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-03-09 12:07_

---

_Comment by @github-actions[bot] on 2024-03-09 12:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+115 -115 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+115 -115 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrow.py#L1'>examples/basic/annotations/arrow.py:1:1:</a> INP001 File `examples/basic/annotations/arrow.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrow.py#L1'>examples/basic/annotations/arrow.py:1:1:</a> INP001 File `examples/basic/annotations/arrow.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L1'>examples/basic/annotations/arrowheads.py:1:1:</a> INP001 File `examples/basic/annotations/arrowheads.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L1'>examples/basic/annotations/arrowheads.py:1:1:</a> INP001 File `examples/basic/annotations/arrowheads.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L1'>examples/basic/annotations/band.py:1:1:</a> INP001 File `examples/basic/annotations/band.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L1'>examples/basic/annotations/band.py:1:1:</a> INP001 File `examples/basic/annotations/band.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/box_annotation.py#L1'>examples/basic/annotations/box_annotation.py:1:1:</a> INP001 File `examples/basic/annotations/box_annotation.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/box_annotation.py#L1'>examples/basic/annotations/box_annotation.py:1:1:</a> INP001 File `examples/basic/annotations/box_annotation.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L1'>examples/basic/annotations/colorbar_log.py:1:1:</a> INP001 File `examples/basic/annotations/colorbar_log.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L1'>examples/basic/annotations/colorbar_log.py:1:1:</a> INP001 File `examples/basic/annotations/colorbar_log.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/label.py#L1'>examples/basic/annotations/label.py:1:1:</a> INP001 File `examples/basic/annotations/label.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/label.py#L1'>examples/basic/annotations/label.py:1:1:</a> INP001 File `examples/basic/annotations/label.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend.py#L1'>examples/basic/annotations/legend.py:1:1:</a> INP001 File `examples/basic/annotations/legend.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend.py#L1'>examples/basic/annotations/legend.py:1:1:</a> INP001 File `examples/basic/annotations/legend.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend_two_dimensions.py#L1'>examples/basic/annotations/legend_two_dimensions.py:1:1:</a> INP001 File `examples/basic/annotations/legend_two_dimensions.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend_two_dimensions.py#L1'>examples/basic/annotations/legend_two_dimensions.py:1:1:</a> INP001 File `examples/basic/annotations/legend_two_dimensions.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_item_visibility.py#L1'>examples/basic/annotations/legends_item_visibility.py:1:1:</a> INP001 File `examples/basic/annotations/legends_item_visibility.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_item_visibility.py#L1'>examples/basic/annotations/legends_item_visibility.py:1:1:</a> INP001 File `examples/basic/annotations/legends_item_visibility.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/slope.py#L1'>examples/basic/annotations/slope.py:1:1:</a> INP001 File `examples/basic/annotations/slope.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/slope.py#L1'>examples/basic/annotations/slope.py:1:1:</a> INP001 File `examples/basic/annotations/slope.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L1'>examples/basic/annotations/whisker.py:1:1:</a> INP001 File `examples/basic/annotations/whisker.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L1'>examples/basic/annotations/whisker.py:1:1:</a> INP001 File `examples/basic/annotations/whisker.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L1'>examples/basic/areas/stacked_area.py:1:1:</a> INP001 File `examples/basic/areas/stacked_area.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L1'>examples/basic/areas/stacked_area.py:1:1:</a> INP001 File `examples/basic/areas/stacked_area.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/logplot.py#L1'>examples/basic/axes/logplot.py:1:1:</a> INP001 File `examples/basic/axes/logplot.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/logplot.py#L1'>examples/basic/axes/logplot.py:1:1:</a> INP001 File `examples/basic/axes/logplot.py` is part of an implicit namespace package. Add an `__init__.py`.
... 199 additional changes omitted for rule INP001
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L540'>tests/unit/bokeh/command/subcommands/test_serve.py:540:20:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L540'>tests/unit/bokeh/command/subcommands/test_serve.py:540:20:</a> S104 Possible binding to all interfaces
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L51'>tests/unit/bokeh/test_server.py:51:48:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L51'>tests/unit/bokeh/test_server.py:51:48:</a> S104 Possible binding to all interfaces
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L52'>tests/unit/bokeh/test_server.py:52:34:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L52'>tests/unit/bokeh/test_server.py:52:34:</a> S104 Possible binding to all interfaces
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 224 | 112 | 112 | 0 | 0 |
| S104 | 6 | 3 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+115 -115 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+115 -115 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrow.py#L1'>examples/basic/annotations/arrow.py:1:1:</a> INP001 File `examples/basic/annotations/arrow.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrow.py#L1'>examples/basic/annotations/arrow.py:1:1:</a> INP001 File `examples/basic/annotations/arrow.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L1'>examples/basic/annotations/arrowheads.py:1:1:</a> INP001 File `examples/basic/annotations/arrowheads.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/arrowheads.py#L1'>examples/basic/annotations/arrowheads.py:1:1:</a> INP001 File `examples/basic/annotations/arrowheads.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L1'>examples/basic/annotations/band.py:1:1:</a> INP001 File `examples/basic/annotations/band.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/band.py#L1'>examples/basic/annotations/band.py:1:1:</a> INP001 File `examples/basic/annotations/band.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/box_annotation.py#L1'>examples/basic/annotations/box_annotation.py:1:1:</a> INP001 File `examples/basic/annotations/box_annotation.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/box_annotation.py#L1'>examples/basic/annotations/box_annotation.py:1:1:</a> INP001 File `examples/basic/annotations/box_annotation.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L1'>examples/basic/annotations/colorbar_log.py:1:1:</a> INP001 File `examples/basic/annotations/colorbar_log.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/colorbar_log.py#L1'>examples/basic/annotations/colorbar_log.py:1:1:</a> INP001 File `examples/basic/annotations/colorbar_log.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/label.py#L1'>examples/basic/annotations/label.py:1:1:</a> INP001 File `examples/basic/annotations/label.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/label.py#L1'>examples/basic/annotations/label.py:1:1:</a> INP001 File `examples/basic/annotations/label.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend.py#L1'>examples/basic/annotations/legend.py:1:1:</a> INP001 File `examples/basic/annotations/legend.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend.py#L1'>examples/basic/annotations/legend.py:1:1:</a> INP001 File `examples/basic/annotations/legend.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend_two_dimensions.py#L1'>examples/basic/annotations/legend_two_dimensions.py:1:1:</a> INP001 File `examples/basic/annotations/legend_two_dimensions.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legend_two_dimensions.py#L1'>examples/basic/annotations/legend_two_dimensions.py:1:1:</a> INP001 File `examples/basic/annotations/legend_two_dimensions.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_item_visibility.py#L1'>examples/basic/annotations/legends_item_visibility.py:1:1:</a> INP001 File `examples/basic/annotations/legends_item_visibility.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/legends_item_visibility.py#L1'>examples/basic/annotations/legends_item_visibility.py:1:1:</a> INP001 File `examples/basic/annotations/legends_item_visibility.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/slope.py#L1'>examples/basic/annotations/slope.py:1:1:</a> INP001 File `examples/basic/annotations/slope.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/slope.py#L1'>examples/basic/annotations/slope.py:1:1:</a> INP001 File `examples/basic/annotations/slope.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L1'>examples/basic/annotations/whisker.py:1:1:</a> INP001 File `examples/basic/annotations/whisker.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/annotations/whisker.py#L1'>examples/basic/annotations/whisker.py:1:1:</a> INP001 File `examples/basic/annotations/whisker.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L1'>examples/basic/areas/stacked_area.py:1:1:</a> INP001 File `examples/basic/areas/stacked_area.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/areas/stacked_area.py#L1'>examples/basic/areas/stacked_area.py:1:1:</a> INP001 File `examples/basic/areas/stacked_area.py` is part of an implicit namespace package. Add an `__init__.py`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/logplot.py#L1'>examples/basic/axes/logplot.py:1:1:</a> INP001 File `examples/basic/axes/logplot.py` is part of an implicit namespace package. Add an `__init__.py`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/logplot.py#L1'>examples/basic/axes/logplot.py:1:1:</a> INP001 File `examples/basic/axes/logplot.py` is part of an implicit namespace package. Add an `__init__.py`.
... 199 additional changes omitted for rule INP001
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L540'>tests/unit/bokeh/command/subcommands/test_serve.py:540:20:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L540'>tests/unit/bokeh/command/subcommands/test_serve.py:540:20:</a> S104 Possible binding to all interfaces
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L51'>tests/unit/bokeh/test_server.py:51:48:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L51'>tests/unit/bokeh/test_server.py:51:48:</a> S104 Possible binding to all interfaces
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L52'>tests/unit/bokeh/test_server.py:52:34:</a> S104 Possible binding to all interfaces
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_server.py#L52'>tests/unit/bokeh/test_server.py:52:34:</a> S104 Possible binding to all interfaces
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 224 | 112 | 112 | 0 | 0 |
| S104 | 6 | 3 | 3 | 0 | 0 |

</p>
</details>




---

_Comment by @dhruvmanila on 2024-03-09 12:23_

Oh right. We will visit the strings inside a f-string and try to change that. I'll fix this later.

---

_@charliermarsh approved on 2024-03-09 12:47_

---

_Comment by @charliermarsh on 2024-03-09 12:54_

Oops, approved before I saw the ecosystem changes.

---

_Comment by @codspeed-hq[bot] on 2024-03-10 05:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-flake8-quote-rules-to-ast-checker)

### Merging #10312 will **not alter performance**

<sub>Comparing <code>dhruv/move-flake8-quote-rules-to-ast-checker</code> (743c772) with <code>main</code> (0c194f5)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@dhruvmanila reviewed on 2024-03-11 13:57_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:443 on 2024-03-11 13:57_

TODO: I just realized we could probably just use `checker.semantic().in_fstring()`

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-13 08:51_

---

_@dhruvmanila reviewed on 2024-03-13 09:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:443 on 2024-03-13 09:21_

Actually, this will be true even for the _outermost_ f-string.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-03-14 07:59_

---

_@dhruvmanila reviewed on 2024-03-14 08:02_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:440 on 2024-03-14 08:02_

We only want to ignore strings which are _strictly_ inside another f-string which is not what `TextRange::contains_range` does.

---

_Comment by @dhruvmanila on 2024-03-14 08:31_

Weird, this PR doesn't include any change to the rules detected in the ecosystem checks `INP001` and `S104`. I tried running them locally as well and it gave me no difference between running ecosystem on `main` and this branch.

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-03-14 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:435 on 2024-03-18 13:15_

Nit: I noticed that this rule performs quiet many allocations even in the happy path where the quotes match (collects the text ranges, collects the trivia, etc). 

I wonder if we could add a fast path (you may need to wait for @AlexWaygood's changes) where we first test if:

* If it's a docstring, the quote matches the configured docstring quotes and if so, exits. 
* For "regular strings", early exit if the configured inline or multiline quotes are the same and the string quotes match. 



---

_@MichaReiser reviewed on 2024-03-18 13:15_

---

_@MichaReiser approved on 2024-03-18 13:16_

---

_@dhruvmanila reviewed on 2024-03-18 13:18_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:435 on 2024-03-18 13:18_

Yeah, I had something similar in mind which is to completely rewrite this rule by only using the string flags which I think should be possible. I'll open a tracking issue for it. Thanks for the suggestion!

Edit: https://github.com/astral-sh/ruff/issues/10456

---

_Merged by @dhruvmanila on 2024-03-23 17:29_

---

_Closed by @dhruvmanila on 2024-03-23 17:29_

---

_Branch deleted on 2024-03-23 17:29_

---
