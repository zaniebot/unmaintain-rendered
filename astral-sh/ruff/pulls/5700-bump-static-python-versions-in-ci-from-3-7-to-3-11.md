```yaml
number: 5700
title: Bump static Python versions in CI from 3.7 to 3.11
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: ci/python-311
created_at: 2023-07-12T00:17:19Z
updated_at: 2023-07-12T14:23:23Z
url: https://github.com/astral-sh/ruff/pull/5700
synced_at: 2026-01-12T03:36:55Z
```

# Bump static Python versions in CI from 3.7 to 3.11

---

_Pull request opened by @zanieb on 2023-07-12 00:17_

Python 3.7 is EOL and we should use the latest stable version for builds.

Related to https://github.com/astral-sh/ruff-lsp/pull/189


---

_Comment by @github-actions[bot] on 2023-07-12 00:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.01ms     4.9 MB/sec    1.00      8.4±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1896.8±2.76µs     8.8 MB/sec    1.00   1896.3±1.83µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    203.6±0.20µs    14.5 MB/sec    1.01    205.0±0.32µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.01      4.2±0.01ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.01ms     2.9 MB/sec    1.02     14.2±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.5±1.72µs     8.1 MB/sec    1.00    365.3±4.02µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.01      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1445.5±2.68µs    11.5 MB/sec    1.02   1476.0±2.47µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.4±0.22µs    19.0 MB/sec    1.01    157.6±1.09µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.1 MB/sec    1.01      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     11.2±0.20ms     3.6 MB/sec    1.00      9.8±0.26ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.12      2.4±0.04ms     6.9 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.07    260.0±6.82µs    11.3 MB/sec    1.00    242.0±9.03µs    12.2 MB/sec
formatter/pydantic/types.py                1.11      5.3±0.12ms     4.8 MB/sec    1.00      4.8±0.16ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.33ms     2.4 MB/sec    1.09     18.4±0.27ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.12ms     3.8 MB/sec    1.06      4.7±0.14ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    511.5±9.94µs     5.8 MB/sec    1.05   536.5±11.27µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.16ms     3.4 MB/sec    1.06      8.0±0.16ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.15ms     4.8 MB/sec    1.20     10.1±0.15ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1779.7±30.38µs     9.4 MB/sec    1.14      2.0±0.02ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.3±4.82µs    14.7 MB/sec    1.12    225.2±7.42µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     6.8 MB/sec    1.16      4.3±0.07ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:19 on 2023-07-12 03:08_

I thought maybe we _had_ to use 3.7 for abi3 or something, but I don't really remember or know what I'm talking about. @konstin would know.

---

_@charliermarsh reviewed on 2023-07-12 03:08_

---

_@zanieb reviewed on 2023-07-12 03:51_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:19 on 2023-07-12 03:51_

hm

> PyO3 has feature flags abi3-py37, abi3-py38, abi3-py39 etc. to set the minimum required Python version for your abi3 wheel. For example, if you set the abi3-py37 feature, your extension wheel can be used on all Python 3 versions from Python 3.7 and up
>
> PyO3 is only able to link your extension module to abi3 version up to and including your host Python version. E.g., if you set abi3-py38 and try to compile the crate with a host of Python 3.7, the build will fail.

https://pyo3.rs/v0.19.1/building_and_distribution#py_limited_apiabi3

Looks safe to me?


---

_@zanieb reviewed on 2023-07-12 03:52_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:19 on 2023-07-12 03:52_

We should be able to test this easily enough locally as well

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:19 on 2023-07-12 04:00_

Nice, yeah, that seems fine. It also may not even matter since we're only shipping a binary executable, which IIUC doesn't need to adhere to the ABI anyway. But maybe worth letting @konstin confirm.

---

_@charliermarsh reviewed on 2023-07-12 04:00_

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:19 on 2023-07-12 07:06_

we're building a binary and not a library so this is safe :)

---

_@konstin reviewed on 2023-07-12 07:06_

---

_@konstin reviewed on 2023-07-12 07:06_

---

_Review comment by @konstin on `.github/workflows/flake8-to-ruff.yaml`:12 on 2023-07-12 07:06_

```suggestion
  PYTHON_VERSION: "3.11"
```

---

_@konstin reviewed on 2023-07-12 07:06_

---

_Review comment by @konstin on `.github/workflows/release.yaml`:23 on 2023-07-12 07:06_

```suggestion
  PYTHON_VERSION: "3.11"
```

---

_@konstin approved on 2023-07-12 07:06_

---

_Merged by @zanieb on 2023-07-12 13:56_

---

_Closed by @zanieb on 2023-07-12 13:56_

---
