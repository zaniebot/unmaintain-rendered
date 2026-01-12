```yaml
number: 3615
title: Add pre-commit in CI
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: add-pre-commit-in-ci
created_at: 2023-03-19T20:47:41Z
updated_at: 2023-03-19T21:09:54Z
url: https://github.com/astral-sh/ruff/pull/3615
synced_at: 2026-01-12T04:39:45Z
```

# Add pre-commit in CI

---

_Pull request opened by @JonathanPlasse on 2023-03-19 20:47_

- Close #3612

---

_@JonathanPlasse reviewed on 2023-03-19 20:54_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:31 on 2023-03-19 20:54_

If I do not use `language: system`, when I run pre-commit locally, then I run `cargo test` for example everything recompile. It is like there is a Rust cache conflict.

---

_Closed by @JonathanPlasse on 2023-03-19 20:55_

---

_Branch deleted on 2023-03-19 20:55_

---

_Comment by @github-actions[bot] on 2023-03-19 20:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.4±0.02ms     3.0 MB/sec    1.00     13.3±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.7±1.25µs     6.0 MB/sec    1.00    490.0±0.98µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      7.2±0.02ms     5.6 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1637.1±1.60µs    10.2 MB/sec    1.00   1616.8±2.36µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.6±1.90µs    16.3 MB/sec    1.00    181.0±0.64µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.00ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     10.8±0.11ms     3.8 MB/sec    1.00     10.7±0.07ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.0±0.02ms     5.6 MB/sec    1.00      3.0±0.02ms     5.6 MB/sec
linter/all-rules/numpy/globals.py          1.02    333.9±6.23µs     8.8 MB/sec    1.00   328.4±10.45µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      5.0±0.79ms     5.1 MB/sec    1.00      4.8±0.04ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.04ms     6.9 MB/sec    1.00      5.9±0.02ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1262.3±8.94µs    13.2 MB/sec    1.01  1271.0±10.23µs    13.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    131.5±1.21µs    22.4 MB/sec    1.00    130.6±1.48µs    22.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.3 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
