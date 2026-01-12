```yaml
number: 6280
title: "Move `find_keyword` helpers onto `Arguments` struct"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/call-args
created_at: 2023-08-02T17:45:09Z
updated_at: 2023-08-02T18:25:22Z
url: https://github.com/astral-sh/ruff/pull/6280
synced_at: 2026-01-12T15:55:21Z
```

# Move `find_keyword` helpers onto `Arguments` struct

---

_@charliermarsh_

## Summary

Similar to #6279, moving some helpers onto the struct in the name of reducing the number of random undiscoverable utilities we have in `helpers.rs`.

Most of the churn is migrating rules to take `ast::ExprCall` instead of the spread call arguments.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-08-02 17:45_

---

_Merged by @charliermarsh on 2023-08-02 17:54_

---

_Closed by @charliermarsh on 2023-08-02 17:54_

---

_Branch deleted on 2023-08-02 17:54_

---

_Comment by @github-actions[bot] on 2023-08-02 17:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>bokeh (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/38d05d037223234aedeb25e35502abf3332bf0b4/src/bokeh/server/views/multi_root_static_handler.py#L58'>src/bokeh/server/views/multi_root_static_handler.py:58:40:</a> PTH206 Replace `.split(os.sep)` with `Path.parts`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH206 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.07ms     4.2 MB/sec    1.01      9.8±0.06ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1939.0±23.48µs     8.6 MB/sec    1.00  1940.8±13.09µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    214.6±2.77µs    13.8 MB/sec    1.02    219.0±4.21µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.04ms     6.2 MB/sec    1.01      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.08ms     3.1 MB/sec    1.01     13.2±0.27ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±5.01µs     6.5 MB/sec    1.00    456.1±1.48µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.06ms     6.0 MB/sec    1.01      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.7±5.23µs    11.6 MB/sec    1.00   1432.9±8.62µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.4±1.46µs    18.7 MB/sec    1.00    157.4±1.43µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.5 MB/sec    1.00      3.0±0.02ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.2±0.46ms     3.3 MB/sec    1.01     12.3±0.44ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.0 MB/sec    1.01      2.4±0.11ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   264.8±17.08µs    11.1 MB/sec    1.03   272.8±23.15µs    10.8 MB/sec
formatter/pydantic/types.py                1.02      5.3±0.32ms     4.8 MB/sec    1.00      5.2±0.20ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.47ms     2.4 MB/sec    1.00     16.8±0.53ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.20ms     3.8 MB/sec    1.01      4.4±0.21ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   530.9±26.69µs     5.6 MB/sec    1.03   545.3±33.41µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.22ms     3.4 MB/sec    1.02      7.6±0.37ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.22ms     4.5 MB/sec    1.00      9.0±0.30ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1841.6±86.24µs     9.0 MB/sec    1.00  1837.2±72.46µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   210.8±14.23µs    14.0 MB/sec    1.00   205.6±11.13µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.23ms     6.3 MB/sec    1.00      4.0±0.16ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
