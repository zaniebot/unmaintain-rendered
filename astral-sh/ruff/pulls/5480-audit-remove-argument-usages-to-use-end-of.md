```yaml
number: 5480
title: "Audit `remove_argument` usages to use end-of-function"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/loc
created_at: 2023-07-03T15:23:08Z
updated_at: 2023-07-03T16:21:03Z
url: https://github.com/astral-sh/ruff/pull/5480
synced_at: 2026-01-12T03:36:55Z
```

# Audit `remove_argument` usages to use end-of-function

---

_Pull request opened by @charliermarsh on 2023-07-03 15:23_

## Summary

This PR applies the fix in #5478 to a variety of other call-sites, and fixes some other range hygienic stuff in the rules that were modified.


---

_Label `internal` added by @charliermarsh on 2023-07-03 15:23_

---

_Comment by @github-actions[bot] on 2023-07-03 15:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+29, -29, 0 error(s))

<details><summary>zulip (+29, -29)</summary>
<p>

```diff
- confirmation/models.py:268:1: DJ008 Model does not define `__str__` method
+ confirmation/models.py:268:7: DJ008 Model does not define `__str__` method
- corporate/models.py:132:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:132:7: DJ008 Model does not define `__str__` method
- corporate/models.py:182:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:182:7: DJ008 Model does not define `__str__` method
- corporate/models.py:307:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:307:7: DJ008 Model does not define `__str__` method
- corporate/models.py:341:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:341:7: DJ008 Model does not define `__str__` method
- corporate/models.py:47:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:47:7: DJ008 Model does not define `__str__` method
- corporate/models.py:86:1: DJ008 Model does not define `__str__` method
+ corporate/models.py:86:7: DJ008 Model does not define `__str__` method
- zerver/models.py:1092:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:1092:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2242:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2242:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2312:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2312:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2320:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2320:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2343:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2343:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2379:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2379:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2466:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2466:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2479:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2479:7: DJ008 Model does not define `__str__` method
- zerver/models.py:2492:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:2492:7: DJ008 Model does not define `__str__` method
- zerver/models.py:277:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:277:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4056:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4056:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4121:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4121:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4144:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4144:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4157:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4157:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4222:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4222:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4230:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4230:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4314:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4314:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4684:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4684:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4871:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4871:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4909:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4909:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4918:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4918:7: DJ008 Model does not define `__str__` method
- zerver/models.py:4951:1: DJ008 Model does not define `__str__` method
+ zerver/models.py:4951:7: DJ008 Model does not define `__str__` method
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| DJ008 | 58 | 29 | 29 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.9±0.40ms     4.1 MB/sec    1.00      9.7±0.45ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.08ms     7.7 MB/sec    1.01      2.2±0.09ms     7.7 MB/sec
formatter/numpy/globals.py                 1.04   247.7±15.30µs    11.9 MB/sec    1.00   238.5±11.80µs    12.4 MB/sec
formatter/pydantic/types.py                1.02      4.7±0.14ms     5.5 MB/sec    1.00      4.6±0.21ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.08     18.6±0.73ms     2.2 MB/sec    1.00     17.2±0.72ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.19ms     3.9 MB/sec    1.00      4.2±0.13ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   572.4±20.84µs     5.2 MB/sec    1.00   573.0±45.44µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.26ms     3.4 MB/sec    1.02      7.7±0.29ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.25ms     4.7 MB/sec    1.02      8.8±0.25ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1928.9±64.65µs     8.6 MB/sec    1.00  1911.8±72.11µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   217.7±10.47µs    13.6 MB/sec    1.00   214.9±11.26µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.19ms     6.3 MB/sec    1.00      4.0±0.13ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.3±0.45ms     3.3 MB/sec    1.03     12.6±0.58ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.17ms     6.2 MB/sec    1.00      2.6±0.13ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   299.8±28.37µs     9.8 MB/sec    1.01   302.0±25.48µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.34ms     4.3 MB/sec    1.01      6.0±0.40ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.00     21.1±0.74ms  1976.2 KB/sec    1.01     21.2±0.74ms  1962.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.6±0.21ms     3.0 MB/sec    1.00      5.5±0.23ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   662.0±37.77µs     4.5 MB/sec    1.00   663.5±42.78µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.4±0.36ms     2.7 MB/sec    1.00      9.4±0.34ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.35ms     3.7 MB/sec    1.00     11.0±0.41ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.12ms     7.3 MB/sec    1.02      2.3±0.14ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.1±13.22µs    11.1 MB/sec    1.01   269.4±19.58µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.22ms     5.3 MB/sec    1.01      4.9±0.17ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-03 16:21_

---

_Closed by @charliermarsh on 2023-07-03 16:21_

---

_Branch deleted on 2023-07-03 16:21_

---
