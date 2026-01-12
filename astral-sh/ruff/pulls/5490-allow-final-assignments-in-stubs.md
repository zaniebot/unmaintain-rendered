```yaml
number: 5490
title: "Allow `Final` assignments in stubs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/final
created_at: 2023-07-03T17:50:05Z
updated_at: 2023-07-03T18:40:33Z
url: https://github.com/astral-sh/ruff/pull/5490
synced_at: 2026-01-12T15:55:18Z
```

# Allow `Final` assignments in stubs

---

_@charliermarsh_

## Summary

This fixes one incompatibility with `flake8-pyi`, and gives us a clean pass on `typeshed`.


---

_Label `bug` added by @charliermarsh on 2023-07-03 17:50_

---

_Renamed from "Allow Final assignments in stubs" to "Allow `Final` assignments in stubs" by @charliermarsh on 2023-07-03 17:50_

---

_Merged by @charliermarsh on 2023-07-03 17:57_

---

_Closed by @charliermarsh on 2023-07-03 17:57_

---

_Branch deleted on 2023-07-03 17:57_

---

_Comment by @github-actions[bot] on 2023-07-03 18:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -16, 0 error(s))

<details><summary>typeshed (+0, -16)</summary>
<p>

```diff
- stubs/netaddr/netaddr/strategy/ipv6.pyi:11:17: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/X.pyi:145:30: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:1005:45: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:1012:50: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:114:32: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:137:29: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:490:42: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:494:31: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:495:37: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:496:36: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:87:28: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:920:33: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:932:52: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:949:54: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:979:40: PYI015 [*] Only simple default values allowed for assignments
- stubs/python-xlib/Xlib/ext/nvcontrol.pyi:983:50: PYI015 [*] Only simple default values allowed for assignments
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI015 | 16 | 0 | 16 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1768.3±2.63µs     9.4 MB/sec    1.00   1768.3±3.31µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    192.2±0.36µs    15.4 MB/sec    1.01    193.4±0.89µs    15.3 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.5 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.02ms     2.9 MB/sec    1.01     14.0±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    370.6±8.56µs     8.0 MB/sec    1.03   380.1±12.62µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1480.0±3.29µs    11.3 MB/sec    1.00   1482.2±2.77µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.03    160.9±0.20µs    18.3 MB/sec    1.00    156.6±0.32µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.11     10.9±0.63ms     3.7 MB/sec     1.00      9.8±0.89ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.3±0.13ms     7.2 MB/sec     1.00      2.1±0.13ms     7.9 MB/sec
formatter/numpy/globals.py                 1.08   260.6±24.95µs    11.3 MB/sec     1.00   241.5±22.14µs    12.2 MB/sec
formatter/pydantic/types.py                1.06      5.0±0.18ms     5.1 MB/sec     1.00      4.7±0.47ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     17.3±1.53ms     2.3 MB/sec     1.00     17.0±1.48ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.47ms     3.8 MB/sec     1.00      4.4±0.35ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   520.9±22.69µs     5.7 MB/sec     1.05   548.4±49.76µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.5±0.58ms     3.4 MB/sec     1.00      7.2±0.38ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.51ms     4.7 MB/sec     1.00      8.5±0.59ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1834.7±123.70µs     9.1 MB/sec    1.00  1780.9±57.87µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   207.8±10.69µs    14.2 MB/sec     1.06   219.4±18.97µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.21ms     6.7 MB/sec     1.01      3.8±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-07-03 18:40_

> and gives us a clean pass on typeshed.

that's cool!

---
