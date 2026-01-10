```yaml
number: 11187
title: "[`refurb`] Implement `repeated-global` (`FURB154`)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: latyshev/furb154
created_at: 2024-04-28T22:24:10Z
updated_at: 2025-06-10T20:47:03Z
url: https://github.com/astral-sh/ruff/pull/11187
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Implement `repeated-global` (`FURB154`)

---

_Pull request opened by @alex-700 on 2024-04-28 22:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement repeated_global (FURB154) lint.
See:
- https://github.com/astral-sh/ruff/issues/1348
- [original lint](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/simplify_global_and_nonlocal.py)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-04-28 22:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 3 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/plugins_manager.py#L361'>airflow/plugins_manager.py:361:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/plugins_manager.py#L422'>airflow/plugins_manager.py:422:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/plugins_manager.py#L476'>airflow/plugins_manager.py:476:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/plugins_manager.py#L503'>airflow/plugins_manager.py:503:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/providers/weaviate/hooks/weaviate.py#L418'>airflow/providers/weaviate/hooks/weaviate.py:418:13:</a> FURB154 [*] Use of repeated consecutive `nonlocal`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/settings.py#L194'>airflow/settings.py:194:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/settings.py#L299'>airflow/settings.py:299:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/apache/airflow/blob/48b619078d0f7ccf4ed7be79d79618f3bf11e12f/airflow/settings.py#L426'>airflow/settings.py:426:5:</a> FURB154 [*] Use of repeated consecutive `global`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_callbacks__document.py#L168'>tests/unit/bokeh/document/test_callbacks__document.py:168:13:</a> FURB154 [*] Use of repeated consecutive `nonlocal`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/data_import/slack.py#L532'>zerver/data_import/slack.py:532:9:</a> FURB154 [*] Use of repeated consecutive `nonlocal`
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/data_import/slack.py#L604'>zerver/data_import/slack.py:604:9:</a> FURB154 [*] Use of repeated consecutive `nonlocal`
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/lib/cache.py#L63'>zerver/lib/cache.py:63:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/lib/markdown/__init__.py#L2727'>zerver/lib/markdown/__init__.py:2727:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/lib/templates.py#L106'>zerver/lib/templates.py:106:5:</a> FURB154 [*] Use of repeated consecutive `global`
+ <a href='https://github.com/zulip/zulip/blob/9444825941638afc357595f18a40bd5d008693d4/zerver/tests/test_auth_backends.py#L7644'>zerver/tests/test_auth_backends.py:7644:13:</a> FURB154 [*] Use of repeated consecutive `nonlocal`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB154 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-04-29 00:43_

Since globals and nonlocals are rare, could we try instead running this rule on every `global` and `nonlocal` statement, and then, for each statement, mapping back to the containing `Suite` and looking for `global` statements that follow it repeatedly?

In other words... instead of iterating over every body / statement, can we instead run this for each `global`? You can look at `traversal::next_sibling` or `traversal::suite` (and their usages) for an example.


---

_Comment by @alex-700 on 2024-04-29 07:54_

> Since globals and nonlocals are rare, could we try instead running this rule on every `global` and `nonlocal` statement, and then, for each statement, mapping back to the containing `Suite` and looking for `global` statements that follow it repeatedly?
> 
> In other words... instead of iterating over every body / statement, can we instead run this for each `global`? You can look at `traversal::next_sibling` or `traversal::suite` (and their usages) for an example.

@charliermarsh done.
Just a note: now the algorithm is quadratic on `len(suite)` for each suite containing `global` or `nonlocal`. Instead of linear (and should be auto vectorized) for each suite.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-04 15:43_

---

_Label `rule` added by @charliermarsh on 2024-06-04 15:43_

---

_Label `preview` added by @charliermarsh on 2024-06-04 15:43_

---

_Comment by @charliermarsh on 2024-06-04 20:53_

Thanks @alex-700. Blocked on me, will review shortly.


---

_@charliermarsh approved on 2024-06-08 20:30_

---

_Renamed from "[`refurb`] implement repeated_global (FURB154) lint" to "[`refurb`] Implement `repeated-global` (`FURB154`)" by @charliermarsh on 2024-06-08 20:30_

---

_Merged by @charliermarsh on 2024-06-08 20:35_

---

_Closed by @charliermarsh on 2024-06-08 20:35_

---

_Comment by @codspeed-hq[bot] on 2024-06-08 20:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex-700:latyshev/furb154)

### Merging #11187 will **degrade performances by 9.38%**

<sub>Comparing <code>alex-700:latyshev/furb154</code> (d66b3f1) with <code>alex-700:latyshev/furb154</code> (333b7f4)</sub>



### Summary

`⚡ 9` improvements
`❌ 8` regressions
`✅ 13` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex-700:latyshev/furb154)._

### Benchmarks breakdown

|     | Benchmark | `alex-700:latyshev/furb154` | `alex-700:latyshev/furb154` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.4 ms | 1.1 ms | +28.25% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 285.3 µs | 225.8 µs | +26.32% |
| ⚡ | `lexer[numpy/globals.py]` | 36.1 µs | 29.4 µs | +23.08% |
| ⚡ | `lexer[pydantic/types.py]` | 639.6 µs | 502.8 µs | +27.22% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 98.2 µs | 76.6 µs | +28.21% |
| ❌ | `linter/all-rules[large/dataset.py]` | 15.5 ms | 16.1 ms | -4.27% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 2 ms | 2.1 ms | -6.2% |
| ❌ | `linter/default-rules[large/dataset.py]` | 3.5 ms | 3.7 ms | -5.65% |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 879.6 µs | 945.5 µs | -6.97% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.7 ms | 1.9 ms | -9.38% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 341.6 µs | 359.8 µs | -5.07% |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 777 µs | 812.5 µs | -4.37% |
| ❌ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 2.2 ms | 2.4 ms | -6.51% |
| ⚡ | `parser[large/dataset.py]` | 5.6 ms | 5.3 ms | +5.85% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1.1 ms | 1 ms | +7.76% |
| ⚡ | `parser[numpy/globals.py]` | 122.3 µs | 107 µs | +14.27% |
| ⚡ | `parser[unicode/pypinyin.py]` | 372 µs | 341.6 µs | +8.89% |


---

_Comment by @charliermarsh on 2024-06-08 20:45_

I'm confused by the CodSpeed results but my guess is it's just confused by the branch being dated? We didn't touch the parser or lexer here of course.

---

_Branch deleted on 2025-06-10 20:47_

---
