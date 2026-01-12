```yaml
number: 6722
title: Simplify suite formatting
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: simpler-suite-formatting
created_at: 2023-08-21T09:42:50Z
updated_at: 2023-08-21T19:01:53Z
url: https://github.com/astral-sh/ruff/pull/6722
synced_at: 2026-01-12T02:52:04Z
```

# Simplify suite formatting

---

_Pull request opened by @konstin on 2023-08-21 09:42_

Avoid the nesting in a macro by using the new `WithNodeLevel` to `PyFormatter` deref. No changes otherwise.

I wanted to follow this up with quickly fixing the typeshed empty line rules but they turned out a lot more complex than i had anticipated. 

---

_Label `internal` added by @konstin on 2023-08-21 09:42_

---

_Label `formatter` added by @konstin on 2023-08-21 09:42_

---

_Review requested from @MichaReiser by @konstin on 2023-08-21 09:42_

---

_Marked ready for review by @konstin on 2023-08-21 09:42_

---

_@konstin reviewed on 2023-08-21 09:43_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:323 on 2023-08-21 09:43_

this is `cargo +nightly fmt`

---

_Comment by @github-actions[bot] on 2023-08-21 10:20_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.02ms    12.4 MB/sec    1.00      3.3±0.01ms    12.5 MB/sec
formatter/numpy/ctypeslib.py               1.01   678.0±13.66µs    24.6 MB/sec    1.00   673.2±12.12µs    24.7 MB/sec
formatter/numpy/globals.py                 1.01     70.7±0.35µs    41.8 MB/sec    1.00     70.2±0.31µs    42.1 MB/sec
formatter/pydantic/types.py                1.01  1326.2±13.81µs    19.2 MB/sec    1.00  1316.1±32.37µs    19.4 MB/sec
linter/all-rules/large/dataset.py          1.01     10.6±0.06ms     3.8 MB/sec    1.00     10.6±0.05ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.00      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    326.7±1.15µs     9.0 MB/sec    1.00    327.4±1.92µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.06ms     4.6 MB/sec    1.00      5.5±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.6±0.03ms     7.3 MB/sec    1.00      5.5±0.02ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1181.8±5.78µs    14.1 MB/sec    1.00   1176.5±2.51µs    14.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.6±0.34µs    24.1 MB/sec    1.02    124.8±0.56µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.5±0.23ms     9.0 MB/sec    1.00      4.4±0.23ms     9.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   897.4±35.68µs    18.6 MB/sec    1.02   919.0±59.78µs    18.1 MB/sec
formatter/numpy/globals.py                 1.01     93.3±4.78µs    31.6 MB/sec    1.00     92.7±8.97µs    31.8 MB/sec
formatter/pydantic/types.py                1.05  1836.4±86.04µs    13.9 MB/sec    1.00  1742.7±74.01µs    14.6 MB/sec
linter/all-rules/large/dataset.py          1.08     17.7±1.41ms     2.3 MB/sec    1.00     16.4±0.69ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.11      4.9±0.64ms     3.4 MB/sec    1.00      4.4±0.19ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.08   607.6±67.04µs     4.9 MB/sec    1.00   560.9±36.16µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.17      9.5±0.99ms     2.7 MB/sec    1.00      8.1±0.24ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.05      9.3±0.74ms     4.4 MB/sec    1.00      8.9±0.43ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12      2.0±0.11ms     8.2 MB/sec    1.00  1819.5±77.64µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   241.6±15.39µs    12.2 MB/sec    1.00   238.9±10.31µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.1±0.16ms     6.2 MB/sec    1.00      3.9±0.19ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:63 on 2023-08-21 15:08_

Is the `&mut` assignment necessary? Most call sites simply use `let mut f = WithNodeLevel::new(node_level, f)`

---

_@MichaReiser reviewed on 2023-08-21 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:62 on 2023-08-21 15:12_

My reasoning for using `format_with` is that `Formatter::write_fmt` (which `write` calls) is a static dispatch. Whereas `X.write_fmt` requires creating a new `Formatter`. But it probably doesn't matter and is just noise.

---

_@MichaReiser approved on 2023-08-21 15:12_

---

_@konstin reviewed on 2023-08-21 17:28_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:62 on 2023-08-21 17:28_

but we don't call `write_fmt`, do we?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:63 on 2023-08-21 17:31_

To my surprise `let f = &mut WithNodeLevel::new(node_level, f);` works, i'd have expected that it wants a let binding for that

---

_@konstin reviewed on 2023-08-21 17:31_

---

_@MichaReiser reviewed on 2023-08-21 17:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:62 on 2023-08-21 17:51_

Ah, this works because `WithNodeLevel` is generic over `Buffer`

---

_Merged by @konstin on 2023-08-21 19:01_

---

_Closed by @konstin on 2023-08-21 19:01_

---

_Branch deleted on 2023-08-21 19:01_

---
