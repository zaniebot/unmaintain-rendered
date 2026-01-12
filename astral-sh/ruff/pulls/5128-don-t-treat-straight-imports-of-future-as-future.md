```yaml
number: 5128
title: "Don't treat straight imports of __future__ as `__future__` imports"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/future
created_at: 2023-06-15T19:42:08Z
updated_at: 2023-06-15T21:18:06Z
url: https://github.com/astral-sh/ruff/pull/5128
synced_at: 2026-01-12T03:43:30Z
```

# Don't treat straight imports of __future__ as `__future__` imports

---

_Pull request opened by @charliermarsh on 2023-06-15 19:42_

## Summary

If you `import __future__`, it's not subject to the same rules as `from __future__ import feature` -- i.e., this is fine:

```python
x = 1

import __future__
```

It doesn't really make sense to treat these as `__future__` imports (though I can't imagine anyone ever does this anyway).


---

_Comment by @konstin on 2023-06-15 19:43_

Does `import __future__` have any effect, or would this be an unused import?

---

_Comment by @charliermarsh on 2023-06-15 19:46_

I think it will correctly get marked as unused.

---

_Comment by @charliermarsh on 2023-06-15 19:46_

I'll add a test for that, to make it explicit.

---

_Comment by @github-actions[bot] on 2023-06-15 19:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.4 MB/sec    1.01      6.4±0.01ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1321.3±1.94µs    12.6 MB/sec    1.02   1342.6±6.07µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    127.5±0.23µs    23.1 MB/sec    1.02    129.6±0.14µs    22.8 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.9 MB/sec    1.02      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.11ms     3.0 MB/sec    1.00     13.5±0.10ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    419.5±2.20µs     7.0 MB/sec    1.00    411.9±0.79µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.7±0.02ms     6.1 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1458.9±1.75µs    11.4 MB/sec    1.00   1423.0±5.69µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.06    167.7±0.44µs    17.6 MB/sec    1.00    158.8±0.96µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.7±0.10ms     5.3 MB/sec    1.00      7.6±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1570.0±19.60µs    10.6 MB/sec    1.00  1576.9±24.80µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    148.6±2.71µs    19.9 MB/sec    1.01    150.5±4.92µs    19.6 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.1 MB/sec    1.00      3.1±0.06ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.04     16.8±0.12ms     2.4 MB/sec    1.00     16.1±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.05ms     3.9 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    501.5±8.63µs     5.9 MB/sec    1.00   494.8±10.21µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.4±0.12ms     3.5 MB/sec    1.00      7.1±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.5±0.08ms     4.8 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1764.9±19.09µs     9.4 MB/sec    1.00  1729.7±17.03µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    199.4±4.24µs    14.8 MB/sec    1.00    198.4±6.51µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.8±0.05ms     6.7 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Don't treat straight imports of __future__ as '__future__' imports" to "Don't treat straight imports of __future__ as `__future__` imports" by @charliermarsh on 2023-06-15 20:45_

---

_Merged by @charliermarsh on 2023-06-15 20:53_

---

_Closed by @charliermarsh on 2023-06-15 20:53_

---

_Branch deleted on 2023-06-15 20:53_

---
