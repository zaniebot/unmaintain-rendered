```yaml
number: 3688
title: Avoid nested loops in missing_whitespace
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/whitespace
created_at: 2023-03-23T18:09:50Z
updated_at: 2023-03-23T22:17:18Z
url: https://github.com/astral-sh/ruff/pull/3688
synced_at: 2026-01-12T04:39:45Z
```

# Avoid nested loops in missing_whitespace

---

_Pull request opened by @charliermarsh on 2023-03-23 18:09_

## Summary

Small performance oversight: on my machine, this checks takes about 70 seconds to run on CPython. With these changes, it's down to a few hundred milliseconds.


---

_Label `performance` added by @charliermarsh on 2023-03-23 18:09_

---

_Merged by @charliermarsh on 2023-03-23 18:18_

---

_Closed by @charliermarsh on 2023-03-23 18:19_

---

_Branch deleted on 2023-03-23 18:19_

---

_Comment by @github-actions[bot] on 2023-03-23 18:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.06ms     3.0 MB/sec    1.03     14.3±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.03      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.8±1.23µs     6.0 MB/sec    1.03    502.3±1.90µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.03      6.2±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.08      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1634.3±2.00µs    10.2 MB/sec    1.05   1720.3±6.97µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.1±0.35µs    16.4 MB/sec    1.05    189.0±1.22µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.06      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.7±0.05ms     3.2 MB/sec    1.00     12.7±0.07ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.9±1.89µs     8.0 MB/sec    1.02    373.1±1.90µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.05ms     4.7 MB/sec    1.00      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.0±0.06ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1458.0±8.27µs    11.4 MB/sec    1.00   1443.2±9.14µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    153.8±1.44µs    19.2 MB/sec    1.00    151.1±1.80µs    19.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/missing_whitespace.rs`:45 on 2023-03-23 21:36_

Not. I like to use match statements if I have many branches matching on constant values. It gives you compile time evaluation that no teo branches are overlapping (and can be shorter than changing char == comparisons)

---

_@MichaReiser reviewed on 2023-03-23 21:37_

---

_@charliermarsh reviewed on 2023-03-23 22:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/missing_whitespace.rs`:45 on 2023-03-23 22:17_

(I think a lot of this code is a line-by-line port of pycodestyle, for correctness purposes as we were trying to get the test suite passing. Doesn't mean it has to stay that way :))

---
