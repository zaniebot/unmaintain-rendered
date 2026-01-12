```yaml
number: 5843
title: "Update from `join_with` example to `join_comma_separated`"
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: docs
created_at: 2023-07-17T22:21:45Z
updated_at: 2023-07-20T11:28:59Z
url: https://github.com/astral-sh/ruff/pull/5843
synced_at: 2026-01-12T03:30:21Z
```

# Update from `join_with` example to `join_comma_separated`

---

_Pull request opened by @cnpryer on 2023-07-17 22:21_

## Summary

Originally `join_with` was used in the formatters README.md. Now it uses

```rs
f.join_comma_separated(item.end())
    .nodes(elts.iter())
    .finish()
```

## Test Plan

None


---

_Renamed from "Update from join_with example to join_comma_separated" to "Update from `join_with` example to `join_comma_separated`" by @cnpryer on 2023-07-17 22:24_

---

_Comment by @github-actions[bot] on 2023-07-17 22:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.03ms     4.1 MB/sec    1.00      9.9±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1900.8±2.67µs     8.8 MB/sec    1.00   1894.0±5.05µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.6±0.36µs    14.4 MB/sec    1.00    206.0±0.25µs    14.3 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.01ms     6.0 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.09     15.1±0.03ms     2.7 MB/sec    1.00     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      3.7±0.00ms     4.5 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.04    393.9±4.66µs     7.5 MB/sec    1.00    377.8±1.84µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.08      6.7±0.05ms     3.8 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.19      8.3±0.03ms     4.9 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.14   1626.5±4.02µs    10.2 MB/sec    1.00   1424.3±2.92µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.11    163.8±0.25µs    18.0 MB/sec    1.00    147.5±0.30µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.15      3.6±0.01ms     7.1 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.9±0.04ms     4.6 MB/sec    1.00      8.9±0.05ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1692.0±10.40µs     9.8 MB/sec    1.00   1692.4±7.35µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    178.1±2.42µs    16.6 MB/sec    1.00    178.1±3.61µs    16.6 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.04ms     6.8 MB/sec    1.00      3.8±0.02ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     12.3±0.04ms     3.3 MB/sec    1.00     12.4±0.04ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.2 MB/sec    1.00      3.2±0.02ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    342.5±2.99µs     8.6 MB/sec    1.00    341.8±3.14µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.02ms     4.6 MB/sec    1.00      5.5±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1302.8±7.73µs    12.8 MB/sec    1.00  1301.3±15.83µs    12.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    139.5±0.98µs    21.1 MB/sec    1.00    139.5±1.28µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @MichaReiser on 2023-07-18 06:04_

---

_@konstin approved on 2023-07-18 09:01_

---

_Comment by @konstin on 2023-07-18 09:03_

thanks

---

_Merged by @konstin on 2023-07-18 09:03_

---

_Closed by @konstin on 2023-07-18 09:03_

---

_Branch deleted on 2023-07-20 11:28_

---
