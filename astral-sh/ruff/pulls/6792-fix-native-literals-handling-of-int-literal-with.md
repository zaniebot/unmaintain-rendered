```yaml
number: 6792
title: "Fix `native-literals` handling of int literal with attribute access"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: fix/native-literals
created_at: 2023-08-22T19:05:08Z
updated_at: 2023-08-23T16:34:58Z
url: https://github.com/astral-sh/ruff/pull/6792
synced_at: 2026-01-12T02:45:38Z
```

# Fix `native-literals` handling of int literal with attribute access

---

_Pull request opened by @zanieb on 2023-08-22 19:05_

Closes https://github.com/astral-sh/ruff/issues/6788 by special casing integer literals with attribute access — either retaining parenthesis for literals with values (e.g. `int(7).denominator` to `(7).denominator)` or leaving calls without values (e.g. `int().denominator`) unchanged.

---

_Label `bug` added by @zanieb on 2023-08-22 19:06_

---

_Label `fuzzer` added by @zanieb on 2023-08-22 19:06_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/pyupgrade/UP018.py`:54 on 2023-08-22 19:20_

```suggestion
# These become a literal but retain parentheses
```

---

_@zanieb reviewed on 2023-08-22 19:20_

---

_Comment by @zanieb on 2023-08-22 19:28_

Are there some parenthesized edge-cases I don't know of? Is there an idiomatic way to add parens to an expression?

---

_Comment by @github-actions[bot] on 2023-08-22 19:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07      3.6±0.04ms    11.4 MB/sec    1.00      3.3±0.05ms    12.2 MB/sec
formatter/numpy/ctypeslib.py               1.05   709.7±19.13µs    23.5 MB/sec    1.00    677.5±5.26µs    24.6 MB/sec
formatter/numpy/globals.py                 1.03     74.2±0.49µs    39.8 MB/sec    1.00     72.1±1.50µs    40.9 MB/sec
formatter/pydantic/types.py                1.04   1412.7±9.77µs    18.1 MB/sec    1.00  1354.9±30.32µs    18.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.05ms     3.9 MB/sec    1.01     10.4±0.05ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.9±0.03ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    313.8±1.19µs     9.4 MB/sec    1.02    321.1±1.92µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.02ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.4 MB/sec    1.01      5.6±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1180.3±2.26µs    14.1 MB/sec    1.00   1178.0±3.41µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.2±0.38µs    24.3 MB/sec    1.03    125.0±2.60µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.01      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.18      5.2±0.22ms     7.8 MB/sec    1.00      4.4±0.35ms     9.2 MB/sec
formatter/numpy/ctypeslib.py               1.16  1026.1±56.34µs    16.2 MB/sec    1.00   884.4±90.02µs    18.8 MB/sec
formatter/numpy/globals.py                 1.03     96.6±6.40µs    30.5 MB/sec    1.00     94.0±7.77µs    31.4 MB/sec
formatter/pydantic/types.py                1.17      2.2±0.18ms    11.8 MB/sec    1.00  1843.6±100.35µs    13.8 MB/sec
linter/all-rules/large/dataset.py          1.01     17.5±0.82ms     2.3 MB/sec    1.00     17.4±0.98ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.39ms     3.6 MB/sec    1.01      4.7±0.36ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   567.5±34.49µs     5.2 MB/sec    1.06   602.9±44.78µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.44ms     3.0 MB/sec    1.05      8.9±0.50ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.53ms     4.4 MB/sec    1.02      9.5±0.59ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1967.2±88.30µs     8.5 MB/sec    1.03      2.0±0.11ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   244.9±14.95µs    12.0 MB/sec    1.00   242.5±18.34µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.29ms     6.0 MB/sec    1.04      4.4±0.33ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-22 20:54_

> Is there an idiomatic way to add parens to an expression?

I would just do `format("({})", locator.slice(expr.range()))`.

---

_Marked ready for review by @zanieb on 2023-08-22 21:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/expression.rs`:447 on 2023-08-23 03:31_

Since we already pass in `checker`, might more straightforward to do this call in the rule itself?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/native_literals.rs`:146 on 2023-08-23 03:31_

Can you do `func.as_ref()` instead of `**func`?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/native_literals.rs`:180 on 2023-08-23 03:32_

I assume floats and complex are fine?

---

_@charliermarsh approved on 2023-08-23 03:32_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/analyze/expression.rs`:447 on 2023-08-23 13:58_

Well, I wasn't sure if the rule should assume the expression it's been given is the current one and it seemed safer to push that assumption up to the visitor since it _knows_ that's the case. I'm happy to move it if we tend to make that assumption though!

---

_@zanieb reviewed on 2023-08-23 13:58_

---

_@zanieb reviewed on 2023-08-23 13:59_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/native_literals.rs`:180 on 2023-08-23 13:59_

I was a little scared to test them and get sucked into more issues, but I'll look into it.

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyupgrade/rules/native_literals.rs`:180 on 2023-08-23 15:41_

We don't support complex here and floats are fine.

---

_@zanieb reviewed on 2023-08-23 15:41_

---

_Merged by @zanieb on 2023-08-23 16:22_

---

_Closed by @zanieb on 2023-08-23 16:22_

---

_Branch deleted on 2023-08-23 16:22_

---
