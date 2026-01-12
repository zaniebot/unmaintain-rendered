```yaml
number: 4591
title: "Move `get_or_import_symbol` onto `Importer`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/importer
created_at: 2023-05-23T01:19:54Z
updated_at: 2023-05-23T01:52:40Z
url: https://github.com/astral-sh/ruff/pull/4591
synced_at: 2026-01-12T03:50:03Z
```

# Move `get_or_import_symbol` onto `Importer`

---

_Pull request opened by @charliermarsh on 2023-05-23 01:19_

No behavioral change, just improving discoverability and colocating relevant logic.

---

_Merged by @charliermarsh on 2023-05-23 01:33_

---

_Closed by @charliermarsh on 2023-05-23 01:33_

---

_Branch deleted on 2023-05-23 01:33_

---

_Comment by @github-actions[bot] on 2023-05-23 01:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.4±0.12ms     2.5 MB/sec    1.00     16.3±0.15ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.1 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    498.8±4.58µs     5.9 MB/sec    1.00    490.4±6.28µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.05ms     3.7 MB/sec    1.00      6.8±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.10ms     5.2 MB/sec    1.00      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1688.3±17.67µs     9.9 MB/sec    1.00  1678.2±19.08µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.7±2.07µs    15.6 MB/sec    1.00    188.7±3.27µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.02ms     7.2 MB/sec    1.00      3.5±0.04ms     7.3 MB/sec
parser/large/dataset.py                    1.00      6.1±0.05ms     6.6 MB/sec    1.00      6.2±0.04ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1210.4±14.38µs    13.8 MB/sec    1.00  1208.5±15.77µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    124.4±1.88µs    23.7 MB/sec    1.00    124.6±1.38µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.04ms     9.6 MB/sec    1.00      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.21ms     2.5 MB/sec    1.01     16.5±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.0 MB/sec    1.00      4.2±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    488.8±7.71µs     6.0 MB/sec    1.00    485.8±8.16µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.08ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.07ms     5.1 MB/sec    1.01      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1681.9±19.62µs     9.9 MB/sec    1.00  1689.3±19.71µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.8±3.11µs    15.9 MB/sec    1.03    191.0±5.50µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.02      3.6±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.08      7.0±0.04ms     5.8 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.07  1309.9±15.61µs    12.7 MB/sec    1.00  1227.6±12.03µs    13.6 MB/sec
parser/numpy/globals.py                    1.06    132.6±2.96µs    22.2 MB/sec    1.00    125.6±1.79µs    23.5 MB/sec
parser/pydantic/types.py                   1.07      3.0±0.03ms     8.6 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
