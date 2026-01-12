```yaml
number: 4001
title: Respect typing-modules when evaluating no-return functions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/never
created_at: 2023-04-17T20:19:31Z
updated_at: 2023-04-17T20:47:52Z
url: https://github.com/astral-sh/ruff/pull/4001
synced_at: 2026-01-12T15:55:14Z
```

# Respect typing-modules when evaluating no-return functions

---

_@charliermarsh_

## Summary

Closes #4000.

## Test Plan

Ran `cargo run -p ruff_cli -- ../cibuildwheel/ --select RET503 -n` before and after (removing the `# noqa: RET503` in `cibuildwheel/__main__.py`).

---

_Merged by @charliermarsh on 2023-04-17 20:25_

---

_Closed by @charliermarsh on 2023-04-17 20:25_

---

_Branch deleted on 2023-04-17 20:25_

---

_Comment by @github-actions[bot] on 2023-04-17 20:30_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>cibuildwheel (+1, -0)</summary>
<p>

```diff
+ cibuildwheel/__main__.py:266:29: RUF100 [*] Unused `noqa` directive (unused: `RET503`)
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.08ms     2.9 MB/sec    1.00     14.3±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    460.6±2.46µs     6.4 MB/sec    1.00    462.1±1.68µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.01      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.01      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1625.5±1.46µs    10.2 MB/sec    1.00   1625.8±2.49µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.5±0.40µs    16.6 MB/sec    1.01    179.2±0.39µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1135.3±1.16µs    14.7 MB/sec    1.00   1137.4±3.81µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    113.4±0.47µs    26.0 MB/sec    1.00    113.5±0.20µs    26.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.18ms     2.5 MB/sec    1.00     16.5±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.7±8.47µs     5.9 MB/sec    1.01    502.8±7.60µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.01      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.01      8.3±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1795.4±18.88µs     9.3 MB/sec    1.01  1820.7±22.94µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.5±6.61µs    15.2 MB/sec    1.02    198.4±6.14µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.02      3.8±0.13ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.1 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1252.3±25.58µs    13.3 MB/sec    1.00  1249.4±22.12µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    124.0±2.37µs    23.8 MB/sec    1.00    123.7±1.98µs    23.9 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
