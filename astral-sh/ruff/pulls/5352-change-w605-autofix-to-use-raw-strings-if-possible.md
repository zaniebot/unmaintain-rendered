```yaml
number: 5352
title: Change W605 autofix to use raw strings if possible
type: pull_request
state: merged
author: hauntsaninja
labels: []
assignees: []
merged: true
base: main
head: w605-raw-string
created_at: 2023-06-24T20:26:09Z
updated_at: 2023-06-25T21:38:49Z
url: https://github.com/astral-sh/ruff/pull/5352
synced_at: 2026-01-12T15:55:18Z
```

# Change W605 autofix to use raw strings if possible

---

_@hauntsaninja_

Fixes #5061

---

_Comment by @hauntsaninja on 2023-06-24 20:27_

I'm a little confused about how testing works. Why do both W605_0.py and W605_1.py exist? The line and column numbers in those comments don't seem to be checked. Also I'm assuming ruff magically deduplicates the autofix via #875 or whatever.

---

_Comment by @github-actions[bot] on 2023-06-24 20:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      7.2±0.30ms     5.7 MB/sec     1.00      7.2±0.22ms     5.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1712.6±155.69µs     9.7 MB/sec    1.01  1737.2±82.99µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    204.6±9.00µs    14.4 MB/sec     1.00    203.7±7.59µs    14.5 MB/sec
formatter/pydantic/types.py                1.02      3.8±0.16ms     6.8 MB/sec     1.00      3.7±0.12ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.7±0.27ms     2.8 MB/sec     1.00     14.7±0.24ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.09ms     4.5 MB/sec     1.00      3.7±0.13ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   487.6±21.69µs     6.1 MB/sec     1.00   478.5±18.43µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.25ms     3.9 MB/sec     1.00      6.4±0.18ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.21ms     5.6 MB/sec     1.00      7.3±0.34ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1604.7±68.80µs    10.4 MB/sec     1.00  1586.8±46.10µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±9.47µs    15.5 MB/sec     1.00    189.6±9.75µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.12ms     7.6 MB/sec     1.00      3.3±0.10ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.07ms     5.2 MB/sec    1.00      7.8±0.09ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1775.3±20.42µs     9.4 MB/sec    1.00  1779.0±30.45µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    216.9±7.17µs    13.6 MB/sec    1.01   219.4±16.62µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.05ms     6.4 MB/sec    1.00      4.0±0.06ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.2±0.18ms     2.7 MB/sec    1.00     15.1±0.18ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.00      4.0±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.7±6.26µs     6.0 MB/sec    1.01    491.9±5.52µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.06ms     3.8 MB/sec    1.01      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.05ms     5.2 MB/sec    1.01      7.9±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1681.5±18.17µs     9.9 MB/sec    1.02  1709.5±17.64µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±3.38µs    15.1 MB/sec    1.00    195.3±2.41µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.2 MB/sec    1.01      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-25 21:30_

> I'm a little confused about how testing works. Why do both W605_0.py and W605_1.py exist? The line and column numbers in those comments don't seem to be checked. Also I'm assuming ruff magically deduplicates the autofix via https://github.com/astral-sh/ruff/pull/875 or whatever.

I believe `W605_1.py` uses [CRLF line-endings](https://github.com/astral-sh/ruff/blob/e0a507e48ebad7bc400f76290535673efece9229/.gitattributes#L4) and so exists as a special-case to ensure that rule handles universal line endings.

The comments in the test files purely exist as documentation -- we only enforce correctness via the checked-in snapshots.


---

_Comment by @charliermarsh on 2023-06-25 21:35_

This looks good to me, thank you!

---

_Merged by @charliermarsh on 2023-06-25 21:35_

---

_Closed by @charliermarsh on 2023-06-25 21:35_

---

_Branch deleted on 2023-06-25 21:38_

---
