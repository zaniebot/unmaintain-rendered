```yaml
number: 6820
title: Respect own-line leading comments before parenthesized nodes
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/paren-comments
created_at: 2023-08-23T17:09:38Z
updated_at: 2023-08-25T04:18:06Z
url: https://github.com/astral-sh/ruff/pull/6820
synced_at: 2026-01-12T02:45:38Z
```

# Respect own-line leading comments before parenthesized nodes

---

_Pull request opened by @charliermarsh on 2023-08-23 17:09_

## Summary

This PR ensures that if an expression has an own-line leading comment _before_ its open parentheses, we render it as such.

For example, given:

```python
[ # foo
    # bar
    ( # baz
        1
    )
]
```

On `main`, we format as:

```python
[  # foo
    (
        # bar
        # baz
        1
    )
]
```

As of this PR, we format as:

```python
[  # foo
    # bar
    (  # baz
        1
    )
]
```

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-23 17:40_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.04ms    11.5 MB/sec    1.00      3.5±0.02ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   706.5±13.84µs    23.6 MB/sec    1.00   709.9±14.96µs    23.5 MB/sec
formatter/numpy/globals.py                 1.00     73.9±0.29µs    39.9 MB/sec    1.00     74.2±0.80µs    39.8 MB/sec
formatter/pydantic/types.py                1.01  1430.2±19.12µs    17.8 MB/sec    1.00  1420.9±13.35µs    17.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.04ms     3.9 MB/sec    1.00     10.4±0.03ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.03ms     5.8 MB/sec    1.00      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    322.1±1.21µs     9.2 MB/sec    1.00    318.1±1.56µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.13ms     4.7 MB/sec    1.00      5.4±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.3 MB/sec    1.00      5.5±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1177.1±5.68µs    14.1 MB/sec    1.00   1179.8±2.97µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    123.0±0.35µs    24.0 MB/sec    1.00    122.0±0.34µs    24.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.04ms    11.0 MB/sec    1.01      3.8±0.06ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   738.9±18.02µs    22.5 MB/sec    1.01   744.9±25.65µs    22.4 MB/sec
formatter/numpy/globals.py                 1.00     75.8±3.27µs    38.9 MB/sec    1.00     76.1±2.08µs    38.8 MB/sec
formatter/pydantic/types.py                1.00  1505.7±36.84µs    16.9 MB/sec    1.01  1520.2±24.72µs    16.8 MB/sec
linter/all-rules/large/dataset.py          1.00     11.5±0.21ms     3.5 MB/sec    1.01     11.6±0.24ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.1±0.05ms     5.3 MB/sec    1.00      3.1±0.05ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    386.8±7.89µs     7.6 MB/sec    1.01    390.8±9.68µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.14ms     4.2 MB/sec    1.00      6.0±0.33ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.4±0.18ms     6.3 MB/sec    1.00      6.3±0.06ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1347.2±24.18µs    12.4 MB/sec    1.01  1354.7±24.37µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.3±3.58µs    19.0 MB/sec    1.00    155.9±3.20µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.05ms     8.9 MB/sec    1.00      2.9±0.05ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-24 12:16_

---

_Label `formatter` added by @MichaReiser on 2023-08-24 12:22_

---

_Merged by @charliermarsh on 2023-08-25 04:18_

---

_Closed by @charliermarsh on 2023-08-25 04:18_

---

_Branch deleted on 2023-08-25 04:18_

---
