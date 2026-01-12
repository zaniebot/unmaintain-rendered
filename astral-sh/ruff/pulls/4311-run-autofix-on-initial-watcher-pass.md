```yaml
number: 4311
title: Run autofix on initial watcher pass
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/initial-fix
created_at: 2023-05-09T16:25:04Z
updated_at: 2023-05-09T17:01:02Z
url: https://github.com/astral-sh/ruff/pull/4311
synced_at: 2026-01-12T03:56:39Z
```

# Run autofix on initial watcher pass

---

_Pull request opened by @charliermarsh on 2023-05-09 16:25_

I think this must've been an oversight -- we weren't passing `autofix` to the initial execution, only to subsequent executions.

---

_@MichaReiser approved on 2023-05-09 16:26_

Looks good. Is there a way that we can test this change to ensure it now works as expected, or can we even add a test? 

---

_Comment by @charliermarsh on 2023-05-09 16:35_

I did some manual testing, but I'm going to punt on automated testing for this, since it sounds complicated 🙈 

---

_Merged by @charliermarsh on 2023-05-09 16:35_

---

_Closed by @charliermarsh on 2023-05-09 16:35_

---

_Branch deleted on 2023-05-09 16:35_

---

_Comment by @github-actions[bot] on 2023-05-09 16:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.13ms     2.5 MB/sec    1.00     16.5±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    493.9±1.15µs     6.0 MB/sec    1.00    490.5±4.35µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.04ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.03      8.4±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1764.0±6.39µs     9.4 MB/sec    1.00   1772.2±8.02µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.3±0.67µs    15.3 MB/sec    1.01    194.6±1.23µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     7.0 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.5±0.06ms     6.2 MB/sec    1.18      7.7±0.01ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1277.7±11.42µs    13.0 MB/sec    1.15   1464.2±3.89µs    11.4 MB/sec
parser/numpy/globals.py                    1.00    129.4±1.33µs    22.8 MB/sec    1.09    141.0±2.18µs    20.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.2 MB/sec    1.15      3.2±0.03ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.10     23.2±0.89ms  1793.7 KB/sec    1.00     21.1±0.66ms  1971.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      5.8±0.31ms     2.8 MB/sec    1.00      5.4±0.30ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.06   685.5±33.86µs     4.3 MB/sec    1.00   648.5±31.30µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.08      9.9±0.50ms     2.6 MB/sec    1.00      9.1±0.51ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     11.4±0.64ms     3.6 MB/sec    1.00     11.3±0.34ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.12ms     6.8 MB/sec    1.00      2.4±0.09ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   285.3±15.08µs    10.3 MB/sec    1.04   297.6±21.73µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.28ms     5.0 MB/sec    1.02      5.2±0.20ms     4.9 MB/sec
parser/large/dataset.py                    1.00      9.7±0.45ms     4.2 MB/sec    1.02      9.9±0.32ms     4.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1830.9±54.70µs     9.1 MB/sec    1.01  1855.2±112.65µs     9.0 MB/sec
parser/numpy/globals.py                    1.05   187.3±10.99µs    15.8 MB/sec    1.00   179.0±11.64µs    16.5 MB/sec
parser/pydantic/types.py                   1.04      4.0±0.21ms     6.3 MB/sec    1.00      3.9±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
