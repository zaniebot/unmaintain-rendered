```yaml
number: 5410
title: "Fill-in missing implementation for `is_native_module_file_name`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/native
created_at: 2023-06-28T00:02:58Z
updated_at: 2023-06-28T15:05:52Z
url: https://github.com/astral-sh/ruff/pull/5410
synced_at: 2026-01-12T03:36:55Z
```

# Fill-in missing implementation for `is_native_module_file_name`

---

_Pull request opened by @charliermarsh on 2023-06-28 00:02_

## Summary

This was just an oversight -- the last remaining `todo!()` that I never filled in. We clearly don't have any test coverage for it yet, but this mimics the Pyright implementation.


---

_Review requested from @konstin by @charliermarsh on 2023-06-28 00:03_

---

_Renamed from "Fill-in missing TODO for is_native_module_file_name" to "Fill-in missing implementation for `is_native_module_file_name`" by @charliermarsh on 2023-06-28 00:03_

---

_Comment by @github-actions[bot] on 2023-06-28 00:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      8.5±0.08ms     4.8 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1746.4±2.47µs     9.5 MB/sec    1.00  1745.1±14.36µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    187.9±0.32µs    15.7 MB/sec    1.00    188.7±1.30µs    15.6 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.04ms     6.5 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.00     13.8±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.1±0.88µs     8.0 MB/sec    1.01   375.7±15.53µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.01      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1479.1±4.65µs    11.3 MB/sec    1.00   1484.1±2.80µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.7±0.27µs    18.7 MB/sec    1.01    158.6±0.56µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.3±0.01ms     7.8 MB/sec    1.00      3.2±0.02ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     13.0±0.42ms     3.1 MB/sec    1.00     12.9±0.60ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.7±0.12ms     6.2 MB/sec    1.00      2.7±0.13ms     6.3 MB/sec
formatter/numpy/globals.py                 1.03   308.8±25.95µs     9.6 MB/sec    1.00   299.3±19.42µs     9.9 MB/sec
formatter/pydantic/types.py                1.05      5.9±0.26ms     4.3 MB/sec    1.00      5.7±0.21ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.03     22.7±0.45ms  1832.6 KB/sec    1.00     22.1±0.84ms  1886.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      6.0±0.25ms     2.8 MB/sec    1.00      5.6±0.18ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   692.1±26.07µs     4.3 MB/sec    1.00   679.3±32.46µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.05     10.0±0.44ms     2.5 MB/sec    1.00      9.5±0.29ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.03     11.6±0.31ms     3.5 MB/sec    1.00     11.2±0.34ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.4±0.08ms     7.0 MB/sec    1.00      2.4±0.06ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   285.0±15.97µs    10.4 MB/sec    1.00   280.5±24.53µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.04      5.2±0.50ms     4.9 MB/sec    1.00      5.0±0.24ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/native_module.rs`:26 on 2023-06-28 05:35_

I'd add the following test cases:
* `foo.abi3.so`
* `foo.cpython-38-x86_64-linux-gnu.so
* `foo.cp39-win_amd64.pyd`

It doesn't seem to support `foo.so`, which iirc also works (but you shouldn't use when shipping)

negative:
* `libopenblasp-r0-41284840.3.18.s`

---

_@konstin reviewed on 2023-06-28 05:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/native_module.rs`:13 on 2023-06-28 11:43_

Should we enable https://rust-lang.github.io/rust-clippy/master/index.html#/todo and https://rust-lang.github.io/rust-clippy/master/index.html#/dbg_macro for CI only?

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/native_module.rs`:18 on 2023-06-28 11:46_

What does the function return for `a.b.c.d.e`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_resolver/src/resolver.rs`:145 on 2023-06-28 11:47_

Would you mind documenting why the `unwrap` call here is safe? 

---

_@MichaReiser approved on 2023-06-28 11:48_

---

_@konstin reviewed on 2023-06-28 12:32_

---

_Review comment by @konstin on `crates/ruff_python_resolver/src/native_module.rs`:13 on 2023-06-28 12:32_

i like been able to run CI even if there are todos

---

_@charliermarsh reviewed on 2023-06-28 13:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/resolver.rs`:145 on 2023-06-28 13:12_

It's probably not, but I need to do a dedicated pass over all of the unwraps.

---

_@charliermarsh reviewed on 2023-06-28 14:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_resolver/src/native_module.rs`:26 on 2023-06-28 14:42_

Thank you, good call.

---

_Merged by @charliermarsh on 2023-06-28 14:50_

---

_Closed by @charliermarsh on 2023-06-28 14:50_

---

_Branch deleted on 2023-06-28 14:50_

---
