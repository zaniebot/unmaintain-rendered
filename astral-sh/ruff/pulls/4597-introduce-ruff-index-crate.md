```yaml
number: 4597
title: "Introduce `ruff_index` crate"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: ruff-idx
created_at: 2023-05-23T11:15:48Z
updated_at: 2023-05-23T15:40:36Z
url: https://github.com/astral-sh/ruff/pull/4597
synced_at: 2026-01-12T03:50:03Z
```

# Introduce `ruff_index` crate

---

_Pull request opened by @MichaReiser on 2023-05-23 11:15_

We have multiple places where we use a custom `Id` to index into a Vector. I now plan to add one new index and thought it might be good to reduce the necessary boilerplate. 

This PR formalizes this approach by introducing new `IndexVec` and `IndexSlice` new type wrappers and thew `newtype_index` macro that allows you to define your own index. 

This work is heavily inspired by [rustc `index` crate](https://github.com/rust-lang/rust/blob/master/compiler/rustc_index/src/lib.rs) and thanks to @boshen for making me aware of it :)

---

_Label `internal` added by @MichaReiser on 2023-05-23 11:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:164 on 2023-05-23 11:17_

We still need these for custom newtype wrappers that want to expose the underlying slices (generally discouraged)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:135 on 2023-05-23 11:17_

Less `usize::from(..)` calls :)

---

_@MichaReiser reviewed on 2023-05-23 11:18_

---

_Comment by @github-actions[bot] on 2023-05-23 12:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.94ms     2.5 MB/sec    1.02     16.9±0.43ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.2±0.15ms     4.0 MB/sec    1.00      4.0±0.19ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   523.3±27.55µs     5.6 MB/sec    1.00   515.3±23.56µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.32ms     3.7 MB/sec    1.05      7.3±0.41ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.24ms     5.0 MB/sec    1.00      8.0±0.21ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1783.6±42.38µs     9.3 MB/sec    1.00  1702.7±66.04µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.7±8.85µs    14.6 MB/sec    1.04   211.4±12.11µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.8±0.12ms     6.7 MB/sec    1.00      3.6±0.17ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.5±0.19ms     6.3 MB/sec    1.02      6.6±0.32ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1273.0±56.83µs    13.1 MB/sec    1.01  1288.2±48.24µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    120.3±6.37µs    24.5 MB/sec    1.02    123.3±7.78µs    23.9 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.15ms     9.5 MB/sec    1.07      2.9±0.14ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     23.2±1.02ms  1799.0 KB/sec    1.00     22.3±1.07ms  1867.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.25ms     3.0 MB/sec    1.01      5.6±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   681.7±35.85µs     4.3 MB/sec    1.00   667.9±51.71µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.39ms     2.8 MB/sec    1.01      9.3±0.33ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.02     11.4±0.43ms     3.6 MB/sec    1.00     11.2±0.66ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05      2.3±0.10ms     7.1 MB/sec    1.00      2.2±0.12ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   277.7±17.94µs    10.6 MB/sec    1.00   272.6±33.55µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.1±0.20ms     5.0 MB/sec    1.00      4.9±0.19ms     5.2 MB/sec
parser/large/dataset.py                    1.01      9.1±0.41ms     4.5 MB/sec    1.00      9.1±0.44ms     4.5 MB/sec
parser/numpy/ctypeslib.py                  1.01  1714.3±87.58µs     9.7 MB/sec    1.00  1697.8±74.38µs     9.8 MB/sec
parser/numpy/globals.py                    1.00   170.2±10.79µs    17.3 MB/sec    1.07   182.8±11.47µs    16.1 MB/sec
parser/pydantic/types.py                   1.00      3.8±0.10ms     6.7 MB/sec    1.01      3.8±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/binding.rs`:133 on 2023-05-23 12:56_

Can this be written as a derive macro? I assume not (as-is) because it redefines the `struct` itself as `BindingId(NonZeroU32)` rather than just adding impls.

---

_@charliermarsh approved on 2023-05-23 12:56_

---

_Comment by @charliermarsh on 2023-05-23 12:59_

Looks great!

---

_Comment by @Boshen on 2023-05-23 13:33_

Nice! Let me quickly steal some of the code back 😃 

Edit: I forgot to search for a crate, and there is one! https://crates.io/crates/index_vec

---

_Comment by @MichaReiser on 2023-05-23 14:04_

> Edit: I forgot to search for a crate, and there is one! [crates.io/crates/index_vec](https://crates.io/crates/index_vec)

Oh nice. I think I still go with our implementation so that we can customize the API (e.g. I intentionally omitted all removal APIs). 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:133 on 2023-05-23 14:05_

No, because of the reason you're stating. 

---

_@MichaReiser reviewed on 2023-05-23 14:05_

---

_Comment by @charliermarsh on 2023-05-23 15:24_

I'd like to use this for references, so merge at your leisure :)

---

_Merged by @MichaReiser on 2023-05-23 15:40_

---

_Closed by @MichaReiser on 2023-05-23 15:40_

---

_Branch deleted on 2023-05-23 15:40_

---
