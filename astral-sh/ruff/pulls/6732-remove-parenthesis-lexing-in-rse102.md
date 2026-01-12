```yaml
number: 6732
title: Remove parenthesis lexing in RSE102
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/args
created_at: 2023-08-21T14:59:37Z
updated_at: 2023-08-21T21:20:19Z
url: https://github.com/astral-sh/ruff/pull/6732
synced_at: 2026-01-12T02:52:04Z
```

# Remove parenthesis lexing in RSE102

---

_Pull request opened by @charliermarsh on 2023-08-21 14:59_

## Summary

Now that we have an `Arguments` node, we can just use the range of the arguments directly to find the parentheses in `raise Error()`.


---

_Label `internal` added by @charliermarsh on 2023-08-21 14:59_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_raise/rules/unnecessary_paren_on_raise_exception.rs`:80 on 2023-08-21 15:03_

```suggestion
            diagnostic.set_fix(Fix::automatic(Edit::range_deletion(
                arguments.range()
            )));
```

---

_@MichaReiser reviewed on 2023-08-21 15:03_

---

_@MichaReiser approved on 2023-08-21 15:03_

---

_Comment by @github-actions[bot] on 2023-08-21 15:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.7±0.14ms    11.0 MB/sec    1.00      3.6±0.15ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   775.3±49.21µs    21.5 MB/sec    1.00   770.9±34.17µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     79.1±5.37µs    37.3 MB/sec    1.00     78.9±4.35µs    37.4 MB/sec
formatter/pydantic/types.py                1.00  1501.8±72.71µs    17.0 MB/sec    1.01  1509.9±60.04µs    16.9 MB/sec
linter/all-rules/large/dataset.py          1.09     13.0±0.35ms     3.1 MB/sec    1.00     11.9±0.47ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      3.4±0.06ms     4.8 MB/sec    1.00      3.2±0.12ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.05   504.1±15.77µs     5.9 MB/sec    1.00   481.3±18.70µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.10      6.9±0.68ms     3.7 MB/sec    1.00      6.3±0.24ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.17      7.3±0.20ms     5.6 MB/sec    1.00      6.2±0.17ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.14  1548.4±43.92µs    10.8 MB/sec    1.00  1363.9±49.34µs    12.2 MB/sec
linter/default-rules/numpy/globals.py      1.04    181.7±8.51µs    16.2 MB/sec    1.00    174.1±7.88µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.10      3.1±0.09ms     8.1 MB/sec    1.00      2.9±0.11ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    10.9 MB/sec    1.01      3.8±0.06ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   767.0±10.24µs    21.7 MB/sec    1.00   768.7±14.74µs    21.7 MB/sec
formatter/numpy/globals.py                 1.00     80.5±1.34µs    36.6 MB/sec    1.01     81.1±2.69µs    36.4 MB/sec
formatter/pydantic/types.py                1.00  1532.8±35.06µs    16.6 MB/sec    1.01  1553.4±28.82µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.08ms     3.3 MB/sec    1.01     12.5±0.10ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.4±7.47µs     6.7 MB/sec    1.00    437.4±7.64µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.09ms     3.9 MB/sec    1.01      6.6±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.8 MB/sec    1.01      7.0±0.05ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1487.1±15.58µs    11.2 MB/sec    1.01  1495.4±21.38µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±4.81µs    17.1 MB/sec    1.01    174.0±4.62µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-21 20:59_

---

_Closed by @charliermarsh on 2023-08-21 20:59_

---

_Branch deleted on 2023-08-21 20:59_

---
