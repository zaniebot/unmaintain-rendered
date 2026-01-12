```yaml
number: 5376
title: format StmtAsyncWith
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-stmt-asyncwith
created_at: 2023-06-26T16:33:04Z
updated_at: 2023-07-07T20:47:54Z
url: https://github.com/astral-sh/ruff/pull/5376
synced_at: 2026-01-12T15:55:18Z
```

# format StmtAsyncWith

---

_@davidszotten_

refactor `FormatStmtWith` and share impl with `FormatStmtAsyncWith`

tests: snapshots

ref: #5368

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_async_with.rs`:12 on 2023-06-26 16:46_

This is fine pattern in general, but we've been mostly using structs with option fields instead, e.g. `struct FormatWithLike { async: bool, ... /*shared fields of with and async with*/ }`

---

_@konstin reviewed on 2023-06-26 16:46_

---

_Comment by @github-actions[bot] on 2023-06-26 17:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1543.8±1.61µs    10.8 MB/sec    1.00   1543.5±7.16µs    10.8 MB/sec
formatter/numpy/globals.py                 1.01    187.9±1.37µs    15.7 MB/sec    1.00    186.9±2.04µs    15.8 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.01ms     7.4 MB/sec    1.00      3.5±0.01ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.03     13.4±0.03ms     3.0 MB/sec    1.00     13.0±0.02ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    430.2±0.50µs     6.9 MB/sec    1.00    427.3±1.68µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.0±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.05      7.0±0.01ms     5.8 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1518.3±2.13µs    11.0 MB/sec    1.00   1465.6±3.04µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.03    168.1±1.79µs    17.6 MB/sec    1.00    163.4±0.49µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.1±0.01ms     8.1 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.07ms     5.2 MB/sec    1.01      7.9±0.10ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1648.8±10.36µs    10.1 MB/sec    1.01   1661.5±9.26µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    187.9±1.51µs    15.7 MB/sec    1.02    191.3±2.35µs    15.4 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.03ms     6.8 MB/sec    1.00      3.8±0.02ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.26ms     2.7 MB/sec    1.00     14.9±0.22ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.07ms     4.6 MB/sec    1.00      3.6±0.05ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    409.4±5.85µs     7.2 MB/sec    1.00    408.6±5.15µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.07ms     4.0 MB/sec    1.00      6.4±0.08ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.08ms     5.4 MB/sec    1.00      7.6±0.06ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1528.6±14.09µs    10.9 MB/sec    1.00   1532.1±9.50µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.7±1.35µs    17.6 MB/sec    1.01    169.1±1.69µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.02ms     7.7 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@davidszotten reviewed on 2023-06-26 18:32_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/statement/stmt_async_with.rs`:12 on 2023-06-26 18:32_

not sure i follow how that would work. micha [suggested](https://github.com/astral-sh/ruff/issues/5368#issuecomment-1607579684) a union enum or a trait

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_async_with.rs`:12 on 2023-06-26 18:45_

sorry, than continue doing that

---

_@konstin reviewed on 2023-06-26 18:45_

---

_Marked ready for review by @davidszotten on 2023-06-26 20:07_

---

_Label `formatter` added by @konstin on 2023-06-28 06:53_

---

_@MichaReiser approved on 2023-06-28 10:14_

---

_Merged by @MichaReiser on 2023-06-28 10:21_

---

_Closed by @MichaReiser on 2023-06-28 10:21_

---

_Comment by @davidszotten on 2023-06-28 10:29_

could you elaborate on what you mean by "to make ASYNC more explicit"?

(given that commit can do the same for try/trystar)

---

_Comment by @MichaReiser on 2023-06-28 13:11_

Sure.

My main thinking is to use an `enum` if the implementations for the different types do not need to customize the formatting (significantly). Here, the only difference is the `async` and we can simply detect this by testing if the enum variant is `AsyncWith`. The main reason is that the `trait` doesn't integrate as nicely into the `Format` infrastructure because the trait cannot override `Format::fmt`, instead it is necessary to call the custom `fmt_<node>` method. 



---

_Comment by @davidszotten on 2023-06-28 14:36_

ok. implementing `Format` is nice (though this approach does add a fair amount of boilerplate, see e.g. https://github.com/astral-sh/ruff/pull/5418/files )

---

_Comment by @MichaReiser on 2023-06-28 16:41_

> ok. implementing `Format` is nice (though this approach does add a fair amount of boilerplate, see e.g. [#5418 (files)](https://github.com/astral-sh/ruff/pull/5418/files) )

That's true!. Rome uses a `declare_node_union!` macro because declaring enums over a set of variants turned out to be an extremely valuable pattern. I think we could write something similar in the future to cut down on the boilerplate

https://github.com/rome/tools/blob/38104b339da8ff688f469799e1fc3f4a46f3d2ec/crates/rome_rowan/src/macros.rs#L47-L141

The rome version generates a bit more than what we need here but I found it super cool.

---

_Branch deleted on 2023-07-07 20:47_

---
