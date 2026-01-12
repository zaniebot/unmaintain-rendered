```yaml
number: 3804
title: "Use multi-fix semantics for `inplace` removal"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/vet
created_at: 2023-03-29T23:40:35Z
updated_at: 2023-03-30T00:27:55Z
url: https://github.com/astral-sh/ruff/pull/3804
synced_at: 2026-01-12T04:39:45Z
```

# Use multi-fix semantics for `inplace` removal

---

_Pull request opened by @charliermarsh on 2023-03-29 23:40_

## Summary

Long ago, we added the ability to "apply a single fix to a string in-place", which we used as a hack to enable multiple autofixes. (In short: we'd apply one fix, look at the fixed code, apply another fix, then tell Ruff that the fix is the replace the entire expression with that fixed output.) Ruff now supports multiple edits per fix, so we can instead model this behavior as two distinct fixes.

While I was here, I also made the `inplace` rule a little more robust.


---

_Comment by @github-actions[bot] on 2023-03-30 00:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     18.6±1.36ms     2.2 MB/sec    1.00     18.0±0.84ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.25ms     3.7 MB/sec    1.00      4.5±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.10   646.1±38.11µs     4.6 MB/sec    1.00   586.1±28.39µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.12      8.3±0.35ms     3.1 MB/sec    1.00      7.4±0.34ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.09     10.0±0.61ms     4.0 MB/sec    1.00      9.2±0.35ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05      2.3±0.13ms     7.3 MB/sec    1.00      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.17   287.5±16.50µs    10.3 MB/sec    1.00    244.9±9.91µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.5±0.29ms     5.6 MB/sec    1.00      4.3±0.27ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.2±0.62ms     2.2 MB/sec    1.12     20.3±0.65ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.23ms     3.5 MB/sec    1.07      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   583.6±27.90µs     5.1 MB/sec    1.10   643.5±29.58µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.33ms     3.2 MB/sec    1.08      8.6±0.32ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.51ms     4.2 MB/sec    1.10     10.7±0.61ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.5 MB/sec    1.06      2.4±0.11ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   276.2±15.90µs    10.7 MB/sec    1.06   293.7±23.46µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.25ms     5.3 MB/sec    1.10      5.2±0.31ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-30 00:16_

---

_Closed by @charliermarsh on 2023-03-30 00:16_

---

_Branch deleted on 2023-03-30 00:16_

---
