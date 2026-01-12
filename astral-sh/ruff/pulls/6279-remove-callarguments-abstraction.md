```yaml
number: 6279
title: "Remove `CallArguments` abstraction"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/call-args
created_at: 2023-08-02T17:07:34Z
updated_at: 2023-08-02T17:51:07Z
url: https://github.com/astral-sh/ruff/pull/6279
synced_at: 2026-01-12T15:55:21Z
```

# Remove `CallArguments` abstraction

---

_@charliermarsh_

## Summary

This PR removes a now-unnecessary abstraction from `helper.rs` (`CallArguments`), in favor of adding methods to `Arguments` directly, which helps with discoverability.



---

_Label `internal` added by @charliermarsh on 2023-08-02 17:08_

---

_@zanieb approved on 2023-08-02 17:16_

---

_Comment by @github-actions[bot] on 2023-08-02 17:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.27ms     3.7 MB/sec    1.03     11.2±0.38ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.07ms     8.1 MB/sec    1.05      2.2±0.11ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    234.0±8.11µs    12.6 MB/sec    1.02   238.4±13.36µs    12.4 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.14ms     5.7 MB/sec    1.07      4.8±0.26ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.64ms     2.6 MB/sec    1.00     15.6±0.52ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.29ms     4.1 MB/sec    1.00      3.9±0.11ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.2±14.74µs     5.8 MB/sec    1.03   522.2±26.07µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.27ms     3.8 MB/sec    1.04      7.0±0.28ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.32ms     5.1 MB/sec    1.03      8.2±0.25ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1632.5±93.04µs    10.2 MB/sec    1.02  1667.6±55.43µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    187.5±9.09µs    15.7 MB/sec    1.00    184.4±6.92µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.10ms     7.3 MB/sec    1.04      3.6±0.20ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.26ms     4.0 MB/sec    1.01     10.3±0.17ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1973.6±47.64µs     8.4 MB/sec    1.00  1963.3±39.43µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    216.9±6.41µs    13.6 MB/sec    1.01   219.5±20.49µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.08ms     5.9 MB/sec    1.01      4.4±0.12ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.18ms     2.9 MB/sec    1.02     14.3±0.34ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.06ms     4.6 MB/sec    1.02      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   446.4±12.37µs     6.6 MB/sec    1.00   447.4±15.85µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.08ms     4.1 MB/sec    1.01      6.3±0.15ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.08ms     5.5 MB/sec    1.03      7.6±0.14ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1535.5±27.35µs    10.8 MB/sec    1.01  1555.5±35.05µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.4±4.94µs    17.0 MB/sec    1.00    174.3±6.18µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.7 MB/sec    1.01      3.3±0.07ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-02 17:25_

---

_Closed by @charliermarsh on 2023-08-02 17:25_

---

_Branch deleted on 2023-08-02 17:25_

---

_Review comment by @konstin on `crates/ruff_python_ast/src/nodes.rs`:2146 on 2023-08-02 17:42_

This function confused me at first because it's reverse from what i expected: It doesn't tell you to which signature parameter a specific user provided argument is matched to, but which user provided parameter gets sent to an argument (if i got the naming the right way round, i still have to read up on that)

---

_@konstin approved on 2023-08-02 17:42_

---
