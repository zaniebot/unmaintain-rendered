```yaml
number: 4793
title: Include ImportError in non-fixable try-catch imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-error
created_at: 2023-06-01T19:45:29Z
updated_at: 2023-06-01T20:25:50Z
url: https://github.com/astral-sh/ruff/pull/4793
synced_at: 2026-01-12T15:55:16Z
```

# Include ImportError in non-fixable try-catch imports

---

_@charliermarsh_

I think this was just an oversight. The flag exists, but we never set it. Closes #4790.


---

_Merged by @charliermarsh on 2023-06-01 19:53_

---

_Closed by @charliermarsh on 2023-06-01 19:53_

---

_Branch deleted on 2023-06-01 19:53_

---

_Comment by @github-actions[bot] on 2023-06-01 19:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.07ms     2.9 MB/sec    1.00     13.9±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    425.9±1.49µs     6.9 MB/sec    1.00    420.0±4.16µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1498.6±4.00µs    11.1 MB/sec    1.00  1494.0±10.26µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    171.0±0.37µs    17.3 MB/sec    1.00    169.9±0.85µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.06ms     7.9 MB/sec    1.02      5.3±0.01ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1019.3±0.55µs    16.3 MB/sec    1.01   1027.2±2.67µs    16.2 MB/sec
parser/numpy/globals.py                    1.00    105.6±1.58µs    27.9 MB/sec    1.00    105.7±0.27µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.01      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.7±0.31ms     2.3 MB/sec    1.00     17.4±0.22ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.8 MB/sec    1.03      4.5±0.14ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.4±8.07µs     5.8 MB/sec    1.00    511.7±9.05µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.09ms     3.5 MB/sec    1.01      7.4±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.12ms     4.8 MB/sec    1.01      8.6±0.21ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1801.8±18.92µs     9.2 MB/sec    1.00  1810.4±28.43µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.0±3.54µs    14.5 MB/sec    1.01    206.1±5.72µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.9±0.05ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.2 MB/sec    1.07      7.0±0.05ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1229.3±12.46µs    13.5 MB/sec    1.06  1297.2±13.07µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    127.1±3.52µs    23.2 MB/sec    1.03    130.3±2.48µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.1 MB/sec    1.04      2.9±0.04ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
