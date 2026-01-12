```yaml
number: 3841
title: "Combine `operations.rs` and `helpers.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/useless-iii
created_at: 2023-04-01T03:10:00Z
updated_at: 2023-04-01T04:07:22Z
url: https://github.com/astral-sh/ruff/pull/3841
synced_at: 2026-01-12T04:28:19Z
```

# Combine `operations.rs` and `helpers.rs`

---

_Pull request opened by @charliermarsh on 2023-04-01 03:10_

I despise both of these files, but it's confusing that they both exist, since they both contain arbitrary collections of utilities. This PR just combines them to remove _that_ confusion, though all of these utilities need to be properly categorized.


---

_Merged by @charliermarsh on 2023-04-01 03:40_

---

_Closed by @charliermarsh on 2023-04-01 03:40_

---

_Branch deleted on 2023-04-01 03:40_

---

_Comment by @github-actions[bot] on 2023-04-01 03:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.2±0.68ms     2.0 MB/sec    1.00     20.0±0.47ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.23ms     3.4 MB/sec    1.02      4.9±0.27ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   626.5±31.10µs     4.7 MB/sec    1.01   629.8±23.42µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.25ms     3.0 MB/sec    1.03      8.6±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.29ms     3.9 MB/sec    1.00     10.4±0.34ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.08ms     7.4 MB/sec    1.01      2.3±0.08ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.0±11.16µs    11.4 MB/sec    1.08   279.6±15.28µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.13ms     5.4 MB/sec    1.01      4.7±0.15ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.08     20.3±0.85ms     2.0 MB/sec    1.00     18.8±0.85ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.18ms     3.2 MB/sec    1.00      5.1±0.27ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   596.4±25.25µs     4.9 MB/sec    1.02   609.3±26.85µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.05      8.4±0.34ms     3.0 MB/sec    1.00      8.0±0.38ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.56ms     4.1 MB/sec    1.02     10.3±0.39ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.13ms     7.6 MB/sec    1.02      2.2±0.11ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   270.1±15.15µs    10.9 MB/sec    1.00   268.8±14.62µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.5 MB/sec    1.02      4.7±0.20ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
