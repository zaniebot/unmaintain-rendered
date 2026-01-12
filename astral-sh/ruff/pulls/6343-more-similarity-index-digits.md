```yaml
number: 6343
title: More similarity index digits
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: more_format_dev_digits
created_at: 2023-08-04T14:40:35Z
updated_at: 2023-08-04T20:20:14Z
url: https://github.com/astral-sh/ruff/pull/6343
synced_at: 2026-01-12T15:55:21Z
```

# More similarity index digits

---

_@konstin_

**Summary** We were at similarity index 0.998 for django, we need more decimal places, now we're at 0.99779.

**Test Plan** n/a


---

_Label `formatter` added by @konstin on 2023-08-04 14:40_

---

_@zanieb approved on 2023-08-04 14:41_

<3

---

_Comment by @MichaReiser on 2023-08-04 14:43_

Lol, what a nice problem to have

---

_Comment by @github-actions[bot] on 2023-08-04 15:08_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.03ms     4.9 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1627.5±17.31µs    10.2 MB/sec    1.00  1610.0±19.73µs    10.3 MB/sec
formatter/numpy/globals.py                 1.01    173.0±0.34µs    17.1 MB/sec    1.00    171.3±0.39µs    17.2 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.03ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     11.2±0.04ms     3.6 MB/sec    1.00     11.2±0.06ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    314.7±1.20µs     9.4 MB/sec    1.00    314.8±1.47µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.2±0.02ms     4.9 MB/sec    1.00      5.2±0.06ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.04ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1117.5±4.99µs    14.9 MB/sec    1.00   1116.4±4.07µs    14.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    115.9±1.34µs    25.5 MB/sec    1.00    115.4±1.72µs    25.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.03ms    10.5 MB/sec    1.00      2.4±0.02ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.11ms     4.2 MB/sec    1.00      9.8±0.11ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1890.7±20.06µs     8.8 MB/sec    1.00  1895.1±22.57µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    212.3±4.77µs    13.9 MB/sec    1.01    214.3±6.56µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.2 MB/sec    1.00      4.1±0.07ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.10ms     3.1 MB/sec    1.00     13.2±0.12ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.9±6.00µs     6.8 MB/sec    1.00    429.6±6.29µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.00      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.05ms     6.0 MB/sec    1.00      6.8±0.06ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1396.6±16.95µs    11.9 MB/sec    1.00  1391.5±16.72µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.1±2.79µs    18.3 MB/sec    1.00    160.4±4.20µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.04ms     8.5 MB/sec    1.00      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @konstin on 2023-08-04 15:12_

---

_Closed by @konstin on 2023-08-04 15:12_

---

_Branch deleted on 2023-08-04 15:12_

---

_Comment by @davidszotten on 2023-08-04 20:01_

maybe it's time to show the number of mismatches instead? (at least if say >99% matching)

---

_Comment by @konstin on 2023-08-04 20:20_

i really want to switch to -log10'ing the scores and confuse everyone again :D 

On a more serious note: yeah that's a good idea, we should consider showing absolute counts. A bit later after we fixed the outstanding compatibility bugs and the remaining differences are acceptable or intentional, we can switch to a fixed count of deviations that prevents regressions.

---
