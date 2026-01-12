```yaml
number: 3583
title: Enable ANSI colors on Windows 10
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/virtual
created_at: 2023-03-17T18:24:21Z
updated_at: 2023-03-17T21:34:41Z
url: https://github.com/astral-sh/ruff/pull/3583
synced_at: 2026-01-12T04:39:45Z
```

# Enable ANSI colors on Windows 10

---

_Pull request opened by @charliermarsh on 2023-03-17 18:24_

## Summary

It looks like our color output is failing on Windows 10 (yet we're still outputting the color markers). There's some discussion in https://github.com/mackwic/colored/issues/59, and the recommended fix is to enable the virtual terminal in that case (https://docs.rs/colored/2.0.0/x86_64-pc-windows-msvc/colored/control/fn.set_virtual_terminal.html).

## Test Plan

See: #3568, where @copperfield42 tested locally before and after.


---

_Comment by @github-actions[bot] on 2023-03-17 18:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.44ms     2.8 MB/sec    1.02     14.8±0.52ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.13ms     4.3 MB/sec    1.00      3.9±0.09ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   534.2±20.61µs     5.5 MB/sec    1.00   532.9±22.12µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.25ms     4.0 MB/sec    1.00      6.4±0.21ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.20ms     5.1 MB/sec    1.00      7.9±0.21ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1791.2±67.61µs     9.3 MB/sec    1.00  1786.9±63.66µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±9.43µs    14.4 MB/sec    1.02    208.1±8.80µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.13ms     6.8 MB/sec    1.02      3.8±0.13ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.8±0.13ms     2.6 MB/sec    1.00     15.6±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    463.7±4.62µs     6.4 MB/sec    1.01    467.8±8.73µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.08ms     3.7 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.09ms     4.6 MB/sec    1.00      8.7±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1879.9±9.79µs     8.9 MB/sec    1.00  1858.0±25.31µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    193.0±1.50µs    15.3 MB/sec    1.00    190.6±1.10µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.0±0.02ms     6.3 MB/sec    1.00      4.0±0.06ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-17 20:22_

---

_Marked ready for review by @charliermarsh on 2023-03-17 20:23_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:73 on 2023-03-17 20:24_

This is what I see everyone doing on GitHub Code Search, not sure if we can scope this any more narrowly.

---

_@charliermarsh reviewed on 2023-03-17 20:24_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:74 on 2023-03-17 20:24_

According to the docs this always returns `Ok`, and the signature is just a backwards-compatibility thing. Should we `.unwrap()`?

---

_@charliermarsh reviewed on 2023-03-17 20:24_

---

_@JonathanPlasse reviewed on 2023-03-17 20:37_

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/lib.rs`:74 on 2023-03-17 20:37_

I think yes, if it returns something other than `Ok` that is an error which would be silenced otherwise.

---

_@MichaReiser reviewed on 2023-03-17 20:49_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:74 on 2023-03-17 20:49_

I would assume that the LTO removes the error case.

I would prefer an assert over unwrap since we're not interested in the value

assert!(...is_ok())

---

_@MichaReiser approved on 2023-03-17 20:49_

---

_Merged by @charliermarsh on 2023-03-17 21:34_

---

_Closed by @charliermarsh on 2023-03-17 21:34_

---

_Branch deleted on 2023-03-17 21:34_

---
