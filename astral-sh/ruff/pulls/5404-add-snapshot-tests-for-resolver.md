```yaml
number: 5404
title: Add snapshot tests for resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/airflow
created_at: 2023-06-27T21:39:45Z
updated_at: 2023-06-28T13:56:21Z
url: https://github.com/astral-sh/ruff/pull/5404
synced_at: 2026-01-12T03:36:55Z
```

# Add snapshot tests for resolver

---

_Pull request opened by @charliermarsh on 2023-06-27 21:39_

## Summary

This PR adds some snapshot tests for the resolver based on executing resolutions within a "mock" of the Airflow repo (that is: a folder that contains a subset of the repo's files, but all empty, and with an only-partially-complete virtual environment). It's intended to act as a lightweight integration test, to enable us to test resolutions on a "real" project without adding a dependency on Airflow itself.


---

_Label `internal` added by @charliermarsh on 2023-06-27 21:39_

---

_Comment by @github-actions[bot] on 2023-06-27 22:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.02ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1727.0±3.26µs     9.6 MB/sec    1.01   1743.9±3.74µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    193.9±0.37µs    15.2 MB/sec    1.02    196.8±1.26µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.00ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.03     13.5±0.06ms     3.0 MB/sec    1.00     13.1±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    433.1±0.83µs     6.8 MB/sec    1.00    428.8±0.31µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.0±0.02ms     4.3 MB/sec    1.00      5.8±0.05ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.06      7.0±0.01ms     5.8 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05   1534.3±4.61µs    10.9 MB/sec    1.00   1463.5±1.82µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.03    172.0±0.41µs    17.2 MB/sec    1.00    166.4±0.20µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.2±0.00ms     8.0 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     12.6±0.55ms     3.2 MB/sec    1.00     12.1±0.56ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.15ms     6.3 MB/sec    1.00      2.6±0.14ms     6.3 MB/sec
formatter/numpy/globals.py                 1.00   300.7±31.69µs     9.8 MB/sec    1.01   302.3±38.23µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.32ms     4.5 MB/sec    1.03      5.9±0.38ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.03     21.2±0.83ms  1966.2 KB/sec    1.00     20.5±0.81ms  2027.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.5±0.24ms     3.0 MB/sec    1.00      5.5±0.33ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   663.9±39.95µs     4.4 MB/sec    1.00   647.7±37.09µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.4±0.43ms     2.7 MB/sec    1.00      9.0±0.34ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.46ms     3.9 MB/sec    1.00     10.5±0.48ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.18ms     7.3 MB/sec    1.00      2.3±0.11ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.03   265.9±23.98µs    11.1 MB/sec    1.00   258.0±12.29µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.21ms     5.3 MB/sec    1.00      4.8±0.29ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-06-27 23:48_

---

_@konstin approved on 2023-06-28 05:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/resources/test/airflow/README.md`:3 on 2023-06-28 11:49_

It may be worth documenting why all these files are empty. Or potentially better, include a comment in all the files instead of just leaving them empty explaining why there's no content.

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/implicit_imports.rs`:1 on 2023-06-28 11:50_

What's the reasoning for replacing all hash maps with `BTreeMap`?

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/implicit_imports.rs`:131 on 2023-06-28 11:51_

Nit: Unrelated to this PR. We should probably wrap these types that we pass around frequently. It would reduce the scope of changes to internal data structures and could even allow us to implement some of these functions as methods. 

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/snapshots/ruff_python_resolver__resolver__tests__airflow_namespace_package.snap`:28 on 2023-06-28 11:52_

Unrelated to this PR. Are some of these booleans exclusive to each other? If so, should some of them be represented as an enum instead?

---

_@MichaReiser approved on 2023-06-28 11:52_

Nice!

---

_@charliermarsh reviewed on 2023-06-28 13:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/implicit_imports.rs`:1 on 2023-06-28 13:12_

Determinism in the tests.

---

_@charliermarsh reviewed on 2023-06-28 13:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/snapshots/ruff_python_resolver__resolver__tests__airflow_namespace_package.snap`:28 on 2023-06-28 13:13_

Yeah, the whole thing needs to be redesigned. It's intentionally a direct port of the Pyright data structure (optimized for correctness).

---

_Merged by @charliermarsh on 2023-06-28 13:38_

---

_Closed by @charliermarsh on 2023-06-28 13:38_

---

_Branch deleted on 2023-06-28 13:38_

---
