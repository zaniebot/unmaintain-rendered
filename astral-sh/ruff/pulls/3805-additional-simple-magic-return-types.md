```yaml
number: 3805
title: Additional simple magic return types
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: new-magic-method-return-types
created_at: 2023-03-30T03:15:26Z
updated_at: 2023-03-30T15:11:33Z
url: https://github.com/astral-sh/ruff/pull/3805
synced_at: 2026-01-12T15:55:13Z
```

# Additional simple magic return types

---

_@dhruvmanila_

Refer: https://github.com/JelleZijlstra/autotyping/pull/61

---

_Comment by @github-actions[bot] on 2023-03-30 03:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.40ms     2.5 MB/sec    1.06     17.3±0.57ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.16ms     4.0 MB/sec    1.03      4.3±0.17ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   558.8±22.65µs     5.3 MB/sec    1.02   568.6±16.71µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.22ms     3.7 MB/sec    1.04      7.2±0.21ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.26ms     4.8 MB/sec    1.10      9.3±0.36ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1872.6±51.94µs     8.9 MB/sec    1.07      2.0±0.07ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    229.6±9.60µs    12.8 MB/sec    1.06   243.8±14.68µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.14ms     6.5 MB/sec    1.10      4.3±0.19ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.6±0.13ms     2.6 MB/sec    1.00     15.5±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.0±6.73µs     6.7 MB/sec    1.00    436.8±6.99µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.09ms     3.7 MB/sec    1.00      6.7±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.09ms     4.9 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1773.3±11.89µs     9.4 MB/sec    1.00  1780.7±12.47µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.7±4.01µs    15.8 MB/sec    1.00    186.6±3.01µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-30 06:20_

---

_Merged by @charliermarsh on 2023-03-30 12:57_

---

_Closed by @charliermarsh on 2023-03-30 12:57_

---

_Branch deleted on 2023-03-30 15:11_

---
