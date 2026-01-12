```yaml
number: 5418
title: format StmtTryStar
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-stmt-try-star
created_at: 2023-06-28T13:27:58Z
updated_at: 2023-07-07T20:47:42Z
url: https://github.com/astral-sh/ruff/pull/5418
synced_at: 2026-01-12T03:36:55Z
```

# format StmtTryStar

---

_Pull request opened by @davidszotten on 2023-06-28 13:27_

refactor StmtTry to share most of impl with StmtTryStar


---

_Comment by @github-actions[bot] on 2023-06-28 13:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12      9.2±0.03ms     4.4 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.09   1886.6±3.33µs     8.8 MB/sec    1.00   1730.6±2.74µs     9.6 MB/sec
formatter/numpy/globals.py                 1.05    198.9±0.96µs    14.8 MB/sec    1.00    188.7±0.39µs    15.6 MB/sec
formatter/pydantic/types.py                1.10      4.3±0.01ms     6.0 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.02     14.1±0.04ms     2.9 MB/sec    1.00     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.0±0.89µs     8.0 MB/sec    1.02    377.1±7.18µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.00      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1499.9±2.27µs    11.1 MB/sec    1.00   1480.6±4.09µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±0.58µs    18.6 MB/sec    1.00    157.5±1.39µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.5±0.04ms     4.3 MB/sec    1.00      9.3±0.07ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1988.0±11.63µs     8.4 MB/sec    1.00  1961.5±23.79µs     8.5 MB/sec
formatter/numpy/globals.py                 1.01    215.0±1.91µs    13.7 MB/sec    1.00   212.7±11.12µs    13.9 MB/sec
formatter/pydantic/types.py                1.03      4.5±0.04ms     5.7 MB/sec    1.00      4.4±0.04ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.11ms     2.7 MB/sec    1.01     15.4±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.01      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±4.67µs     6.9 MB/sec    1.00    426.6±4.56µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.04ms     5.0 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1678.2±14.68µs     9.9 MB/sec    1.00  1662.2±11.48µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.1±1.47µs    16.5 MB/sec    1.00    178.3±1.55µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @davidszotten on 2023-06-28 14:34_

---

_Label `formatter` added by @MichaReiser on 2023-06-28 16:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:12 on 2023-06-28 16:45_

Nit: I think an enum `Starred` with the options `Yes` and `No` or `ExceptHandlerKind` with the variants `Starred` and `Regular` would improve readability on the `with_options` call site 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:42 on 2023-06-28 16:46_

Nit
```suggestion
        write!(f, [text("except"), self.has_star.then_some(text("*"))])?;
```

Calling `text` is cheap (it compiles to a const)

---

_@MichaReiser approved on 2023-06-28 16:47_

Nice! I've two small nits. Let me know if you want to address them or if you prefer the code as is.

---

_@davidszotten reviewed on 2023-06-28 19:25_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:42 on 2023-06-28 19:25_

wasn't saving the call, just missed that method existing 😆 

---

_Merged by @MichaReiser on 2023-06-29 06:07_

---

_Closed by @MichaReiser on 2023-06-29 06:07_

---

_Branch deleted on 2023-07-07 20:47_

---
