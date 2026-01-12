```yaml
number: 5525
title: "Refactor tokens-based rules to take an `&mut Vec<Diagnostic>`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extends
created_at: 2023-07-05T04:33:31Z
updated_at: 2023-07-05T23:21:43Z
url: https://github.com/astral-sh/ruff/pull/5525
synced_at: 2026-01-12T15:55:18Z
```

# Refactor tokens-based rules to take an `&mut Vec<Diagnostic>`

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-07-05 04:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.32ms     4.2 MB/sec    1.03     10.0±0.29ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.09ms     7.7 MB/sec    1.02      2.2±0.07ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   246.2±14.32µs    12.0 MB/sec    1.06   260.4±65.98µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.21ms     5.4 MB/sec    1.01      4.7±0.18ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     17.7±0.64ms     2.3 MB/sec    1.00     17.7±0.57ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.12ms     3.9 MB/sec    1.00      4.3±0.14ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   562.0±18.65µs     5.2 MB/sec    1.00   550.0±19.36µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.7±0.24ms     3.3 MB/sec    1.00      7.6±0.29ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.6±0.21ms     4.7 MB/sec    1.00      8.5±0.19ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1877.6±37.24µs     8.9 MB/sec    1.00  1823.8±50.23µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    217.7±8.67µs    13.6 MB/sec    1.00    215.8±9.18µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.12ms     6.6 MB/sec    1.00      3.8±0.14ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.39ms     3.8 MB/sec    1.00     10.6±0.37ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.4 MB/sec    1.00      2.2±0.12ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00   252.9±21.26µs    11.7 MB/sec    1.05   264.4±25.70µs    11.2 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.22ms     5.2 MB/sec    1.01      4.9±0.24ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.03     17.9±0.52ms     2.3 MB/sec    1.00     17.3±0.57ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.8±0.16ms     3.5 MB/sec    1.00      4.5±0.18ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.06   587.2±25.37µs     5.0 MB/sec    1.00   551.5±27.28µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.05      8.1±0.22ms     3.2 MB/sec    1.00      7.7±0.31ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      9.0±0.31ms     4.5 MB/sec    1.00      8.9±0.32ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1912.7±75.85µs     8.7 MB/sec    1.00  1906.1±74.69µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   227.4±13.24µs    13.0 MB/sec    1.00   224.5±14.43µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.15ms     6.3 MB/sec    1.00      4.0±0.17ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/extraneous_parentheses.rs`:137 on 2023-07-05 05:19_

Nit: I would have kept tokens as the first argument because it is what the rules primarily operate on. Diagnostics is more a context argument, similar to settings 

---

_@MichaReiser approved on 2023-07-05 05:19_

---

_Comment by @MichaReiser on 2023-07-05 05:20_

What's the motivation for this change?

---

_Comment by @charliermarsh on 2023-07-05 23:12_

I want to migrate towards this consistent API, I think it will aid in future refactors -- and it should be more efficient too since we don't have to `collect` an intermediate result.

---

_Merged by @charliermarsh on 2023-07-05 23:21_

---

_Closed by @charliermarsh on 2023-07-05 23:21_

---

_Branch deleted on 2023-07-05 23:21_

---
