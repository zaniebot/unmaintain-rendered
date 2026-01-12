```yaml
number: 4309
title: Write diagnostic name when failing to create fix
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: print_diagnostic_name_when_failing_to_create_fix
created_at: 2023-05-09T14:58:38Z
updated_at: 2023-05-09T15:46:41Z
url: https://github.com/astral-sh/ruff/pull/4309
synced_at: 2026-01-12T15:55:15Z
```

# Write diagnostic name when failing to create fix

---

_@konstin_

Tweak to debug some opaque "error: Failed to create fix: Unable to split parenthesized condition" errors i've seen.

---

_Comment by @github-actions[bot] on 2023-05-09 15:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.5±0.12ms     2.8 MB/sec    1.00     14.3±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.04ms     4.8 MB/sec    1.00      3.4±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    417.7±1.09µs     7.1 MB/sec    1.00    416.0±1.29µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.04ms     4.3 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.05ms     5.8 MB/sec    1.00      7.0±0.04ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.7±2.48µs    11.2 MB/sec    1.00   1482.9±4.71µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.7±0.23µs    18.0 MB/sec    1.00    162.0±0.21µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.18      6.4±0.01ms     6.3 MB/sec    1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.15   1220.3±0.83µs    13.6 MB/sec    1.00   1065.3±0.68µs    15.6 MB/sec
parser/numpy/globals.py                    1.11    120.2±0.27µs    24.5 MB/sec    1.00    108.6±0.26µs    27.2 MB/sec
parser/pydantic/types.py                   1.16      2.7±0.00ms     9.5 MB/sec    1.00      2.3±0.01ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     23.4±0.72ms  1777.7 KB/sec    1.00     23.5±0.99ms  1771.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.23ms     2.9 MB/sec    1.03      5.9±0.32ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   660.3±33.50µs     4.5 MB/sec    1.00   663.4±34.01µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.7±0.33ms     2.6 MB/sec    1.06     10.2±0.33ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.5±0.64ms     3.5 MB/sec    1.00     11.5±0.50ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.10ms     7.0 MB/sec    1.01      2.4±0.07ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   276.5±14.40µs    10.7 MB/sec    1.02   282.2±12.36µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.17ms     5.1 MB/sec    1.03      5.1±0.27ms     5.0 MB/sec
parser/large/dataset.py                    1.00      9.1±0.35ms     4.5 MB/sec    1.01      9.2±0.39ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.03  1844.5±63.50µs     9.0 MB/sec    1.00  1795.2±65.04µs     9.3 MB/sec
parser/numpy/globals.py                    1.05    184.2±8.23µs    16.0 MB/sec    1.00    175.1±5.48µs    16.9 MB/sec
parser/pydantic/types.py                   1.02      4.0±0.23ms     6.3 MB/sec    1.00      4.0±0.20ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-05-09 15:12_

---

_@MichaReiser approved on 2023-05-09 15:42_

Nice!

---

_Merged by @konstin on 2023-05-09 15:46_

---

_Closed by @konstin on 2023-05-09 15:46_

---

_Branch deleted on 2023-05-09 15:46_

---
