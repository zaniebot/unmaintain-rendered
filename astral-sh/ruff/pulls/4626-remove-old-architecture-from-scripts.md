```yaml
number: 4626
title: Remove old architecture from scripts
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: remove-old-architecture-from-scripts
created_at: 2023-05-24T12:08:56Z
updated_at: 2023-05-24T16:22:44Z
url: https://github.com/astral-sh/ruff/pull/4626
synced_at: 2026-01-12T03:50:03Z
```

# Remove old architecture from scripts

---

_Pull request opened by @JonathanPlasse on 2023-05-24 12:08_

- Folded into #3747

Depends on
- #4589
- #3747

This removes the special handling for the old architecture.

---

_Comment by @github-actions[bot] on 2023-05-24 12:41_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.15ms     2.7 MB/sec    1.00     15.3±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    381.2±3.92µs     7.7 MB/sec    1.00    377.1±1.24µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.04ms     4.0 MB/sec    1.00      6.3±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.04      7.9±0.04ms     5.2 MB/sec    1.00      7.5±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1668.4±4.94µs    10.0 MB/sec    1.00   1612.3±5.03µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    182.2±0.79µs    16.2 MB/sec    1.00    180.0±5.50µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.5±0.01ms     7.2 MB/sec    1.00      3.4±0.03ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.2 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1122.5±0.61µs    14.8 MB/sec    1.00   1123.7±2.32µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    114.8±0.30µs    25.7 MB/sec    1.01    115.8±0.24µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.4±0.00ms    10.4 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.2±0.38ms     2.0 MB/sec    1.05     21.1±0.64ms  1971.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.24ms     3.2 MB/sec    1.01      5.2±0.12ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   614.4±17.89µs     4.8 MB/sec    1.02   625.7±21.60µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.22ms     3.0 MB/sec    1.02      8.8±0.26ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.52ms     4.0 MB/sec    1.01     10.4±0.29ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.7 MB/sec    1.01      2.2±0.05ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    247.2±8.96µs    11.9 MB/sec    1.08   268.2±21.76µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.14ms     5.6 MB/sec    1.00      4.6±0.11ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.0±0.24ms     5.1 MB/sec    1.01      8.1±0.16ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1551.1±85.91µs    10.7 MB/sec    1.00  1554.0±81.79µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    153.6±5.24µs    19.2 MB/sec    1.05    160.7±8.24µs    18.4 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.12ms     7.3 MB/sec    1.01      3.5±0.10ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @JonathanPlasse on 2023-05-24 15:39_

---

_Branch deleted on 2023-05-24 15:39_

---
