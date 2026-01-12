```yaml
number: 11349
title: "[`pycodestyle`] Implement `missing-or-outdented-indentation` (`E122`)"
type: pull_request
state: open
author: augustelalande
labels: []
assignees: []
base: main
head: E122
created_at: 2024-05-09T01:54:43Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/11349
synced_at: 2026-01-12T15:55:37Z
```

# [`pycodestyle`] Implement `missing-or-outdented-indentation` (`E122`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Following the discussion in #8557, and since no more work has been done since then, I thought I would take a stab at `E122`.

Part of #2402.

## Test Plan

Test fixtures added.

I also validated the output against pycodestyle on some of the ecosystem repos.

---

_Comment by @codspeed-hq[bot] on 2024-05-09 02:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande%3AE122?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #11349 will **not alter performance**

<sub>Comparing <code>augustelalande:E122</code> (c12d4b2) with <code>main</code> (5806bc9)</sub>



### Summary

`✅ 30` untouched  





---

_Comment by @github-actions[bot] on 2024-05-09 02:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -0 violations, +0 -0 fixes in 3 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/ui/ui_element.py#L81'>src/bokeh/models/ui/ui_element.py:81:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L329'>src/bokeh/util/compiler.py:329:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L340'>src/bokeh/util/compiler.py:340:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L351'>src/bokeh/util/compiler.py:351:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L366'>src/bokeh/util/compiler.py:366:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L382'>src/bokeh/util/compiler.py:382:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L385'>src/bokeh/util/compiler.py:385:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/command/subcommands/test_serve.py#L403'>tests/unit/bokeh/command/subcommands/test_serve.py:403:1:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e95f5a5294ff28588c14a2f3f5aae19d9210a47e/examples/example_tls1.py#L70'>examples/example_tls1.py:70:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e95f5a5294ff28588c14a2f3f5aae19d9210a47e/examples/example_tls1.py#L71'>examples/example_tls1.py:71:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e95f5a5294ff28588c14a2f3f5aae19d9210a47e/examples/example_tls2.py#L72'>examples/example_tls2.py:72:1:</a> E122 Continuation line missing indentation or outdented.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e95f5a5294ff28588c14a2f3f5aae19d9210a47e/examples/example_tls2.py#L73'>examples/example_tls2.py:73:1:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0e90f66a88e0d4b16b143f1e563ec5fa3565469b/pandas/tests/extension/test_arrow.py#L929'>pandas/tests/extension/test_arrow.py:929:1:</a> E122 Continuation line missing indentation or outdented.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E122 | 13 | 13 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @augustelalande on 2024-05-09 02:33_

---

_Marked ready for review by @augustelalande on 2024-05-19 06:48_

---

_Comment by @augustelalande on 2024-05-19 06:48_

@MichaReiser  Could you review this when you get the chance? Thanks.

Note when reviewing: pycodestyle allows two types of indentation: hanging indent and visual indent. However, it does not enforce a single style. Which is to say that even if one style is violated, if the other is followed then no error is reported. Therefore even though this rule doesn't check for visual indentation, it still needs to establish if visual indentation is present in order to match the pycodestyle behavior.

---

_Assigned to @MichaReiser by @MichaReiser on 2024-07-23 07:36_

---
