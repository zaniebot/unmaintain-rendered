```yaml
number: 5768
title: "Move `Literal` flag detection into recurse phase"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/literal
created_at: 2023-07-15T01:57:18Z
updated_at: 2023-07-15T02:32:34Z
url: https://github.com/astral-sh/ruff/pull/5768
synced_at: 2026-01-12T03:30:21Z
```

# Move `Literal` flag detection into recurse phase

---

_Pull request opened by @charliermarsh on 2023-07-15 01:57_

## Summary

The AST pass is broken up into three phases: pre-visit (which includes analysis), recurse (visit all members), and post-visit (clean-up). We're not supposed to edit semantic model flags in the pre-visit phase, but it looks like we were for literal detection. This didn't matter in practice, but I'm looking into some AST refactors for which this _does_ cause issues.

No behavior changes expected.

## Test Plan

Good test coverage on these.


---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/typing.rs`:278 on 2023-07-15 01:58_

These are obviously sort of dumb but they exist for consistency with the other variants (e.g., `is_pep_593_generic_member`).

---

_@charliermarsh reviewed on 2023-07-15 01:58_

---

_Label `internal` added by @charliermarsh on 2023-07-15 01:58_

---

_Merged by @charliermarsh on 2023-07-15 02:04_

---

_Closed by @charliermarsh on 2023-07-15 02:04_

---

_Branch deleted on 2023-07-15 02:04_

---

_Comment by @github-actions[bot] on 2023-07-15 02:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.02ms     4.1 MB/sec    1.00      9.9±0.02ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1894.3±2.15µs     8.8 MB/sec    1.00   1898.9±2.23µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.5±0.37µs    14.4 MB/sec    1.01    206.6±0.38µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.01     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.7±1.43µs     7.7 MB/sec    1.00    385.7±1.85µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1475.7±2.85µs    11.3 MB/sec    1.01   1484.4±7.67µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.2±0.22µs    18.5 MB/sec    1.00    158.8±0.22µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.27ms     3.6 MB/sec    1.00     11.2±0.18ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.01   251.2±10.06µs    11.7 MB/sec    1.00    249.7±6.99µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.08ms     5.3 MB/sec    1.00      4.8±0.07ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.08     17.6±0.27ms     2.3 MB/sec    1.00     16.3±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.08      4.5±0.13ms     3.7 MB/sec    1.00      4.2±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.06   534.5±15.53µs     5.5 MB/sec    1.00    502.1±6.16µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.10      7.9±0.19ms     3.2 MB/sec    1.00      7.2±0.15ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.04      8.7±0.09ms     4.7 MB/sec    1.00      8.3±0.17ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1809.0±22.53µs     9.2 MB/sec    1.00  1790.2±22.98µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.5±6.96µs    14.4 MB/sec    1.01    206.9±5.33µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.8±0.09ms     6.7 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
