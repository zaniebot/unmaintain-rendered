```yaml
number: 4625
title: "Add Checker::enabled shortcut"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: checker_enabled
created_at: 2023-05-24T11:46:40Z
updated_at: 2023-05-24T14:06:08Z
url: https://github.com/astral-sh/ruff/pull/4625
synced_at: 2026-01-12T15:55:16Z
```

# Add Checker::enabled shortcut

---

_@konstin_

## Summary

This is a refactoring that shortens a bunch of code by replacing `checker.settings.rules.enabled` with `checker.enabled`. github says +566/-1,273. 

## Test Plan

`cargo clippy` (no functional changes)


---

_Comment by @github-actions[bot] on 2023-05-24 11:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.09     18.5±0.82ms     2.2 MB/sec     1.00     16.9±0.72ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.3±0.20ms     3.8 MB/sec     1.00      4.1±0.20ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.07   547.2±20.06µs     5.4 MB/sec     1.00   513.6±28.52µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.6±0.34ms     3.4 MB/sec     1.00      7.0±0.31ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.04      8.1±0.43ms     5.0 MB/sec     1.00      7.8±0.31ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1785.9±101.18µs     9.3 MB/sec    1.00  1736.5±76.55µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.6±8.78µs    14.2 MB/sec     1.07   221.4±25.37µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.13ms     7.0 MB/sec     1.00      3.6±0.25ms     7.1 MB/sec
parser/large/dataset.py                    1.03      6.4±0.20ms     6.4 MB/sec     1.00      6.2±0.29ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.05  1273.6±32.80µs    13.1 MB/sec     1.00  1210.8±48.27µs    13.8 MB/sec
parser/numpy/globals.py                    1.02    128.8±5.69µs    22.9 MB/sec     1.00    126.2±6.80µs    23.4 MB/sec
parser/pydantic/types.py                   1.04      2.8±0.11ms     9.2 MB/sec     1.00      2.7±0.12ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     22.7±0.39ms  1837.2 KB/sec    1.00     22.5±0.46ms  1847.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.17ms     3.0 MB/sec    1.03      5.8±0.45ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   652.7±20.41µs     4.5 MB/sec    1.03   669.4±26.57µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.29ms     2.8 MB/sec    1.04      9.6±0.19ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.30ms     3.8 MB/sec    1.03     11.2±0.27ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.06ms     7.3 MB/sec    1.01      2.3±0.07ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   278.3±13.00µs    10.6 MB/sec    1.00    276.4±9.92µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.10ms     5.2 MB/sec    1.01      5.0±0.10ms     5.1 MB/sec
parser/large/dataset.py                    1.18     10.4±0.17ms     3.9 MB/sec    1.00      8.9±0.17ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.14  1960.1±37.81µs     8.5 MB/sec    1.00  1714.5±33.16µs     9.7 MB/sec
parser/numpy/globals.py                    1.11    192.3±6.64µs    15.3 MB/sec    1.00    173.0±5.74µs    17.1 MB/sec
parser/pydantic/types.py                   1.15      4.4±0.09ms     5.8 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-24 12:24_

I love it

---

_@charliermarsh approved on 2023-05-24 12:55_

Horray!

---

_Merged by @konstin on 2023-05-24 12:56_

---

_Closed by @konstin on 2023-05-24 12:56_

---

_Branch deleted on 2023-05-24 12:56_

---

_Comment by @charliermarsh on 2023-05-24 14:06_

Lets do the same for `any_enabled`?

---
