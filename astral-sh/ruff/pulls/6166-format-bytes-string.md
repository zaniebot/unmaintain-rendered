```yaml
number: 6166
title: Format bytes string
type: pull_request
state: merged
author: lkh42t
labels: []
assignees: []
merged: true
base: main
head: format-bytes
created_at: 2023-07-29T07:07:03Z
updated_at: 2023-07-31T08:48:40Z
url: https://github.com/astral-sh/ruff/pull/6166
synced_at: 2026-01-12T02:58:30Z
```

# Format bytes string

---

_Pull request opened by @lkh42t on 2023-07-29 07:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Format bytes string

Closes #6064

## Test Plan

Added a fixture based on string's one

---

_Comment by @github-actions[bot] on 2023-07-29 07:38_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.6±0.19ms     4.7 MB/sec    1.00      8.5±0.12ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1656.8±21.32µs    10.1 MB/sec    1.01  1674.6±29.60µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    179.6±1.77µs    16.4 MB/sec    1.03    184.6±0.34µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.03ms     7.2 MB/sec    1.01      3.6±0.08ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.03ms     3.7 MB/sec    1.00     11.2±0.05ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    385.4±0.65µs     7.7 MB/sec    1.00    385.2±0.55µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.02ms     5.1 MB/sec    1.00      5.0±0.11ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.03ms     6.9 MB/sec    1.01      6.0±0.03ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1247.3±8.86µs    13.4 MB/sec    1.00   1250.3±5.63µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.3±1.22µs    21.5 MB/sec    1.01    138.3±0.70µs    21.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.01      2.6±0.03ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.16ms     4.0 MB/sec    1.00     10.2±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1969.6±26.67µs     8.5 MB/sec    1.00  1972.5±22.45µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    216.8±6.12µs    13.6 MB/sec    1.02    220.0±8.65µs    13.4 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.13ms     5.9 MB/sec    1.00      4.3±0.05ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.11ms     3.0 MB/sec    1.00     13.5±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.00      3.6±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.6±5.80µs     6.8 MB/sec    1.00    433.6±7.08µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.10ms     4.1 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.06ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1534.5±30.48µs    10.9 MB/sec    1.00  1513.9±12.86µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.6±3.49µs    17.4 MB/sec    1.00    167.5±2.75µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.05ms     7.7 MB/sec    1.00      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_constant.rs`:59 on 2023-07-31 08:13_

@konstin I'm a bit surprised that black also normalizes byte strings because I assumed we would need to treat them as opaque bytes. Are you aware of any normalization that isn't safe for byte strings?

---

_@MichaReiser approved on 2023-07-31 08:15_

Nice! Thank you

---

_Review requested from @konstin by @MichaReiser on 2023-07-31 08:15_

---

_@konstin reviewed on 2023-07-31 08:43_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_constant.rs`:59 on 2023-07-31 08:43_

as far as i know and reading https://docs.python.org/3/library/stdtypes.html#bytes, byte strings are pretty much like normal strings except that they are limited to ascii input, allow arbitrary bytes and `\x` behaves differently

---

_Comment by @konstin on 2023-07-31 08:45_

Found a bug in raw string formatting bug while reviewing, but it's a preexisting general string formatting bug and not your fault (https://github.com/astral-sh/ruff/issues/6186). Fixing #6186 however should add tests both for regular strings and for byte strings.

---

_@konstin approved on 2023-07-31 08:46_

Thanks!

---

_Merged by @konstin on 2023-07-31 08:46_

---

_Closed by @konstin on 2023-07-31 08:46_

---

_Branch deleted on 2023-07-31 08:48_

---
