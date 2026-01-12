```yaml
number: 3645
title: "isort: fix bad interaction between `force-sort-within-sections` and `force-to-top`"
type: pull_request
state: merged
author: bluetech
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: isort-sections-top
created_at: 2023-03-21T11:00:59Z
updated_at: 2023-03-22T18:00:17Z
url: https://github.com/astral-sh/ruff/pull/3645
synced_at: 2026-01-12T04:39:45Z
```

# isort: fix bad interaction between `force-sort-within-sections` and `force-to-top`

---

_Pull request opened by @bluetech on 2023-03-21 11:00_

Fixes #3630.

To ease review I split to 2 commits - simple refactor + the logic change.

I've tested this on our code base, works well now.

---

_Comment by @github-actions[bot] on 2023-03-21 11:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.16ms     2.6 MB/sec    1.01     15.6±0.23ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    568.8±8.12µs     5.2 MB/sec    1.02    577.6±6.66µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.7 MB/sec    1.01      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.06ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1924.4±13.96µs     8.7 MB/sec    1.00  1894.1±21.54µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.6±2.47µs    13.6 MB/sec    1.01    218.7±1.66µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.03ms     6.4 MB/sec    1.00      4.0±0.04ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.06     15.4±0.79ms     2.6 MB/sec     1.00     14.5±0.42ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.2±0.21ms     4.0 MB/sec     1.00      4.0±0.22ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.13   587.4±26.36µs     5.0 MB/sec     1.00   520.6±29.79µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.12      7.2±0.32ms     3.5 MB/sec     1.00      6.4±0.23ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.12      9.5±0.56ms     4.3 MB/sec     1.00      8.4±0.29ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1894.8±155.65µs     8.8 MB/sec    1.00  1787.1±94.85µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   193.4±10.95µs    15.3 MB/sec     1.13   218.1±11.40µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.8 MB/sec     1.10      4.1±0.24ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/isort/sorting.rs`:54 on 2023-03-21 11:25_

I think you can simply this to
```suggestion
    force_to_top.contains(name2).cmp(&force_to_top.contains(name1))
```

---

_@JonathanPlasse reviewed on 2023-03-21 11:25_

---

_@bluetech reviewed on 2023-03-21 13:42_

---

_Review comment by @bluetech on `crates/ruff/src/rules/isort/sorting.rs`:54 on 2023-03-21 13:42_

Changed to something similar, thanks.

---

_Merged by @charliermarsh on 2023-03-22 18:00_

---

_Closed by @charliermarsh on 2023-03-22 18:00_

---

_Label `bug` added by @charliermarsh on 2023-03-22 18:00_

---

_Label `isort` added by @charliermarsh on 2023-03-22 18:00_

---
