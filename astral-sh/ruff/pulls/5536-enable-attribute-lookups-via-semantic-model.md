```yaml
number: 5536
title: Enable attribute lookups via semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/attributes
created_at: 2023-07-05T18:51:52Z
updated_at: 2023-07-05T19:40:18Z
url: https://github.com/astral-sh/ruff/pull/5536
synced_at: 2026-01-12T15:55:18Z
```

# Enable attribute lookups via semantic model

---

_@charliermarsh_

## Summary

This PR enables us to resolve attribute accesses within files, at least for static and class methods. For example, we can now detect that this is a function access (and avoid a false-positive):

```python
class Class:
    @staticmethod
    def error():
        return ValueError("Something")


# OK
raise Class.error()
```

Closes #5487.

Closes #5416.


---

_Label `bug` added by @charliermarsh on 2023-07-05 18:51_

---

_Comment by @github-actions[bot] on 2023-07-05 19:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.11ms     4.4 MB/sec    1.02      9.4±0.07ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     8.0 MB/sec    1.01      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    231.0±3.63µs    12.8 MB/sec    1.02    234.9±7.05µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.03ms     5.7 MB/sec    1.01      4.5±0.02ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.12ms     2.6 MB/sec    1.00     15.8±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.02      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.9±7.85µs     5.9 MB/sec    1.03    520.0±1.75µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.01      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.0±13.66µs     9.6 MB/sec    1.02  1774.6±15.20µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.2±2.03µs    15.0 MB/sec    1.00    198.1±2.65µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.02      3.7±0.03ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.9±0.61ms     3.2 MB/sec    1.03     13.2±0.57ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.8±0.15ms     5.9 MB/sec    1.00      2.8±0.14ms     6.0 MB/sec
formatter/numpy/globals.py                 1.02   334.7±27.63µs     8.8 MB/sec    1.00   326.7±24.67µs     9.0 MB/sec
formatter/pydantic/types.py                1.03      6.3±0.31ms     4.0 MB/sec    1.00      6.2±0.34ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     22.1±0.86ms  1886.8 KB/sec    1.05     23.2±0.74ms  1799.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.33ms     3.0 MB/sec    1.01      5.7±0.22ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   706.4±61.09µs     4.2 MB/sec    1.02   723.9±47.52µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.47ms     2.7 MB/sec    1.06     10.0±0.46ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.3±0.62ms     3.6 MB/sec    1.04     11.6±0.43ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.13ms     7.1 MB/sec    1.05      2.5±0.10ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   284.2±20.25µs    10.4 MB/sec    1.05   298.3±23.24µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.37ms     5.0 MB/sec    1.04      5.2±0.18ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 19:19_

---

_Closed by @charliermarsh on 2023-07-05 19:19_

---

_Branch deleted on 2023-07-05 19:19_

---
