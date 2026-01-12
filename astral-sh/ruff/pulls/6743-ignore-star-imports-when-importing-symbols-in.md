```yaml
number: 6743
title: Ignore star imports when importing symbols in fixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/star-import
created_at: 2023-08-21T23:19:58Z
updated_at: 2023-08-21T23:42:32Z
url: https://github.com/astral-sh/ruff/pull/6743
synced_at: 2026-01-12T02:52:04Z
```

# Ignore star imports when importing symbols in fixes

---

_Pull request opened by @charliermarsh on 2023-08-21 23:19_

## Summary

Given:

```python
from sys import *

exit(0)
```

We can't add `exit` to `from sys import *`, so we should just ignore it. Ideally, we'd just resolve `exit` in the first place (since it's imported from `from sys import *`), but as long as we don't support wildcard imports, this is more consistent.

Closes https://github.com/astral-sh/ruff/issues/6718.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-21 23:20_

---

_Merged by @charliermarsh on 2023-08-21 23:31_

---

_Closed by @charliermarsh on 2023-08-21 23:31_

---

_Branch deleted on 2023-08-21 23:31_

---

_Comment by @github-actions[bot] on 2023-08-21 23:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.4±0.01ms    11.9 MB/sec    1.00      3.4±0.02ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    714.0±3.15µs    23.3 MB/sec    1.00    716.0±2.54µs    23.3 MB/sec
formatter/numpy/globals.py                 1.00     76.9±0.29µs    38.4 MB/sec    1.00     77.3±0.63µs    38.2 MB/sec
formatter/pydantic/types.py                1.01  1399.8±27.37µs    18.2 MB/sec    1.00  1389.8±29.05µs    18.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.03ms     4.0 MB/sec    1.00     10.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.02ms     6.1 MB/sec    1.00      2.7±0.02ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.3±0.60µs     7.7 MB/sec    1.00    385.2±1.00µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.02ms     4.8 MB/sec    1.00      5.3±0.02ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1201.5±13.53µs    13.9 MB/sec    1.00  1202.6±13.08µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.9±0.21µs    20.9 MB/sec    1.00    141.1±0.25µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.5±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      2.8±0.02ms    14.3 MB/sec    1.01      2.9±0.02ms    14.2 MB/sec
formatter/numpy/ctypeslib.py               1.00    562.4±6.67µs    29.6 MB/sec    1.01    568.7±6.16µs    29.3 MB/sec
formatter/numpy/globals.py                 1.00     55.6±0.72µs    53.0 MB/sec    1.04     57.9±1.00µs    50.9 MB/sec
formatter/pydantic/types.py                1.00  1163.5±14.33µs    21.9 MB/sec    1.02  1181.0±24.55µs    21.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.04ms     4.1 MB/sec    1.01     10.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    297.8±3.27µs     9.9 MB/sec    1.01    300.6±5.69µs     9.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.02ms     5.0 MB/sec    1.00      5.1±0.02ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.03ms     7.3 MB/sec    1.01      5.7±0.04ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1150.2±38.94µs    14.5 MB/sec    1.00   1150.6±6.52µs    14.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.7±1.76µs    24.2 MB/sec    1.02    123.5±5.75µs    23.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.01      2.5±0.02ms    10.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
