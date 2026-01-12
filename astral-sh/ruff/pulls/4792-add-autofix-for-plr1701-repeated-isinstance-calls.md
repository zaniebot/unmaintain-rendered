```yaml
number: 4792
title: "Add autofix for `PLR1701` (repeated-isinstance-calls)"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: feat/PLR1701/autofix
created_at: 2023-06-01T18:23:28Z
updated_at: 2023-06-02T02:32:17Z
url: https://github.com/astral-sh/ruff/pull/4792
synced_at: 2026-01-12T03:50:03Z
```

# Add autofix for `PLR1701` (repeated-isinstance-calls)

---

_Pull request opened by @dhruvmanila on 2023-06-01 18:23_

## Summary

Add auto-fix for `PLR1701` (repeated-isinstance-calls)

## Test Plan

* Updated existing snapshot with `cargo test` & `cargo insta review`
* Added new snapshot for Python version 3.9

resolves: #4768


---

_@dhruvmanila reviewed on 2023-06-01 18:33_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/pylint/rules/repeated_isinstance_calls.rs`:67 on 2023-06-01 18:33_

Should we use the AST to build this or is it fine to format it directly?

---

_Comment by @github-actions[bot] on 2023-06-01 18:55_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

```diff
- airflow/utils/dot_renderer.py:122:8: PLR1701 Merge these isinstance calls: `isinstance(node, (BaseOperator, MappedOperator))`
+ airflow/utils/dot_renderer.py:122:8: PLR1701 [*] Merge `isinstance` calls: `isinstance(node, (BaseOperator, MappedOperator))`
- airflow/www/views.py:4236:14: PLR1701 Merge these isinstance calls: `isinstance(items, (DagRun, TaskInstance))`
+ airflow/www/views.py:4236:14: PLR1701 [*] Merge `isinstance` calls: `isinstance(items, (DagRun, TaskInstance))`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR1701 | 4 | 2 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.08ms     2.7 MB/sec    1.00     15.1±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.00ms     4.5 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02    381.3±0.94µs     7.7 MB/sec    1.00    374.8±0.97µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.02ms     5.4 MB/sec    1.00      7.6±0.07ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1614.6±5.90µs    10.3 MB/sec    1.00   1610.2±6.19µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    177.9±0.32µs    16.6 MB/sec    1.00    176.2±0.56µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.02ms     7.4 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.8±0.00ms     7.1 MB/sec    1.01      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1124.5±2.50µs    14.8 MB/sec    1.01   1134.3±1.56µs    14.7 MB/sec
parser/numpy/globals.py                    1.00    116.0±0.34µs    25.4 MB/sec    1.01    116.7±0.64µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.01      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.12ms     2.4 MB/sec    1.00     16.9±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.04ms     3.8 MB/sec    1.00      4.3±0.01ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.7±4.49µs     6.6 MB/sec    1.01   454.4±11.61µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.01      7.3±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.07ms     4.8 MB/sec    1.01      8.6±0.17ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1790.8±16.72µs     9.3 MB/sec    1.26      2.3±0.43ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.8±2.65µs    15.1 MB/sec    1.02    200.4±5.49µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.02ms     6.6 MB/sec    1.03      4.0±0.38ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.6±0.03ms     6.1 MB/sec    1.20      8.0±0.02ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1256.3±20.72µs    13.3 MB/sec    1.17  1465.3±10.05µs    11.4 MB/sec
parser/numpy/globals.py                    1.00    131.1±1.04µs    22.5 MB/sec    1.12    146.3±0.90µs    20.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.1 MB/sec    1.18      3.3±0.02ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-06-01 20:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/repeated_isinstance_calls.rs`:67 on 2023-06-01 20:34_

I think either would be reasonable.

---

_Merged by @charliermarsh on 2023-06-01 20:43_

---

_Closed by @charliermarsh on 2023-06-01 20:43_

---

_@dhruvmanila reviewed on 2023-06-02 02:25_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/pylint/rules/repeated_isinstance_calls.rs`:84 on 2023-06-02 02:25_

@charliermarsh I'm unable to understand why was this moved here 😅

Instead of checking once, this will check for each iteration but I don't think the semantic model changes. Am I missing something here?

---

_Branch deleted on 2023-06-02 02:27_

---

_@charliermarsh reviewed on 2023-06-02 02:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/repeated_isinstance_calls.rs`:84 on 2023-06-02 02:30_

It effectively delays the check until we have at least one `isinstance` call. The result should be the same, since we're `return`ing, but we avoid having to do the `is_builtin` check if none of the calls are `isinstance`.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/pylint/rules/repeated_isinstance_calls.rs`:84 on 2023-06-02 02:32_

Oh right, understood. Thanks!

---

_@dhruvmanila reviewed on 2023-06-02 02:32_

---
