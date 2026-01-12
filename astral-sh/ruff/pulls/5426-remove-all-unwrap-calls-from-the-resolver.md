```yaml
number: 5426
title: "Remove all `unwrap` calls from the resolver"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/error-handling
created_at: 2023-06-28T17:53:44Z
updated_at: 2023-06-28T18:24:43Z
url: https://github.com/astral-sh/ruff/pull/5426
synced_at: 2026-01-12T03:36:55Z
```

# Remove all `unwrap` calls from the resolver

---

_Pull request opened by @charliermarsh on 2023-06-28 17:53_

_No description provided._

---

_@charliermarsh reviewed on 2023-06-28 17:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/implicit_imports.rs`:64 on 2023-06-28 17:54_

I don't think this is necessary, because if the file ends with one of these extensions, it'd hit the previous branch?

---

_Merged by @charliermarsh on 2023-06-28 18:06_

---

_Closed by @charliermarsh on 2023-06-28 18:06_

---

_Branch deleted on 2023-06-28 18:06_

---

_Comment by @github-actions[bot] on 2023-06-28 18:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14     11.9±0.82ms     3.4 MB/sec    1.00     10.5±0.32ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.11      2.6±0.08ms     6.4 MB/sec    1.00      2.3±0.18ms     7.2 MB/sec
formatter/numpy/globals.py                 1.05   275.4±10.71µs    10.7 MB/sec    1.00   263.3±13.80µs    11.2 MB/sec
formatter/pydantic/types.py                1.11      5.8±0.21ms     4.4 MB/sec    1.00      5.2±0.29ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±1.50ms     2.4 MB/sec    1.06     17.7±0.75ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.1±0.20ms     4.1 MB/sec    1.00      4.0±0.16ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   479.8±27.37µs     6.1 MB/sec    1.14   547.9±21.64µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.48ms     3.5 MB/sec    1.02      7.4±0.48ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.79ms     4.9 MB/sec    1.03      8.5±0.44ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1621.3±95.29µs    10.3 MB/sec    1.11  1795.4±63.39µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   214.7±12.89µs    13.7 MB/sec    1.00   206.9±12.10µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.21ms     7.1 MB/sec    1.02      3.7±0.11ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.9±0.07ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1436.8±24.25µs    11.6 MB/sec    1.00  1432.5±17.72µs    11.6 MB/sec
formatter/numpy/globals.py                 1.00    153.1±2.32µs    19.3 MB/sec    1.01    154.0±2.54µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.08ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.2±0.08ms     3.6 MB/sec    1.01     11.3±0.13ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.1±0.03ms     5.4 MB/sec    1.00      3.0±0.03ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.02    322.6±5.45µs     9.1 MB/sec    1.00    316.9±6.24µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      5.2±0.07ms     4.9 MB/sec    1.00      5.1±0.04ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.04ms     6.9 MB/sec    1.00      5.9±0.04ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1218.6±9.35µs    13.7 MB/sec    1.00  1223.8±17.53µs    13.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    129.4±1.79µs    22.8 MB/sec    1.00    129.3±1.82µs    22.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.02ms     9.5 MB/sec    1.00      2.7±0.03ms     9.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
