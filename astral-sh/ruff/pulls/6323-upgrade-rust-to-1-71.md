```yaml
number: 6323
title: Upgrade Rust to 1.71
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: deps/rust
created_at: 2023-08-04T00:24:34Z
updated_at: 2023-08-04T12:52:13Z
url: https://github.com/astral-sh/ruff/pull/6323
synced_at: 2026-01-12T15:55:21Z
```

# Upgrade Rust to 1.71

---

_@zanieb_

Addresses [CVE-2023-38497](https://blog.rust-lang.org/2023/08/03/cve-2023-38497.html) 

See also the [version release post](https://blog.rust-lang.org/2023/08/03/Rust-1.71.1.html)

---

_Comment by @github-actions[bot] on 2023-08-04 00:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      9.8±0.09ms     4.1 MB/sec    1.00      9.1±0.09ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.06  1937.0±20.80µs     8.6 MB/sec    1.00  1833.1±18.18µs     9.1 MB/sec
formatter/numpy/globals.py                 1.03    216.4±6.21µs    13.6 MB/sec    1.00    209.8±3.13µs    14.1 MB/sec
formatter/pydantic/types.py                1.07      4.1±0.05ms     6.2 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.03     12.9±0.11ms     3.2 MB/sec    1.00     12.6±0.08ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.3±0.03ms     5.0 MB/sec    1.00      3.2±0.02ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    450.7±5.61µs     6.5 MB/sec    1.00    445.6±3.61µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.03      5.9±0.05ms     4.3 MB/sec    1.00      5.7±0.05ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.10      6.7±0.06ms     6.0 MB/sec    1.00      6.1±0.05ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1400.3±17.65µs    11.9 MB/sec    1.00  1313.4±11.86µs    12.7 MB/sec
linter/default-rules/numpy/globals.py      1.05    154.8±1.81µs    19.1 MB/sec    1.00    147.0±1.26µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.0±0.04ms     8.6 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.2±0.44ms     3.6 MB/sec    1.00     10.8±0.35ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.11ms     7.5 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   235.8±13.45µs    12.5 MB/sec    1.07   253.1±23.76µs    11.7 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.20ms     5.4 MB/sec    1.00      4.6±0.20ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.42ms     2.7 MB/sec    1.00     15.2±0.40ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.13ms     4.1 MB/sec    1.00      4.1±0.14ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   515.7±41.70µs     5.7 MB/sec    1.00   497.1±20.83µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.22ms     3.6 MB/sec    1.00      7.1±0.28ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.09      8.4±0.27ms     4.8 MB/sec    1.00      7.7±0.21ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1694.2±56.61µs     9.8 MB/sec    1.00  1614.5±62.53µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.05   195.0±10.43µs    15.1 MB/sec    1.00    185.0±8.89µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.7±0.14ms     7.0 MB/sec    1.00      3.5±0.11ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-04 01:08_

---

_Closed by @charliermarsh on 2023-08-04 01:08_

---

_Branch deleted on 2023-08-04 01:08_

---

_Comment by @MichaReiser on 2023-08-04 05:51_

I was afraid of upgrading Rust again because I remember that I broke the release workflow last time. @charliermarsh do you remember what broke back then?

---

_Comment by @charliermarsh on 2023-08-04 12:51_

I think the issue was in conda-forge. Sometimes the update to conda-forge’s Rust is slower than our upgrade and release cycle. I did consider that but assumed it was worth it to fix a CVE.

---
