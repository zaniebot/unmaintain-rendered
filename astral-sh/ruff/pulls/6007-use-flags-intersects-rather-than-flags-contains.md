```yaml
number: 6007
title: "Use `Flags::intersects` rather than `Flags::contains`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/flags-ii
created_at: 2023-07-23T02:47:43Z
updated_at: 2023-07-23T03:17:53Z
url: https://github.com/astral-sh/ruff/pull/6007
synced_at: 2026-01-12T15:55:20Z
```

# Use `Flags::intersects` rather than `Flags::contains`

---

_@charliermarsh_

## Summary

This is equivalent for a single flag, but I think it's more likely to be correct when the bitflags are modified -- the primary reason being that we sometimes define flags as the union of other flags, e.g.:

```rust
const ANNOTATION = Self::TYPING_ONLY_ANNOTATION.bits() | Self::RUNTIME_ANNOTATION.bits();
```

In this case, `flags.contains(Flag::ANNOTATION)` requires that _both_ flags in the union are set, whereas `flags.intersects(Flag::ANNOTATION)` requires that _at least one_ flag is set.


---

_Marked ready for review by @charliermarsh on 2023-07-23 02:47_

---

_Merged by @charliermarsh on 2023-07-23 02:59_

---

_Closed by @charliermarsh on 2023-07-23 02:59_

---

_Branch deleted on 2023-07-23 02:59_

---

_Comment by @github-actions[bot] on 2023-07-23 03:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.7±0.25ms     3.5 MB/sec    1.02     11.9±0.29ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.15ms     7.1 MB/sec    1.01      2.4±0.08ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   265.4±14.71µs    11.1 MB/sec    1.02   270.6±10.40µs    10.9 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.13ms     5.0 MB/sec    1.01      5.2±0.14ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.42ms     2.4 MB/sec    1.04     17.6±0.38ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.16ms     3.9 MB/sec    1.02      4.4±0.11ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   563.7±18.33µs     5.2 MB/sec    1.01   571.9±10.75µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.19ms     3.3 MB/sec    1.04      7.9±0.21ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.15ms     4.7 MB/sec    1.08      9.5±0.23ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1842.4±43.97µs     9.0 MB/sec    1.08  1983.1±50.69µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.4±6.59µs    13.6 MB/sec    1.05    226.6±5.70µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.12ms     6.5 MB/sec    1.08      4.2±0.11ms     6.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.09ms     4.2 MB/sec    1.01      9.7±0.12ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1896.3±26.76µs     8.8 MB/sec    1.01  1916.0±25.15µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    212.0±4.36µs    13.9 MB/sec    1.03    217.6±7.57µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.1 MB/sec    1.03      4.3±0.08ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.8±0.15ms     3.0 MB/sec    1.00     13.7±0.14ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.04ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.9±7.02µs     6.8 MB/sec    1.00    429.6±5.34µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.09ms     4.1 MB/sec    1.00      6.2±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.10ms     5.7 MB/sec    1.00      7.1±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1488.2±17.25µs    11.2 MB/sec    1.00  1478.3±19.05µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.2±2.90µs    17.5 MB/sec    1.00    168.2±4.22µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     8.0 MB/sec    1.01      3.2±0.05ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
