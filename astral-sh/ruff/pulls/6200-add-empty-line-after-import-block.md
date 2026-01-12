```yaml
number: 6200
title: "Add empty line after `import` block"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/imports
created_at: 2023-07-31T15:19:27Z
updated_at: 2023-07-31T16:01:47Z
url: https://github.com/astral-sh/ruff/pull/6200
synced_at: 2026-01-12T15:55:20Z
```

# Add empty line after `import` block

---

_@charliermarsh_

## Summary

Ensures that, given:

```python
import os
x = 1
```

We format like:

```python
import os

x = 1
```


---

_Review requested from @konstin by @charliermarsh on 2023-07-31 15:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__function.py.snap`:114 on 2023-07-31 15:19_

Minor compatibility improvement.

---

_@charliermarsh reviewed on 2023-07-31 15:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:127 on 2023-07-31 15:40_

The idea was to only evaluate whether `statement` is a class or function definition once. Now we check the last and current in each iteration. But I guess the check is cheap enough

---

_@MichaReiser approved on 2023-07-31 15:40_

---

_Comment by @github-actions[bot] on 2023-07-31 15:56_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.7±0.15ms     4.7 MB/sec    1.00      8.5±0.08ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1696.6±33.06µs     9.8 MB/sec    1.00  1675.7±30.53µs     9.9 MB/sec
formatter/numpy/globals.py                 1.01    184.1±4.51µs    16.0 MB/sec    1.00    182.9±3.26µs    16.1 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.06ms     7.0 MB/sec    1.00      3.6±0.07ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.4±0.13ms     3.6 MB/sec    1.00     11.4±0.17ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     5.9 MB/sec    1.00      2.8±0.03ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.5±1.22µs     7.7 MB/sec    1.00    382.3±1.49µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.03ms     5.1 MB/sec    1.07      5.3±0.07ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.04ms     6.8 MB/sec    1.02      6.1±0.09ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1244.5±3.98µs    13.4 MB/sec    1.03  1282.2±22.83µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    136.4±0.81µs    21.6 MB/sec    1.00    136.6±0.37µs    21.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.04ms     9.6 MB/sec    1.02      2.7±0.04ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     12.7±0.40ms     3.2 MB/sec    1.00     12.3±0.26ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.5±0.14ms     6.6 MB/sec    1.00      2.4±0.09ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   267.3±13.78µs    11.0 MB/sec    1.02   273.0±37.09µs    10.8 MB/sec
formatter/pydantic/types.py                1.03      5.5±0.34ms     4.7 MB/sec    1.00      5.3±0.31ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.3±0.37ms     2.4 MB/sec    1.00     17.3±0.36ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.16ms     3.7 MB/sec    1.00      4.5±0.14ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   564.3±64.66µs     5.2 MB/sec    1.00   556.4±18.63µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.31ms     3.3 MB/sec    1.00      7.6±0.18ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.23ms     4.3 MB/sec    1.03      9.7±0.25ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1916.6±76.52µs     8.7 MB/sec    1.01  1944.8±60.71µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   230.8±26.91µs    12.8 MB/sec    1.00    229.9±9.72µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.12ms     6.3 MB/sec    1.01      4.1±0.13ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-31 16:01_

---

_Closed by @charliermarsh on 2023-07-31 16:01_

---

_Branch deleted on 2023-07-31 16:01_

---
