```yaml
number: 4083
title: "Fix `E713` and `E714` false positives for multiple comparisons"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-e713-a714-false-positive
created_at: 2023-04-24T19:47:58Z
updated_at: 2023-04-25T17:44:47Z
url: https://github.com/astral-sh/ruff/pull/4083
synced_at: 2026-01-12T15:55:14Z
```

# Fix `E713` and `E714` false positives for multiple comparisons

---

_@JonathanPlasse_

- Close #4082

---

_Comment by @github-actions[bot] on 2023-04-24 19:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.8±0.70ms     2.1 MB/sec    1.00     19.7±0.50ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.14ms     3.5 MB/sec    1.00      4.8±0.11ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.0±27.29µs     4.8 MB/sec    1.04   646.2±41.10µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.28ms     3.1 MB/sec    1.01      8.3±0.30ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02     10.1±0.29ms     4.0 MB/sec    1.00      9.8±0.22ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.07ms     7.4 MB/sec    1.00      2.2±0.05ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   269.1±10.95µs    11.0 MB/sec    1.00   268.6±11.70µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.6±0.13ms     5.5 MB/sec    1.00      4.5±0.13ms     5.7 MB/sec
parser/large/dataset.py                    1.01      8.1±0.35ms     5.0 MB/sec    1.00      8.0±0.19ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1550.3±53.24µs    10.7 MB/sec    1.03  1596.3±49.16µs    10.4 MB/sec
parser/numpy/globals.py                    1.02    162.1±7.19µs    18.2 MB/sec    1.00    159.3±5.61µs    18.5 MB/sec
parser/pydantic/types.py                   1.02      3.6±0.18ms     7.2 MB/sec    1.00      3.5±0.15ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.25ms     2.4 MB/sec    1.02     17.4±0.32ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.07ms     3.7 MB/sec    1.01      4.5±0.09ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    550.3±9.05µs     5.4 MB/sec    1.00   547.3±10.63µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.11ms     3.5 MB/sec    1.00      7.3±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      9.1±0.16ms     4.5 MB/sec    1.00      8.9±0.15ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1967.3±27.26µs     8.5 MB/sec    1.00  1925.8±21.02µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    223.8±6.52µs    13.2 MB/sec    1.00    222.6±8.29µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.08ms     6.2 MB/sec    1.00      4.1±0.08ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1317.5±12.11µs    12.6 MB/sec    1.00  1303.3±15.29µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    131.2±1.44µs    22.5 MB/sec    1.01    132.0±1.98µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.03ms     8.6 MB/sec    1.02      3.0±0.05ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E714.py`:9 on 2023-04-25 00:13_

I feel like this one is arguably a false negative, what do you think?

---

_@charliermarsh reviewed on 2023-04-25 00:13_

---

_@JonathanPlasse reviewed on 2023-04-25 11:49_

---

_Review comment by @JonathanPlasse on `crates/ruff/resources/test/fixtures/pycodestyle/E714.py`:9 on 2023-04-25 11:49_

How would you rewrite it?

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E714.py`:9 on 2023-04-25 17:37_

Yeah, I guess you're right -- I have no idea :joy:

---

_@charliermarsh reviewed on 2023-04-25 17:37_

---

_Renamed from "Fix E713 and E714 false positive" to "Fix `E713` and `E714` false positives for multiple comparisons" by @charliermarsh on 2023-04-25 17:37_

---

_Merged by @charliermarsh on 2023-04-25 17:37_

---

_Closed by @charliermarsh on 2023-04-25 17:37_

---

_Branch deleted on 2023-04-25 17:44_

---
