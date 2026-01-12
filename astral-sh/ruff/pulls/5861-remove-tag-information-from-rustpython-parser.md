```yaml
number: 5861
title: Remove tag information from RustPython-Parser dependency
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: internal/remove-tag
created_at: 2023-07-18T14:21:28Z
updated_at: 2023-07-18T15:48:53Z
url: https://github.com/astral-sh/ruff/pull/5861
synced_at: 2026-01-12T15:55:19Z
```

# Remove tag information from RustPython-Parser dependency

---

_@zanieb_

Following https://github.com/astral-sh/RustPython-Parser/pull/27 we now cherry-pick commits onto our fork instead of rebasing our fork on top of the upstream which means we do not overwrite history and a tag is not necessary to preserve the pinned commit.

In the future, we may rewrite the history in our fork. If we do, we should return to tagging the commits.


---

_Label `internal` added by @zanieb on 2023-07-18 14:26_

---

_Comment by @github-actions[bot] on 2023-07-18 14:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.24ms     4.2 MB/sec    1.00      9.5±0.20ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1877.0±6.72µs     8.9 MB/sec    1.00   1878.9±6.83µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    210.5±2.24µs    14.0 MB/sec    1.00    210.7±0.50µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.06ms     6.3 MB/sec    1.02      4.1±0.07ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.02     14.0±0.45ms     2.9 MB/sec    1.00     13.7±0.36ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.11ms     4.9 MB/sec    1.00      3.4±0.10ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.7±1.79µs     6.7 MB/sec    1.00    438.4±1.27µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.26ms     4.1 MB/sec    1.02      6.3±0.20ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.14ms     5.9 MB/sec    1.00      6.8±0.12ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1431.9±3.69µs    11.6 MB/sec    1.01   1439.1±3.65µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.3±0.40µs    18.9 MB/sec    1.01    157.6±0.33µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.4 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     12.3±0.49ms     3.3 MB/sec     1.22     15.0±0.57ms     2.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.09ms     6.1 MB/sec     1.03      2.8±0.11ms     5.9 MB/sec
formatter/numpy/globals.py                 1.00   325.5±10.85µs     9.1 MB/sec     1.01   328.3±16.69µs     9.0 MB/sec
formatter/pydantic/types.py                1.00      6.2±0.25ms     4.1 MB/sec     1.02      6.3±0.19ms     4.0 MB/sec
linter/all-rules/large/dataset.py          1.00     22.7±0.43ms  1834.9 KB/sec     1.00     22.6±0.40ms  1841.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.8±0.14ms     2.9 MB/sec     1.01      5.9±0.25ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.03   670.8±34.45µs     4.4 MB/sec     1.00   654.3±19.08µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.03     10.2±0.29ms     2.5 MB/sec     1.00     10.0±0.23ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.4±0.44ms     3.6 MB/sec     1.04     11.9±0.34ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1964.2±159.05µs     8.5 MB/sec    1.18      2.3±0.06ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.8±19.80µs    12.4 MB/sec     1.16    275.9±9.16µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.31ms     5.9 MB/sec     1.17      5.0±0.15ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-18 15:17_

---

_Merged by @zanieb on 2023-07-18 15:48_

---

_Closed by @zanieb on 2023-07-18 15:48_

---

_Branch deleted on 2023-07-18 15:48_

---
