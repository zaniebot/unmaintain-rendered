```yaml
number: 9769
title: Skip LibCST parsing for standard dedent adjustments
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2024-02-02T02:12:54Z
updated_at: 2024-02-02T18:24:32Z
url: https://github.com/astral-sh/ruff/pull/9769
synced_at: 2026-01-12T15:55:30Z
```

# Skip LibCST parsing for standard dedent adjustments

---

_@charliermarsh_

## Summary

Often, when fixing, we need to dedent a block of code (e.g., if we remove an `if` and dedent its body). Today, we use LibCST to parse and adjust the indentation, which is really expensive -- but this is only really necessary if the block contains a multiline string, since naively adjusting the indentation for such a string can change the whitespace _within_ the string.

This PR uses a simple dedent implementation for cases in which the block doesn't intersect with a multi-line string (or an f-string, since we don't support tracking multi-line strings for f-strings right now).

We could improve this even further by using the ranges to guide the dedent function, such that we don't apply the dedent if the line starts within a multiline string. But that would also need to take f-strings into account, which is a little tricky.

## Test Plan

`cargo test`


---

_Label `performance` added by @charliermarsh on 2024-02-02 02:13_

---

_Comment by @codspeed-hq[bot] on 2024-02-02 02:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/p)

### Merging #9769 will **improve performances by 29.71%**

<sub>Comparing <code>charlie/p</code> (43e972a) with <code>main</code> (4f7fb56)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/p` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 34.6 ms | 26.7 ms | +29.71% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 63.8 ms | 56.5 ms | +13.07% |


---

_@charliermarsh reviewed on 2024-02-02 02:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 02:27_

I think this was a confusing name. `contains` indicates that the query is: "Does this `MultilineRanges` contain the `TextRange`?"

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-02 02:27_

---

_Comment by @github-actions[bot] on 2024-02-02 02:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -530 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -342 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1001'>tests/utils/test_task_group.py:1001:9:</a> ANN202 Missing return type annotation for private function `task_1`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1003'>tests/utils/test_task_group.py:1003:9:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1006'>tests/utils/test_task_group.py:1006:9:</a> ANN202 Missing return type annotation for private function `task_2`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1008'>tests/utils/test_task_group.py:1008:9:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1011'>tests/utils/test_task_group.py:1011:9:</a> ANN202 Missing return type annotation for private function `task_3`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1013'>tests/utils/test_task_group.py:1013:9:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1016'>tests/utils/test_task_group.py:1016:9:</a> ANN202 Missing return type annotation for private function `task_group1`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1022'>tests/utils/test_task_group.py:1022:9:</a> ANN202 Missing return type annotation for private function `task_group2`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1026'>tests/utils/test_task_group.py:1026:9:</a> ANN202 Missing return type annotation for private function `task_group3`
... 65 additional changes omitted for rule ANN202
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1050'>tests/utils/test_task_group.py:1050:5:</a> S101 Use of `assert` detected
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1053'>tests/utils/test_task_group.py:1053:5:</a> ANN201 Missing return type annotation for public function `test_call_taskgroup_twice`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1055'>tests/utils/test_task_group.py:1055:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1060'>tests/utils/test_task_group.py:1060:9:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1065'>tests/utils/test_task_group.py:1065:9:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1071'>tests/utils/test_task_group.py:1071:9:</a> T201 `print` found
... 15 additional changes omitted for rule T201
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1107'>tests/utils/test_task_group.py:1107:5:</a> S101 Use of `assert` detected
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1110'>tests/utils/test_task_group.py:1110:5:</a> ANN201 Missing return type annotation for public function `test_pass_taskgroup_output_to_task`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1112'>tests/utils/test_task_group.py:1112:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1119'>tests/utils/test_task_group.py:1119:29:</a> ANN001 Missing type annotation for function argument `num`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1121'>tests/utils/test_task_group.py:1121:21:</a> ANN001 Missing type annotation for function argument `i`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1127'>tests/utils/test_task_group.py:1127:19:</a> ANN001 Missing type annotation for function argument `num`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1133'>tests/utils/test_task_group.py:1133:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1135'>tests/utils/test_task_group.py:1135:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1137'>tests/utils/test_task_group.py:1137:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1142'>tests/utils/test_task_group.py:1142:5:</a> ANN201 Missing return type annotation for public function `test_decorator_unknown_args`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1151'>tests/utils/test_task_group.py:1151:5:</a> ANN201 Missing return type annotation for public function `test_decorator_multiple_use_task`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1152'>tests/utils/test_task_group.py:1152:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1168'>tests/utils/test_task_group.py:1168:5:</a> S101 Use of `assert` detected
... 101 additional changes omitted for rule S101
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1177'>tests/utils/test_task_group.py:1177:5:</a> ANN201 Missing return type annotation for public function `test_topological_sort1`
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1184'>tests/utils/test_task_group.py:1184:9:</a> AIR001 Task variable name should match the `task_id`: "A"
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1185'>tests/utils/test_task_group.py:1185:9:</a> AIR001 Task variable name should match the `task_id`: "B"
- <a href='https://github.com/apache/airflow/blob/7324400c6b379d22030a6bd54fb14912c1cb291f/tests/utils/test_task_group.py#L1186'>tests/utils/test_task_group.py:1186:9:</a> AIR001 Task variable name should match the `task_id`: "C"
... 310 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -188 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L100'>zerver/lib/test_helpers.py:100:44:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L100'>zerver/lib/test_helpers.py:100:69:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L100'>zerver/lib/test_helpers.py:100:78:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L104'>zerver/lib/test_helpers.py:104:33:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L104'>zerver/lib/test_helpers.py:104:56:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Optional`
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L104'>zerver/lib/test_helpers.py:104:81:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
... 50 additional changes omitted for rule FA100
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L113'>zerver/lib/test_helpers.py:113:5:</a> D103 Missing docstring in public function
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L121'>zerver/lib/test_helpers.py:121:58:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L131'>zerver/lib/test_helpers.py:131:7:</a> D101 Missing docstring in public class
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L138'>zerver/lib/test_helpers.py:138:39:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L138'>zerver/lib/test_helpers.py:138:39:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L138'>zerver/lib/test_helpers.py:138:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L138'>zerver/lib/test_helpers.py:138:5:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L138'>zerver/lib/test_helpers.py:138:68:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L140'>zerver/lib/test_helpers.py:140:5:</a> D202 [*] No blank lines allowed after function docstring (found 1)
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L140'>zerver/lib/test_helpers.py:140:5:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/zulip/zulip/blob/70f491eae27ff5e66e81f9444e00f4b85bfc5e7f/zerver/lib/test_helpers.py#L140'>zerver/lib/test_helpers.py:140:5:</a> D212 [*] Multi-line docstring summary should start at the first line
... 171 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (53 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S101 | 117 | 0 | 117 | 0 | 0 |
| ANN202 | 70 | 0 | 70 | 0 | 0 |
| FA100 | 55 | 0 | 55 | 0 | 0 |
| AIR001 | 40 | 0 | 40 | 0 | 0 |
| ANN201 | 38 | 0 | 38 | 0 | 0 |
| ANN001 | 28 | 0 | 28 | 0 | 0 |
| D103 | 24 | 0 | 24 | 0 | 0 |
| T201 | 23 | 0 | 23 | 0 | 0 |
| PLC0415 | 18 | 0 | 18 | 0 | 0 |
| COM812 | 14 | 0 | 14 | 0 | 0 |
| SIM117 | 12 | 0 | 12 | 0 | 0 |
| FBT001 | 6 | 0 | 6 | 0 | 0 |
| FBT002 | 5 | 0 | 5 | 0 | 0 |
| PT012 | 5 | 0 | 5 | 0 | 0 |
| PTH118 | 5 | 0 | 5 | 0 | 0 |
| PTH123 | 4 | 0 | 4 | 0 | 0 |
| FIX002 | 4 | 0 | 4 | 0 | 0 |
| TD002 | 4 | 0 | 4 | 0 | 0 |
| TD003 | 4 | 0 | 4 | 0 | 0 |
| D106 | 4 | 0 | 4 | 0 | 0 |
| RET505 | 3 | 0 | 3 | 0 | 0 |
| D101 | 3 | 0 | 3 | 0 | 0 |
| D205 | 3 | 0 | 3 | 0 | 0 |
| ANN401 | 3 | 0 | 3 | 0 | 0 |
| B015 | 2 | 0 | 2 | 0 | 0 |
| CPY001 | 2 | 0 | 2 | 0 | 0 |
| D202 | 2 | 0 | 2 | 0 | 0 |
| D212 | 2 | 0 | 2 | 0 | 0 |
| PLW1514 | 2 | 0 | 2 | 0 | 0 |
| D107 | 2 | 0 | 2 | 0 | 0 |
| D400 | 2 | 0 | 2 | 0 | 0 |
| D415 | 2 | 0 | 2 | 0 | 0 |
| C408 | 2 | 0 | 2 | 0 | 0 |
| PERF401 | 1 | 0 | 1 | 0 | 0 |
| A002 | 1 | 0 | 1 | 0 | 0 |
| A001 | 1 | 0 | 1 | 0 | 0 |
| D100 | 1 | 0 | 1 | 0 | 0 |
| D401 | 1 | 0 | 1 | 0 | 0 |
| D404 | 1 | 0 | 1 | 0 | 0 |
| PTH100 | 1 | 0 | 1 | 0 | 0 |
| PTH120 | 1 | 0 | 1 | 0 | 0 |
| D209 | 1 | 0 | 1 | 0 | 0 |
| PLR0913 | 1 | 0 | 1 | 0 | 0 |
| PLR0917 | 1 | 0 | 1 | 0 | 0 |
| C901 | 1 | 0 | 1 | 0 | 0 |
| PLR0915 | 1 | 0 | 1 | 0 | 0 |
| PLR1702 | 1 | 0 | 1 | 0 | 0 |
| PLR6201 | 1 | 0 | 1 | 0 | 0 |
| PLR2004 | 1 | 0 | 1 | 0 | 0 |
| TD004 | 1 | 0 | 1 | 0 | 0 |
| RET504 | 1 | 0 | 1 | 0 | 0 |
| PLR0914 | 1 | 0 | 1 | 0 | 0 |
| ARG001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_@zanieb reviewed on 2024-02-02 02:46_

---

_Review comment by @zanieb on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 02:46_

But all the rest are still `intersects`?

---

_@charliermarsh reviewed on 2024-02-02 03:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:04_

Sorry, say more?

---

_@zanieb reviewed on 2024-02-02 03:10_

---

_Review comment by @zanieb on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:10_

Why is this method changing from `intersects` to `contains` but there are a bunch of other methods with the same signature that are called `intersects`? I don't quite get it.

---

_@charliermarsh reviewed on 2024-02-02 03:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:13_

Those are entirely new methods with different behavior. The docstrings and implementations are different. (`intersects` calls `target.contains_range(*range)`, while this method calls `range.contains_range(target)`.)

---

_@zanieb reviewed on 2024-02-02 03:26_

---

_Review comment by @zanieb on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:26_

Thanks for clarifying. I guess my point is we have

- `X.contains(Y)` meaning that `Y` is inside of `X` 
- `X.intersects(Y)` meaning that `X` is inside of `Y`

I think the first is relatively obvious but the second is less so. What about `within`?

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:41_

That's not quite what they mean but of course open to ways to make them clearer. `X` is a collection of ranges, and `Y` is a single range. Thus:

- `X.contains(Y)` means that `Y` is contained within an entry in `X`.
- `X.intersects(Y)` means that `Y` intersects with an entry in `X`.

---

_@charliermarsh reviewed on 2024-02-02 03:41_

---

_@charliermarsh reviewed on 2024-02-02 03:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:43_

Perhaps `X.contains(Y)` should be `X.covers(Y)`?

---

_@zanieb reviewed on 2024-02-02 03:49_

---

_Review comment by @zanieb on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 03:49_

Oh I see... `covers` seems a bit clearer. I think I'd have to actually play with this API to have a strong opinion though, so definitely do what you think is best. I'm curious to hear what other people think too.

---

_@charliermarsh reviewed on 2024-02-02 05:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 05:18_

(It might also be clearer if I change the check _within_ `intersects` to `target.intersects(*range)`. It doesn't make a difference in practice, since the `target` can never intersect-but-not-contain a string.)

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/multiline_ranges.rs`:12 on 2024-02-02 06:08_

I expect the method to return `true` if `target` intersects with any multline string. But that's not what the method does. It only returns `true` if `target` is fully contained by one of the multline string ranges. 

Now, the solution is to either:
* change the method to return whether `target` intersects with any multline string and keep the name as is
* Change the name to `contains` if that's the semantic we want.

My general advice here would be that we design the naming similar to the `TextRange` methods to use a single terminology when it comes to ranges.

Edit: That's what @charliermarsh's last comment says too :) 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/textwrap.rs`:145 on 2024-02-02 06:09_

Nit: Why not `indentation_at_offset`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/textwrap.rs`:154 on 2024-02-02 06:10_

I didn't know we even have a `line_ending` method. Neat! 

---

_@MichaReiser approved on 2024-02-02 06:13_

Neat

---

_@charliermarsh reviewed on 2024-02-02 18:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/textwrap.rs`:145 on 2024-02-02 18:04_

Because the text here is the "full line", not the start of the content.

---

_Merged by @charliermarsh on 2024-02-02 18:13_

---

_Closed by @charliermarsh on 2024-02-02 18:13_

---

_Branch deleted on 2024-02-02 18:13_

---
