```yaml
number: 6105
title: call-datetime-without-tzinfo comment
type: pull_request
state: merged
author: arembridge
labels:
  - documentation
assignees: []
merged: true
base: main
head: call-datetime-without-tzinfo
created_at: 2023-07-26T20:23:27Z
updated_at: 2023-07-26T23:40:04Z
url: https://github.com/astral-sh/ruff/pull/6105
synced_at: 2026-01-12T03:30:22Z
```

# call-datetime-without-tzinfo comment

---

_Pull request opened by @arembridge on 2023-07-26 20:23_

## Summary

Updated doc comment for `call_datetime_without_tzinfo.rs`. Online docs also benefit from this update.

## Test Plan

Checked docs via [mkdocs](https://github.com/astral-sh/ruff/blob/389fe13c934fe73679474006412b1eded4a2cad0/CONTRIBUTING.md?plain=1#L267-L296)


---

_Comment by @github-actions[bot] on 2023-07-26 20:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.13ms     4.1 MB/sec    1.00      9.8±0.12ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1944.2±30.25µs     8.6 MB/sec    1.02  1982.3±21.73µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    214.1±4.80µs    13.8 MB/sec    1.03    221.2±5.46µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.08ms     6.1 MB/sec    1.01      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.20ms     2.9 MB/sec    1.00     13.9±0.23ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.02      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.1±8.06µs     6.4 MB/sec    1.02    466.6±3.99µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.07ms     4.1 MB/sec    1.01      6.4±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.01      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1439.5±13.15µs    11.6 MB/sec    1.01  1457.8±11.46µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.8±2.58µs    18.7 MB/sec    1.00    157.8±2.38µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.03ms     8.4 MB/sec    1.00      3.0±0.05ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.0±0.48ms     3.4 MB/sec    1.02     12.2±0.40ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.13ms     7.1 MB/sec    1.00      2.3±0.12ms     7.2 MB/sec
formatter/numpy/globals.py                 1.01   262.9±21.54µs    11.2 MB/sec    1.00   261.1±14.79µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.24ms     5.0 MB/sec    1.02      5.2±0.21ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.03     17.9±0.64ms     2.3 MB/sec    1.00     17.3±0.72ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.6±0.18ms     3.6 MB/sec    1.00      4.5±0.23ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.04   543.1±22.99µs     5.4 MB/sec    1.00   522.2±32.95µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.26ms     3.3 MB/sec    1.00      7.7±0.35ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.38ms     4.6 MB/sec    1.00      8.8±0.30ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1782.4±85.02µs     9.3 MB/sec    1.02  1809.1±83.56µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   199.9±14.11µs    14.8 MB/sec    1.00   196.9±10.41µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.16ms     6.6 MB/sec    1.00      3.8±0.12ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-26 23:12_

---

_Merged by @charliermarsh on 2023-07-26 23:21_

---

_Closed by @charliermarsh on 2023-07-26 23:21_

---
