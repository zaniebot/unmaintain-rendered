```yaml
number: 5008
title: Improve ruff_parse_simple to find UTF-8 violations
type: pull_request
state: merged
author: addisoncrump
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-06-10T16:53:14Z
updated_at: 2023-06-12T16:10:24Z
url: https://github.com/astral-sh/ruff/pull/5008
synced_at: 2026-01-12T15:55:17Z
```

# Improve ruff_parse_simple to find UTF-8 violations

---

_@addisoncrump_

Improves the `ruff_parse_simple` fuzz harness by adding checks for parsed locations to ensure they all lie on UTF-8 character boundaries. This will allow for faster identification of issues like #5004.

This also adds additional details for Apple M1 users and clarifies the importance of using `init-fuzzer.sh` (thanks for the feedback, @jasikpark :slightly_smiling_face:).

---

_Comment by @github-actions[bot] on 2023-06-10 17:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.26ms     4.8 MB/sec    1.00      8.4±0.29ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1745.7±57.59µs     9.5 MB/sec    1.01  1762.7±75.32µs     9.4 MB/sec
formatter/numpy/globals.py                 1.01    171.1±8.84µs    17.2 MB/sec    1.00    170.0±5.71µs    17.4 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.11ms     7.4 MB/sec    1.00      3.4±0.10ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.40ms     2.2 MB/sec    1.01     18.9±0.40ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.18ms     3.8 MB/sec    1.00      4.4±0.14ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   550.9±20.58µs     5.4 MB/sec    1.00   547.8±16.07µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.23ms     3.3 MB/sec    1.00      7.8±0.20ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      9.1±0.19ms     4.5 MB/sec    1.00      9.0±0.23ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1911.1±42.84µs     8.7 MB/sec    1.00  1903.7±39.12µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.04   228.2±19.48µs    12.9 MB/sec    1.00    220.2±8.56µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.1±0.15ms     6.2 MB/sec    1.00      4.0±0.11ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15      8.8±0.08ms     4.6 MB/sec    1.00      7.7±0.14ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.12  1746.5±19.31µs     9.5 MB/sec    1.00  1562.5±16.92µs    10.7 MB/sec
formatter/numpy/globals.py                 1.05    163.7±4.60µs    18.0 MB/sec    1.00    156.2±5.47µs    18.9 MB/sec
formatter/pydantic/types.py                1.11      3.6±0.04ms     7.2 MB/sec    1.00      3.2±0.06ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.16ms     2.5 MB/sec    1.04     17.2±0.37ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.04      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.4±7.52µs     5.9 MB/sec    1.01   499.4±10.19µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.06      7.5±0.29ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.12ms     4.8 MB/sec    1.00      8.4±0.12ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.5±30.44µs     9.4 MB/sec    1.01  1789.4±30.03µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±4.15µs    14.8 MB/sec    1.01    202.5±4.50µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-06-11 02:17_

---

_Review comment by @charliermarsh on `fuzz/fuzz_targets/ruff_parse_simple.rs`:34 on 2023-06-11 02:17_

Do you mind elaborating a bit on what problem this is solving? Just trying to catch up on the relevant issue. It looks like we're detecting here whether the lexer is emitting tokens.

---

_@addisoncrump reviewed on 2023-06-11 02:38_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_parse_simple.rs`:34 on 2023-06-11 02:38_

Yes, and checking if the indices specified by those tokens are aligned on character boundaries. This was not the case for #5004 and led to panics in some cases.

---

_@addisoncrump reviewed on 2023-06-11 02:43_

---

_Review comment by @addisoncrump on `fuzz/fuzz_targets/ruff_parse_simple.rs`:34 on 2023-06-11 02:43_

As for why you'd do this: you can use this as a bit of an indirect way to detect miscomputed locations, as the fuzzer has the potential to inject the multi-byte UTF-8 chars at the miscomputed indices.

---

_Merged by @charliermarsh on 2023-06-12 16:10_

---

_Closed by @charliermarsh on 2023-06-12 16:10_

---
