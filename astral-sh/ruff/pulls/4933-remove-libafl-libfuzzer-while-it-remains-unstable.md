```yaml
number: 4933
title: Remove libafl_libfuzzer while it remains unstable
type: pull_request
state: merged
author: addisoncrump
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2023-06-07T17:11:08Z
updated_at: 2023-06-07T17:56:43Z
url: https://github.com/astral-sh/ruff/pull/4933
synced_at: 2026-01-12T15:55:17Z
```

# Remove libafl_libfuzzer while it remains unstable

---

_@addisoncrump_

#4928 shows that this wasn't as stable as I thought. We can revisit this when it's stable, but for now it's probably best to omit these dependencies -- I'll just enable them locally.

---

_Renamed from "Remove libafl while it remains unstable" to "Remove libafl_libfuzzer while it remains unstable" by @addisoncrump on 2023-06-07 17:11_

---

_Comment by @github-actions[bot] on 2023-06-07 17:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.4±0.02ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1071.8±3.71µs    15.5 MB/sec    1.00   1075.9±3.28µs    15.5 MB/sec
formatter/numpy/globals.py                 1.00    119.8±0.21µs    24.6 MB/sec    1.00    119.9±0.10µs    24.6 MB/sec
formatter/pydantic/types.py                1.00      2.4±0.00ms    10.7 MB/sec    1.00      2.4±0.01ms    10.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.03ms     2.9 MB/sec    1.00     13.8±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    408.5±0.36µs     7.2 MB/sec    1.01    410.9±0.79µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.3±1.35µs    11.6 MB/sec    1.00   1445.2±1.57µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.5±0.30µs    18.7 MB/sec    1.00    157.1±0.18µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.20ms     4.9 MB/sec    1.06      8.8±0.24ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1596.2±63.87µs    10.4 MB/sec    1.05  1675.7±60.06µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    175.0±7.96µs    16.9 MB/sec    1.06   185.6±11.90µs    15.9 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.13ms     7.1 MB/sec    1.07      3.8±0.12ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     20.8±0.38ms  2002.8 KB/sec    1.00     20.7±0.38ms  2009.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.13ms     3.2 MB/sec    1.01      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   610.9±21.39µs     4.8 MB/sec    1.00   603.0±14.19µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.19ms     2.9 MB/sec    1.00      8.8±0.19ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.20ms     4.0 MB/sec    1.00     10.1±0.18ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.7 MB/sec    1.00      2.2±0.06ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.5±12.55µs    12.0 MB/sec    1.00    246.4±9.66µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.6±0.23ms     5.5 MB/sec    1.00      4.6±0.11ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-06-07 17:22_

---

_Comment by @MichaReiser on 2023-06-07 17:56_

Thanks!

---

_Merged by @MichaReiser on 2023-06-07 17:56_

---

_Closed by @MichaReiser on 2023-06-07 17:56_

---

_Label `internal` added by @MichaReiser on 2023-06-07 17:56_

---
