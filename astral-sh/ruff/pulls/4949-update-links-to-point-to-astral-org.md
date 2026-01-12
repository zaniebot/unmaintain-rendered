```yaml
number: 4949
title: Update links to point to Astral org
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-links
created_at: 2023-06-08T04:00:50Z
updated_at: 2023-06-08T15:43:45Z
url: https://github.com/astral-sh/ruff/pull/4949
synced_at: 2026-01-12T03:43:29Z
```

# Update links to point to Astral org

---

_Pull request opened by @dhruvmanila on 2023-06-08 04:00_

## Summary

Update links from `https://github.com/charliermarsh/ruff` to `https://github.com/astral-sh/ruff`

\cc @charliermarsh 

## Test Plan

Open the links? xD


---

_@dhruvmanila reviewed on 2023-06-08 04:03_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/Cargo.toml`:7 on 2023-06-08 04:03_

Should we use https://beta.ruff.rs/docs/configuration/#command-line-interface here?

---

_Comment by @github-actions[bot] on 2023-06-08 04:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.11ms     6.5 MB/sec    1.02      6.4±0.20ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.05  1263.3±17.63µs    13.2 MB/sec    1.00  1202.7±32.60µs    13.8 MB/sec
formatter/numpy/globals.py                 1.05    144.0±2.40µs    20.5 MB/sec    1.00    136.7±5.17µs    21.6 MB/sec
formatter/pydantic/types.py                1.05      2.8±0.05ms     9.1 MB/sec    1.00      2.7±0.07ms     9.6 MB/sec
linter/all-rules/large/dataset.py          1.09     16.9±0.21ms     2.4 MB/sec    1.00     15.5±0.25ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.0±0.05ms     4.2 MB/sec    1.00      3.7±0.05ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.04    482.7±9.38µs     6.1 MB/sec    1.00   465.2±12.33µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.7±0.10ms     3.8 MB/sec    1.00      6.5±0.11ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.03      7.7±0.14ms     5.3 MB/sec    1.00      7.5±0.21ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1666.3±35.15µs    10.0 MB/sec    1.00  1571.2±41.24µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.05    183.5±4.39µs    16.1 MB/sec    1.00    174.3±5.99µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.5±0.06ms     7.4 MB/sec    1.00      3.2±0.06ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.43ms     4.8 MB/sec    1.16      9.8±0.34ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1625.6±85.10µs    10.2 MB/sec    1.15  1863.9±120.18µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    175.6±7.58µs    16.8 MB/sec    1.12   197.4±15.43µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.13ms     7.1 MB/sec    1.17      4.2±0.19ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     20.6±0.50ms  2024.6 KB/sec    1.00     20.7±0.62ms  2016.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.22ms     3.2 MB/sec    1.00      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   615.6±28.15µs     4.8 MB/sec    1.02   628.9±39.37µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.32ms     2.9 MB/sec    1.01      8.9±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.32ms     4.0 MB/sec    1.02     10.3±0.33ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.7 MB/sec    1.02      2.2±0.17ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   250.7±10.95µs    11.8 MB/sec    1.01   252.6±12.64µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.19ms     5.6 MB/sec    1.04      4.7±0.22ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-08 05:49_

---

_@konstin reviewed on 2023-06-08 06:10_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:7 on 2023-06-08 06:10_

I'd link to the top level docs in all crates, mainly since e.g. the cli crate is the entrypoint for maturin and iirc uses those links for pypi

---

_@konstin approved on 2023-06-08 06:11_

---

_@dhruvmanila reviewed on 2023-06-08 13:23_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/Cargo.toml`:7 on 2023-06-08 13:23_

PyPi uses the links from `pyproject.toml`. I think this should be used by https://docs.rs if the crate was published.

---

_Merged by @charliermarsh on 2023-06-08 15:43_

---

_Closed by @charliermarsh on 2023-06-08 15:43_

---

_Label `documentation` added by @charliermarsh on 2023-06-08 15:43_

---
