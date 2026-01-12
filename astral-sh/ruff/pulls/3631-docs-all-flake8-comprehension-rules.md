```yaml
number: 3631
title: "docs: all `flake8-comprehension` rules"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: docs/flake8-comprehension
created_at: 2023-03-20T16:49:56Z
updated_at: 2023-03-22T03:11:20Z
url: https://github.com/astral-sh/ruff/pull/3631
synced_at: 2026-01-12T04:39:45Z
```

# docs: all `flake8-comprehension` rules

---

_Pull request opened by @dhruvmanila on 2023-03-20 16:49_

Created an initial draft for this, will go over them once more and make it ready to review.

#2646 

---

_@dhruvmanila reviewed on 2023-03-20 16:53_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_double_cast_or_process.rs`:34 on 2023-03-20 16:53_

This is done to avoid grouping all the list elements into a single line: https://beta.ruff.rs/docs/rules/unnecessary-double-cast-or-process/

I generated the docs locally and confirmed that the elements are now rendered as required.

---

_@dhruvmanila reviewed on 2023-03-20 16:53_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_map.rs`:35 on 2023-03-20 16:53_

Same as above: https://beta.ruff.rs/docs/rules/unnecessary-map/

---

_Renamed from "docs: all flake8-comprehension rules" to "docs: all `flake8-comprehension` rules" by @dhruvmanila on 2023-03-20 16:55_

---

_Comment by @github-actions[bot] on 2023-03-20 17:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.2±0.02ms     2.9 MB/sec    1.00     14.0±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.4 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.9±1.85µs     6.8 MB/sec    1.00    435.6±2.43µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.0 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.03      8.0±0.01ms     5.1 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1730.2±6.18µs     9.6 MB/sec    1.00   1704.3±3.34µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.2±0.42µs    16.5 MB/sec    1.00    178.5±1.12µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.02ms     6.9 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.4±0.80ms  1944.2 KB/sec    1.03     22.1±0.99ms  1887.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      6.0±0.24ms     2.8 MB/sec    1.00      5.8±0.43ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.05   726.6±28.82µs     4.1 MB/sec    1.00   695.1±26.76µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.07      9.8±0.29ms     2.6 MB/sec    1.00      9.2±0.43ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.04     12.4±0.37ms     3.3 MB/sec    1.00     11.9±0.30ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.6±0.09ms     6.4 MB/sec    1.00      2.5±0.08ms     6.6 MB/sec
linter/default-rules/numpy/globals.py      1.05   301.7±12.69µs     9.8 MB/sec    1.00   286.5±15.16µs    10.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      5.7±0.27ms     4.5 MB/sec    1.00      5.5±0.17ms     4.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-20 20:30_

Awesome, thanks for taking this on. Just request a review from me whenever it's ready :)

---

_Marked ready for review by @dhruvmanila on 2023-03-21 04:11_

---

_Comment by @dhruvmanila on 2023-03-21 04:12_

@charliermarsh Review request

---

_Merged by @charliermarsh on 2023-03-21 14:28_

---

_Closed by @charliermarsh on 2023-03-21 14:28_

---

_Branch deleted on 2023-03-22 03:11_

---
