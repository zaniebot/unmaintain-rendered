```yaml
number: 4080
title: 03 30 use pyproject toml
type: pull_request
state: closed
author: holzkohlengrill
labels: []
assignees: []
base: main
head: 03-30-use_pyproject-toml
created_at: 2023-04-24T15:15:58Z
updated_at: 2023-04-24T16:07:35Z
url: https://github.com/astral-sh/ruff/pull/4080
synced_at: 2026-01-12T04:28:19Z
```

# 03 30 use pyproject toml

---

_Pull request opened by @holzkohlengrill on 2023-04-24 15:15_

_No description provided._

---

_Closed by @holzkohlengrill on 2023-04-24 15:36_

---

_Comment by @github-actions[bot] on 2023-04-24 16:07_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     18.6±0.78ms     2.2 MB/sec    1.00     18.0±0.51ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec    1.02      4.5±0.15ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   578.8±20.40µs     5.1 MB/sec    1.00   580.6±17.28µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.7±0.26ms     3.3 MB/sec    1.00      7.6±0.22ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      9.2±0.28ms     4.4 MB/sec    1.00      8.9±0.21ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.08ms     8.2 MB/sec    1.00  1991.7±43.66µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   242.6±13.47µs    12.2 MB/sec    1.00    238.5±9.00µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.2±0.14ms     6.1 MB/sec    1.00      4.1±0.14ms     6.2 MB/sec
parser/large/dataset.py                    1.00      7.3±0.16ms     5.6 MB/sec    1.10      8.0±0.17ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1471.8±42.43µs    11.3 MB/sec    1.06  1556.8±42.99µs    10.7 MB/sec
parser/numpy/globals.py                    1.01    149.3±5.61µs    19.8 MB/sec    1.00    147.2±5.85µs    20.1 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.07ms     7.9 MB/sec    1.06      3.4±0.15ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     24.6±0.93ms  1692.2 KB/sec    1.00     24.3±0.84ms  1713.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.2±0.28ms     2.7 MB/sec    1.03      6.3±0.27ms     2.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   728.1±37.38µs     4.1 MB/sec    1.04   753.8±39.49µs     3.9 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.1±0.42ms     2.5 MB/sec    1.03     10.4±0.37ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     12.6±0.53ms     3.2 MB/sec    1.01     12.8±0.43ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.7±0.17ms     6.1 MB/sec    1.00      2.7±0.10ms     6.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   313.4±18.17µs     9.4 MB/sec    1.03   324.2±22.83µs     9.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.6±0.18ms     4.5 MB/sec    1.03      5.8±0.26ms     4.4 MB/sec
parser/large/dataset.py                    1.00      9.7±0.38ms     4.2 MB/sec    1.02      9.9±0.36ms     4.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1899.0±95.20µs     8.8 MB/sec    1.02  1938.3±79.95µs     8.6 MB/sec
parser/numpy/globals.py                    1.03   197.8±11.35µs    14.9 MB/sec    1.00   192.8±10.90µs    15.3 MB/sec
parser/pydantic/types.py                   1.00      4.2±0.20ms     6.1 MB/sec    1.03      4.3±0.23ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
