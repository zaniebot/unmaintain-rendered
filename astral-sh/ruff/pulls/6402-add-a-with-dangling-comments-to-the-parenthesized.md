```yaml
number: 6402
title: "Add a `with_dangling_comments` to the parenthesized formatter"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: charlie/with_dangling_comments
created_at: 2023-08-07T19:04:38Z
updated_at: 2023-08-07T19:41:46Z
url: https://github.com/astral-sh/ruff/pull/6402
synced_at: 2026-01-12T02:52:04Z
```

# Add a `with_dangling_comments` to the parenthesized formatter

---

_Pull request opened by @charliermarsh on 2023-08-07 19:04_

See: https://github.com/astral-sh/ruff/pull/6376#discussion_r1285514328.

---

_Label `internal` added by @charliermarsh on 2023-08-07 19:04_

---

_Label `formatter` added by @charliermarsh on 2023-08-07 19:04_

---

_Merged by @charliermarsh on 2023-08-07 19:12_

---

_Closed by @charliermarsh on 2023-08-07 19:12_

---

_Branch deleted on 2023-08-07 19:12_

---

_Comment by @github-actions[bot] on 2023-08-07 19:41_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1626.5±40.87µs    10.2 MB/sec    1.00  1632.7±36.28µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    182.1±3.37µs    16.2 MB/sec    1.00    182.8±3.21µs    16.1 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.06ms     7.4 MB/sec    1.01      3.5±0.07ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.05ms     4.0 MB/sec    1.00     10.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.03ms     6.2 MB/sec    1.01      2.7±0.01ms     6.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.7±1.69µs     7.8 MB/sec    1.01    380.9±4.14µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.6±0.02ms     5.5 MB/sec    1.00      4.6±0.01ms     5.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.3±0.02ms     7.7 MB/sec    1.00      5.3±0.01ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1128.8±3.10µs    14.8 MB/sec    1.01   1134.6±7.00µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    127.9±1.64µs    23.1 MB/sec    1.00    128.0±0.55µs    23.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.01ms    10.9 MB/sec    1.01      2.4±0.04ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.5±0.40ms     3.3 MB/sec    1.04     13.0±0.80ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.11ms     7.0 MB/sec    1.01      2.4±0.20ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   261.4±19.30µs    11.3 MB/sec    1.05   275.6±26.91µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.26ms     4.8 MB/sec    1.01      5.3±0.24ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.75ms     2.5 MB/sec    1.05     17.0±0.70ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.14ms     3.9 MB/sec    1.11      4.8±0.38ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   551.0±50.85µs     5.4 MB/sec    1.03   569.4±63.09µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.61ms     3.3 MB/sec    1.03      8.0±0.50ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.22ms     4.7 MB/sec    1.03      8.9±0.41ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1724.9±47.03µs     9.7 MB/sec    1.04  1792.4±85.77µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   204.1±24.15µs    14.5 MB/sec    1.03   210.7±19.10µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.15ms     6.7 MB/sec    1.02      3.9±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
