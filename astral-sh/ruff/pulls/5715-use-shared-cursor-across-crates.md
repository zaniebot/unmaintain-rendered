```yaml
number: 5715
title: "Use shared `Cursor` across crates"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cursor
created_at: 2023-07-12T16:42:18Z
updated_at: 2023-07-12T21:26:08Z
url: https://github.com/astral-sh/ruff/pull/5715
synced_at: 2026-01-12T03:36:55Z
```

# Use shared `Cursor` across crates

---

_Pull request opened by @charliermarsh on 2023-07-12 16:42_

## Summary

We have two `Cursor` implementations. This PR moves the implementation from the formatter into `ruff_python_whitespace` (kind of a poorly-named crate now) and uses it for both use-cases.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-12 16:42_

---

_@charliermarsh reviewed on 2023-07-12 16:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:254 on 2023-07-12 16:42_

Only semi-related (noticed during the refactor), but we no longer need to do this -- we have ranges on the names.

---

_Label `internal` added by @charliermarsh on 2023-07-12 17:17_

---

_Comment by @github-actions[bot] on 2023-07-12 17:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.12ms     4.3 MB/sec    1.00      9.4±0.13ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.03ms     7.5 MB/sec    1.00      2.2±0.03ms     7.6 MB/sec
formatter/numpy/globals.py                 1.02    251.7±0.98µs    11.7 MB/sec    1.00    248.0±4.39µs    11.9 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.3 MB/sec    1.00      4.8±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.07     17.2±0.22ms     2.4 MB/sec    1.00     16.0±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.2±0.05ms     4.0 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.04    522.0±4.95µs     5.7 MB/sec    1.00    501.2±7.84µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.5±0.12ms     3.4 MB/sec    1.00      7.1±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.14      9.1±0.10ms     4.5 MB/sec    1.00      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09  1872.6±38.98µs     8.9 MB/sec    1.00  1720.7±25.50µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.06    206.3±3.15µs    14.3 MB/sec    1.00    194.7±2.39µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.10      4.0±0.05ms     6.4 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.09ms     4.3 MB/sec    1.11     10.5±0.09ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.08      2.3±0.04ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00    240.6±4.57µs    12.3 MB/sec    1.05    253.7±6.13µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.4 MB/sec    1.08      5.1±0.06ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.10ms     2.6 MB/sec    1.00     15.6±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.4±5.82µs     6.0 MB/sec    1.00    493.7±6.12µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.00      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.5±13.55µs     9.8 MB/sec    1.00  1692.1±18.19µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.7±2.72µs    14.9 MB/sec    1.00    198.3±2.96µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.04ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/identifier.rs`:231 on 2023-07-12 20:45_

You can avoid some of the offset math by calling cursor.start_token before the match (and I think cursor also gives you the offset, you can store that in a stack variable)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/identifier.rs`:237 on 2023-07-12 20:46_

I wonder if we should panic on any other token to avoid that the tokenizer eg returns a name from inside a string (because it happily lex over quotes)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/identifier.rs`:232 on 2023-07-12 20:47_

We also need to test that it is not a string prefix. 

---

_@MichaReiser approved on 2023-07-12 20:48_

---

_@charliermarsh reviewed on 2023-07-12 20:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:231 on 2023-07-12 20:55_

IDK this part is pretty confusing. I think I'd have to call `start_token` before every bump, and then handle adding offsets in every branch of the match here? (cursor doesn't seem to expose an offset. My old implementation did, which simplified this specific usage a little, but I didn't feel strongly about preserving.)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:232 on 2023-07-12 20:56_

This tokenizer makes a lot of assumptions, one of which is that you're parsing code that can't contain strings (e.g., the `else` in `else:`).

---

_@charliermarsh reviewed on 2023-07-12 20:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:232 on 2023-07-12 20:59_

@MichaReiser - Okay, I think this simplified things a little...

---

_@charliermarsh reviewed on 2023-07-12 21:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/identifier.rs`:231 on 2023-07-12 21:08_

Ah yeah sorry. You would need to call start before every bump. 

---

_@MichaReiser reviewed on 2023-07-12 21:08_

---

_@charliermarsh reviewed on 2023-07-12 21:08_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:231 on 2023-07-12 21:08_

Apology... accepted!

---

_Merged by @charliermarsh on 2023-07-12 21:09_

---

_Closed by @charliermarsh on 2023-07-12 21:09_

---

_Branch deleted on 2023-07-12 21:09_

---
