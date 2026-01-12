```yaml
number: 4116
title: Add docs build validation stage to CI
type: pull_request
state: merged
author: calumy
labels: []
assignees: []
merged: true
base: main
head: docs-ci
created_at: 2023-04-26T12:16:58Z
updated_at: 2023-04-26T14:11:38Z
url: https://github.com/astral-sh/ruff/pull/4116
synced_at: 2026-01-12T15:55:14Z
```

# Add docs build validation stage to CI

---

_@calumy_

Following on from #4097, this PR adds a docs build validation stage to CI.

---

_Comment by @calumy on 2023-04-26 12:37_

The first run was pretty slow (see: https://github.com/charliermarsh/ruff/actions/runs/4808551555/jobs/8558675843), but the second run was significantly quicker (see: https://github.com/charliermarsh/ruff/actions/runs/4808652426/jobs/8558902348). This appears to be because it doesn't download and compile a significant number of packages on the second run. I'm not sure if there is any way to cache them so that the first run each time is quicker. Any advice on this would be appreciated. 

---

_Comment by @github-actions[bot] on 2023-04-26 12:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.27ms     2.4 MB/sec    1.02     17.4±0.25ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.01      4.3±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    556.6±8.11µs     5.3 MB/sec    1.00    547.3±7.11µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.08ms     3.5 MB/sec    1.00      7.3±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.06ms     4.6 MB/sec    1.00      8.8±0.10ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1980.2±5.67µs     8.4 MB/sec    1.00  1957.9±29.32µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    218.6±1.22µs    13.5 MB/sec    1.00    212.4±3.96µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.02ms     6.3 MB/sec    1.00      4.0±0.07ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.8±0.12ms     6.0 MB/sec    1.03      7.0±0.08ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1339.2±30.03µs    12.4 MB/sec    1.03  1385.0±16.76µs    12.0 MB/sec
parser/numpy/globals.py                    1.00    132.5±2.99µs    22.3 MB/sec    1.02    135.4±0.77µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.06ms     8.7 MB/sec    1.02      3.0±0.03ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.07     21.6±0.74ms  1928.3 KB/sec    1.00     20.1±0.64ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.4±0.16ms     3.1 MB/sec    1.00      5.2±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.10   668.0±23.72µs     4.4 MB/sec    1.00   607.4±23.89µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.06      9.0±0.37ms     2.8 MB/sec    1.00      8.5±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.09     10.5±0.27ms     3.9 MB/sec    1.00      9.6±0.29ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10      2.4±0.12ms     7.1 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   264.2±13.40µs    11.2 MB/sec    1.01   267.4±24.27µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.11      4.9±0.35ms     5.2 MB/sec    1.00      4.5±0.15ms     5.7 MB/sec
parser/large/dataset.py                    1.02      8.5±0.30ms     4.8 MB/sec    1.00      8.3±0.29ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1631.6±53.30µs    10.2 MB/sec    1.01  1647.5±96.78µs    10.1 MB/sec
parser/numpy/globals.py                    1.00    166.0±6.54µs    17.8 MB/sec    1.05   173.9±11.11µs    17.0 MB/sec
parser/pydantic/types.py                   1.03      3.7±0.16ms     7.0 MB/sec    1.00      3.5±0.18ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-04-26 13:04_

---

_Merged by @MichaReiser on 2023-04-26 13:57_

---

_Closed by @MichaReiser on 2023-04-26 13:57_

---

_Branch deleted on 2023-04-26 14:11_

---
