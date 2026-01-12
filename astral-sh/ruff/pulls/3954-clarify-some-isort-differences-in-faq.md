```yaml
number: 3954
title: Clarify some isort differences in FAQ
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/isort-docs
created_at: 2023-04-13T03:59:08Z
updated_at: 2023-04-13T04:25:29Z
url: https://github.com/astral-sh/ruff/pull/3954
synced_at: 2026-01-12T04:28:19Z
```

# Clarify some isort differences in FAQ

---

_Pull request opened by @charliermarsh on 2023-04-13 03:59_

Closes #3908.

---

_Label `documentation` added by @charliermarsh on 2023-04-13 03:59_

---

_Merged by @charliermarsh on 2023-04-13 04:05_

---

_Closed by @charliermarsh on 2023-04-13 04:05_

---

_Branch deleted on 2023-04-13 04:05_

---

_Comment by @github-actions[bot] on 2023-04-13 04:10_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -7, 0 error(s))

<details><summary>bokeh (+0, -7)</summary>
<p>

```diff
- tests/unit/bokeh/embed/test_server__embed.py:110:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:129:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:157:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:171:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:67:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:81:19: UP031 [*] Use format specifiers instead of percent format
- tests/unit/bokeh/embed/test_server__embed.py:96:19: UP031 [*] Use format specifiers instead of percent format
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.09ms     2.4 MB/sec    1.00     17.1±0.09ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    550.9±2.43µs     5.4 MB/sec    1.00    552.7±1.58µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.03ms     3.5 MB/sec    1.00      7.3±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.06ms     4.6 MB/sec    1.00      8.7±0.04ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1972.6±7.90µs     8.4 MB/sec    1.00  1958.5±10.55µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    216.7±1.75µs    13.6 MB/sec    1.00    215.6±2.77µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.03ms     6.3 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.26ms     2.5 MB/sec    1.01     16.5±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.10ms     4.0 MB/sec    1.00      4.2±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   502.0±15.33µs     5.9 MB/sec    1.00   499.0±10.83µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.00      6.9±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.22ms     4.9 MB/sec    1.00      8.3±0.15ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1810.8±33.37µs     9.2 MB/sec    1.01  1821.5±41.76µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.0±5.01µs    15.1 MB/sec    1.01    196.4±7.04µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.8 MB/sec    1.00      3.8±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
