```yaml
number: 5406
title: Make lib iteration platform-specific
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/libs
created_at: 2023-06-27T22:07:31Z
updated_at: 2023-06-28T14:09:12Z
url: https://github.com/astral-sh/ruff/pull/5406
synced_at: 2026-01-12T03:36:55Z
```

# Make lib iteration platform-specific

---

_Pull request opened by @charliermarsh on 2023-06-27 22:07_

_No description provided._

---

_Review requested from @konstin by @charliermarsh on 2023-06-27 22:07_

---

_Comment by @github-actions[bot] on 2023-06-27 22:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1744.5±3.74µs     9.5 MB/sec    1.00   1742.7±7.32µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    195.5±0.28µs    15.1 MB/sec    1.00    195.8±2.38µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     13.4±0.12ms     3.0 MB/sec    1.00     13.3±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.00      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.8±1.65µs     6.8 MB/sec    1.00    431.6±3.70µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.03ms     6.0 MB/sec    1.00      6.7±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1481.1±18.80µs    11.2 MB/sec    1.00   1484.4±6.04µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.1±0.23µs    17.6 MB/sec    1.00    166.9±0.22µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.00      3.1±0.03ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.41ms     4.0 MB/sec    1.10     11.2±0.64ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.7 MB/sec    1.06      2.3±0.09ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   259.5±12.31µs    11.4 MB/sec    1.01   263.2±23.03µs    11.2 MB/sec
formatter/pydantic/types.py                1.03      5.0±0.25ms     5.1 MB/sec    1.00      4.8±0.25ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     17.0±0.46ms     2.4 MB/sec    1.02     17.4±1.05ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.20ms     3.6 MB/sec    1.04      4.8±0.22ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   555.3±22.85µs     5.3 MB/sec    1.03   570.1±33.79µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.30ms     3.3 MB/sec    1.04      8.0±0.39ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      9.2±0.31ms     4.4 MB/sec    1.00      9.1±0.56ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1858.2±71.75µs     9.0 MB/sec    1.04  1941.8±78.39µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   230.1±10.18µs    12.8 MB/sec    1.03   236.6±12.84µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.10ms     6.5 MB/sec    1.04      4.1±0.25ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/python_platform.rs`:16 on 2023-06-28 05:27_

`lib64` is something that i could totally see some distro using but i've never seen in practice so it's hard to test

---

_@konstin approved on 2023-06-28 05:27_

fwiw this is what i did in monotrail: https://github.com/konstin/poc-monotrail/blob/c6425ef3baf2a2fd9e96474b9997ee989419b3de/install-wheel-rs/src/install_location.rs#L129-L144

---

_@MichaReiser approved on 2023-06-28 12:02_

---

_Merged by @charliermarsh on 2023-06-28 13:52_

---

_Closed by @charliermarsh on 2023-06-28 13:52_

---

_Branch deleted on 2023-06-28 13:52_

---
