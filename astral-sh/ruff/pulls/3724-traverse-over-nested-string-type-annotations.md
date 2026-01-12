```yaml
number: 3724
title: Traverse over nested string type annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/deferred
created_at: 2023-03-24T21:46:25Z
updated_at: 2023-03-26T01:56:11Z
url: https://github.com/astral-sh/ruff/pull/3724
synced_at: 2026-01-12T15:55:13Z
```

# Traverse over nested string type annotations

---

_@charliermarsh_

## Summary

In Python, you can technically nest string type annotations within string type annotations, like:

```py
from typing import List
from pathlib import Path

x: "List['Path']" = []
```

Right now, Ruff doesn't recurse into the second round of parsing when it encounters these nested annotations, largely due to ownership issues. But I threw that problem at @MichaReiser who did my job for me 😅 by re-writing `check_deferred_string_type_definitions` in a way that supports this iterative, nested parsing.

Closes #3655.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-24 21:46_

---

_Comment by @github-actions[bot] on 2023-03-24 22:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.02ms     3.0 MB/sec    1.01     13.8±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.8±1.44µs     5.9 MB/sec    1.01    503.2±0.83µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.01      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.02      7.3±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1641.9±1.44µs    10.1 MB/sec    1.01   1657.3±1.84µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.4±0.41µs    16.1 MB/sec    1.03    188.3±1.44µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.01      3.5±0.01ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     22.6±0.76ms  1847.0 KB/sec    1.00     22.4±0.66ms  1859.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.20ms     2.9 MB/sec    1.04      6.0±0.30ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   742.0±39.05µs     4.0 MB/sec    1.01   748.6±39.13µs     3.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.7±0.41ms     2.6 MB/sec    1.02      9.9±0.39ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.9±0.37ms     3.4 MB/sec    1.02     12.1±0.48ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.6±0.09ms     6.5 MB/sec    1.01      2.6±0.09ms     6.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   304.9±18.38µs     9.7 MB/sec    1.03   313.1±22.86µs     9.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.5±0.20ms     4.6 MB/sec    1.02      5.6±0.23ms     4.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-26 00:33_

---

_Merged by @charliermarsh on 2023-03-26 01:56_

---

_Closed by @charliermarsh on 2023-03-26 01:56_

---

_Branch deleted on 2023-03-26 01:56_

---
