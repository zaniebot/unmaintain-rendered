```yaml
number: 9775
title: Track top-level module imports in the semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/imp
created_at: 2024-02-02T04:24:10Z
updated_at: 2024-02-02T19:37:21Z
url: https://github.com/astral-sh/ruff/pull/9775
synced_at: 2026-01-10T22:57:09Z
```

# Track top-level module imports in the semantic model

---

_Pull request opened by @charliermarsh on 2024-02-02 04:24_

## Summary

This is a simple idea to avoid unnecessary work in the linter, especially for rules that run on all name and/or all attribute nodes. Imagine a rule like the NumPy deprecation check. If the user never imported `numpy`, we should be able to skip that rule entirely -- whereas today, we do a `resolve_call_path` check on _every_ name in the file. It turns out that there's basically a finite set of modules that we care about, so we now track imports on those modules as explicit flags on the semantic model. In rules that can _only_ ever trigger if those modules were imported, we add a dedicated and extremely cheap check to the top of the rule.

We could consider generalizing this to all modules, but I would expect that not to be much faster than `resolve_call_path`, which is just a hash map lookup on `TextSize` anyway.

It would also be nice to make this declarative, such that rules could declare the modules they care about, the analyzers could call the rules as appropriate. But, I don't think such a design should block merging this.


---

_Label `performance` added by @charliermarsh on 2024-02-02 04:24_

---

_Comment by @codspeed-hq[bot] on 2024-02-02 04:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/imp)

### Merging #9775 will **improve performances by 14.55%**

<sub>Comparing <code>charlie/imp</code> (e09bf0a) with <code>main</code> (c3ca345)</sub>



### Summary

`⚡ 10` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/imp` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 3 ms | 2.9 ms | +4.76% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 26.6 ms | 24.7 ms | +7.91% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 49.9 ms | 45.3 ms | +10.19% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.3 ms | 3.2 ms | +4.97% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 108.1 ms | 94.4 ms | +14.55% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 12.5 ms | 11.8 ms | +5.34% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 13.3 ms | 12.6 ms | +5.16% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 24 ms | 22.2 ms | +8.24% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 56.1 ms | 51.9 ms | +7.97% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 122 ms | 107.6 ms | +13.33% |


---

_Marked ready for review by @charliermarsh on 2024-02-02 04:59_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-02 04:59_

---

_Comment by @charliermarsh on 2024-02-02 05:00_

I suspect the gains here are actually a little under-estimated, because most of our benchmark files import NumPy, and files that _don't_ import NumPy should get a huge boost from this change.

---

_Comment by @github-actions[bot] on 2024-02-02 05:04_

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




---

_Comment by @charliermarsh on 2024-02-02 05:17_

The ecosystem changes are because I gated some of the Pandas rules to require a Pandas import. I can roll that back. It honestly might be an improvement... It's trading false positives for false negatives.

---

_Review requested from @zanieb by @zanieb on 2024-02-02 05:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:181 on 2024-02-02 06:33_

Nit: I would go with a more explicit name: `see_module` or `add_module`. I don't really know what `see` module is. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:187 on 2024-02-02 06:34_

I assume the specific rules still check if `module` is known at their location (that the import is not later in the module). We should mention in the documentation that this is not "location sensitive".

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:106 on 2024-02-02 06:36_

This reads a bit inside-out (`Some(expr)`)

You can use `bool.then` or `bool.then_some`

```suggestion
semantic.seen(Modules::TYPING | Modules::TYPING_EXTENSIONS)
	.then(||semantic.resolve_call_path(expr))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/non_lowercase_variable_in_function.rs`:59 on 2024-02-02 06:38_

What if the entire performance win is because of this unrelated lint rule change :smile: 

---

_@MichaReiser approved on 2024-02-02 06:41_

Very Clever and nice perf improvement, especially for projects that have the frameworks specific rules enabled without using the framework.

I struggle a bit with the "see" and "seen" terminology. It's very generic and to me its unclear what "semantic.see(module)" would mean. 



Should we also consider' builtins'? For if pandas is globally imported, run the panda rules. 

I fear that this otherwise breaks some workflows. For example, I recently recommended the use of builtins to cover the use case where they use `%run other_module.py` where `other_module.py` imports pandas globally. 



---

_Comment by @zanieb on 2024-02-02 16:53_

The ecosystem changes all look like previous false positives. +1 from me.

---

_Comment by @charliermarsh on 2024-02-02 19:37_

@zanieb - I decided to roll back that specific change. We can definitely consider it but it felt wrong to include in this optimization PR. (Some (not all) of the changes were false negatives.)

---

_Merged by @charliermarsh on 2024-02-02 19:37_

---

_Closed by @charliermarsh on 2024-02-02 19:37_

---

_Branch deleted on 2024-02-02 19:37_

---
