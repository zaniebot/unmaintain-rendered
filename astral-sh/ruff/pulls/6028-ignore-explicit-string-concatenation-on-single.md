```yaml
number: 6028
title: "Ignore `explicit-string-concatenation` on single line"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: isc-fix
created_at: 2023-07-24T12:16:57Z
updated_at: 2023-07-26T01:15:50Z
url: https://github.com/astral-sh/ruff/pull/6028
synced_at: 2026-01-12T15:55:20Z
```

# Ignore `explicit-string-concatenation` on single line

---

_@tjkuson_

## Summary

Ignore `explicit-string-concatenation` on single line.

Closes #5332.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-24 12:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -2, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ed689f2be90cc8899438be66e3c75c3921e156cb/helm_tests/airflow_aux/test_basic_helm_chart.py#L559'>helm_tests/airflow_aux/test_basic_helm_chart.py:559:52:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
<details><summary>bokeh (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/bokeh/model/docs.py#L74'>src/bokeh/model/docs.py:74:31:</a> ISC003 Explicitly concatenated string should be implicitly concatenated
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| ISC003 | 2 | 0 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.08ms     4.3 MB/sec    1.01      9.5±0.10ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1864.9±3.36µs     8.9 MB/sec    1.01   1889.2±4.86µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    207.0±0.88µs    14.3 MB/sec    1.04    214.4±5.87µs    13.8 MB/sec
formatter/pydantic/types.py                1.06      4.0±0.01ms     6.3 MB/sec    1.00      3.8±0.25ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.08ms     3.1 MB/sec    1.00     13.0±0.13ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.00      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.6±0.65µs     6.9 MB/sec    1.00    426.3±2.22µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.05ms     6.2 MB/sec    1.02      6.7±0.05ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.13   1414.7±5.54µs    11.8 MB/sec    1.00   1257.2±1.75µs    13.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.2±0.31µs    18.5 MB/sec    1.00    157.7±0.65µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.0±0.01ms     8.5 MB/sec    1.00      2.7±0.01ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.22     19.5±0.65ms     2.1 MB/sec    1.00     16.0±0.78ms     2.5 MB/sec
formatter/numpy/ctypeslib.py               1.15      3.6±0.20ms     4.6 MB/sec    1.00      3.2±0.21ms     5.3 MB/sec
formatter/numpy/globals.py                 1.12   397.6±53.09µs     7.4 MB/sec    1.00   356.0±30.99µs     8.3 MB/sec
formatter/pydantic/types.py                1.19      8.1±0.50ms     3.2 MB/sec    1.00      6.8±0.38ms     3.8 MB/sec
linter/all-rules/large/dataset.py          1.03     23.0±0.86ms  1812.0 KB/sec    1.00     22.4±0.76ms  1863.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.8±0.20ms     2.9 MB/sec    1.00      5.7±0.24ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   717.6±78.82µs     4.1 MB/sec    1.02   733.2±81.94µs     4.0 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.5±0.45ms     2.4 MB/sec    1.02     10.7±0.65ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.07     13.1±0.56ms     3.1 MB/sec    1.00     12.2±0.52ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.6±0.11ms     6.5 MB/sec    1.00      2.5±0.10ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.02   307.2±18.93µs     9.6 MB/sec    1.00   302.0±24.03µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.06      5.7±0.32ms     4.5 MB/sec    1.00      5.3±0.26ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_implicit_str_concat/rules/explicit.rs`:50 on 2023-07-25 06:42_

Nit: Can we make this the last check (move it after the `left` and `right` checks as well). Testing whether the range contains a line break isn't extremely expensive, but is more expensive than testing whether the operator or `left` and `right` are of a certain type.

---

_@MichaReiser reviewed on 2023-07-25 06:42_

---

_Merged by @charliermarsh on 2023-07-25 23:20_

---

_Closed by @charliermarsh on 2023-07-25 23:20_

---

_Branch deleted on 2023-07-26 01:15_

---
