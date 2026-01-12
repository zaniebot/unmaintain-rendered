```yaml
number: 6319
title: Add API to chain comment placement operations
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/or-else
created_at: 2023-08-03T21:22:26Z
updated_at: 2023-08-04T12:50:39Z
url: https://github.com/astral-sh/ruff/pull/6319
synced_at: 2026-01-12T15:55:21Z
```

# Add API to chain comment placement operations

---

_@charliermarsh_

## Summary

This PR adds an API for chaining comment placement methods based on the [`then_with`](https://doc.rust-lang.org/std/cmp/enum.Ordering.html#method.then_with) from `Ordering` in the standard library.

For example, you can now do:

```rust
try_some_case(comment).then_with(|comment| try_some_other_case_if_still_default(comment))
```

This lets us avoid this kind of pattern, which I've seen in `placement.rs` and used myself before:

```rust
let comment = match handle_own_line_comment_between_branches(comment, preceding, locator) {
    CommentPlacement::Default(comment) => comment,
    placement => return placement,
};
```


---

_Renamed from "Then-with" to "An API to chain comment placement operations" by @charliermarsh on 2023-08-03 21:22_

---

_Renamed from "An API to chain comment placement operations" to "Add API to chain comment placement operations" by @charliermarsh on 2023-08-03 21:22_

---

_Label `formatter` added by @charliermarsh on 2023-08-03 21:31_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 21:37_

---

_Review requested from @konstin by @charliermarsh on 2023-08-03 21:37_

---

_Comment by @github-actions[bot] on 2023-08-03 21:54_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.09ms     4.1 MB/sec    1.00      9.9±0.08ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1946.4±18.49µs     8.6 MB/sec    1.00  1944.1±11.75µs     8.6 MB/sec
formatter/numpy/globals.py                 1.02    221.4±6.55µs    13.3 MB/sec    1.00    216.8±2.40µs    13.6 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.10ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.12ms     3.1 MB/sec    1.01     13.3±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.4±2.56µs     6.5 MB/sec    1.01   463.8±25.99µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.3 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.04ms     5.9 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1420.0±9.05µs    11.7 MB/sec    1.01   1434.9±9.52µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.2±1.37µs    18.8 MB/sec    1.01    158.2±0.38µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.13ms     4.1 MB/sec    1.00     10.0±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1921.1±27.55µs     8.7 MB/sec    1.00  1925.5±28.22µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    212.4±4.61µs    13.9 MB/sec    1.00    212.4±8.17µs    13.9 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.01      4.2±0.07ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.02     13.8±0.19ms     3.0 MB/sec    1.00     13.5±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.04ms     4.7 MB/sec    1.00      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    445.4±6.20µs     6.6 MB/sec    1.00   441.2±14.11µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.09ms     4.1 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.07ms     5.5 MB/sec    1.00      7.3±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1508.3±17.49µs    11.0 MB/sec    1.00  1496.0±21.74µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±3.57µs    17.3 MB/sec    1.00    170.2±3.71µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-08-04 00:01_

---

_Comment by @zanieb on 2023-08-04 00:01_

I like it.

---

_Merged by @charliermarsh on 2023-08-04 01:08_

---

_Closed by @charliermarsh on 2023-08-04 01:08_

---

_Branch deleted on 2023-08-04 01:08_

---

_@MichaReiser reviewed on 2023-08-04 05:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:666 on 2023-08-04 05:50_

Nit: I would prefer `or_else` to match the `Option` and `Result` naming (which is what we used to have before)

---

_@charliermarsh reviewed on 2023-08-04 12:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/visitor.rs`:666 on 2023-08-04 12:50_

Interesting, this to me is more similar to the ordering semantics so following their naming felt more natural to me, I will change though.

---
