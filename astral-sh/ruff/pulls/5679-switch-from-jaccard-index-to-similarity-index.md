```yaml
number: 5679
title: Switch from jaccard index to similarity index
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: similarity_index
created_at: 2023-07-11T09:25:34Z
updated_at: 2023-07-11T11:03:45Z
url: https://github.com/astral-sh/ruff/pull/5679
synced_at: 2026-01-12T03:36:55Z
```

# Switch from jaccard index to similarity index

---

_Pull request opened by @konstin on 2023-07-11 09:25_

## Summary

The similarity index, the fraction of unchanged lines, is easier to understand than the jaccard index, the fraction between intersection and union.

## Test Plan

I ran this on django and git a 0.945 index, meaning 5.5% of lines are currently reformatted when compared to black


---

_Marked ready for review by @konstin on 2023-07-11 09:25_

---

_Comment by @github-actions[bot] on 2023-07-11 09:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.01ms     4.8 MB/sec    1.00      8.4±0.01ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1862.4±2.09µs     8.9 MB/sec    1.00   1854.6±4.75µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    199.1±0.52µs    14.8 MB/sec    1.00    199.4±0.19µs    14.8 MB/sec
formatter/pydantic/types.py                1.02      4.2±0.01ms     6.1 MB/sec    1.00      4.1±0.00ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.03ms     2.8 MB/sec    1.00     14.3±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.3±1.29µs     8.0 MB/sec    1.00    368.4±1.18µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1536.9±2.68µs    10.8 MB/sec    1.00   1520.3±1.77µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.5±0.23µs    18.0 MB/sec    1.00    162.7±0.29µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.26ms     3.6 MB/sec    1.01     11.5±0.23ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.07ms     6.6 MB/sec    1.01      2.6±0.06ms     6.5 MB/sec
formatter/numpy/globals.py                 1.01   288.4±14.61µs    10.2 MB/sec    1.00   285.6±12.78µs    10.3 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.15ms     4.6 MB/sec    1.02      5.6±0.13ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.2±0.38ms     2.1 MB/sec    1.01     19.4±0.39ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.16ms     3.3 MB/sec    1.01      5.0±0.34ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   616.6±16.62µs     4.8 MB/sec    1.01   620.8±36.59µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.19ms     2.9 MB/sec    1.00      8.7±0.18ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.17ms     4.1 MB/sec    1.02     10.0±0.24ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.05ms     8.0 MB/sec    1.02      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    245.2±8.04µs    12.0 MB/sec    1.01    248.0±8.27µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.11ms     5.8 MB/sec    1.01      4.5±0.10ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/format_dev.rs`:191 on 2023-07-11 09:54_

Should we print both?

---

_@MichaReiser approved on 2023-07-11 09:54_

---

_@konstin reviewed on 2023-07-11 11:03_

---

_Review comment by @konstin on `crates/ruff_dev/src/format_dev.rs`:191 on 2023-07-11 11:03_

nah i'd keep this simple. if we want we can write an extensive report separately

---

_Merged by @konstin on 2023-07-11 11:03_

---

_Closed by @konstin on 2023-07-11 11:03_

---

_Branch deleted on 2023-07-11 11:03_

---
