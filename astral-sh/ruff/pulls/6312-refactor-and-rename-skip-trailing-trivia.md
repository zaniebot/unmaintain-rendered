```yaml
number: 6312
title: "Refactor and rename `skip_trailing_trivia`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/deco
created_at: 2023-08-03T16:13:41Z
updated_at: 2023-08-04T13:57:53Z
url: https://github.com/astral-sh/ruff/pull/6312
synced_at: 2026-01-12T02:52:04Z
```

# Refactor and rename `skip_trailing_trivia`

---

_Pull request opened by @charliermarsh on 2023-08-03 16:13_

Based on feedback here: https://github.com/astral-sh/ruff/pull/6274#discussion_r1282747964.

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:74 on 2023-08-03 16:14_

Should this return `Option<TextSize>`?

---

_@charliermarsh reviewed on 2023-08-03 16:14_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 16:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:83 on 2023-08-03 16:42_

Nit: How about 

```
token.kind == TokenKind::Newline || !token.kind.is_trivia()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:74 on 2023-08-03 16:46_

I'm unsure, and maybe the name is still unclear. I thought about that too when commenting. 

This returns the position of the line end, or the start position of the next non-trivia token. The second case doesn't seem to matter for all usages. 

Since we don't know the answer, should we change the method to `lines_after_ignoring_trivia` or similar, because we have no need for the more generic case? This also allows counting the newline tokens (skip while not a new line and trivia, then take while newline, count)

---

_@MichaReiser reviewed on 2023-08-03 16:46_

---

_Comment by @github-actions[bot] on 2023-08-03 16:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.9±0.12ms     5.1 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1593.2±24.39µs    10.5 MB/sec    1.00  1593.6±23.21µs    10.4 MB/sec
formatter/numpy/globals.py                 1.01    182.2±1.82µs    16.2 MB/sec    1.00    181.0±3.36µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.03ms     7.6 MB/sec    1.00      3.4±0.06ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.7±0.06ms     3.8 MB/sec    1.03     11.0±0.06ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.2±0.72µs     7.8 MB/sec    1.01    384.2±0.46µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.02ms     5.2 MB/sec    1.02      5.0±0.04ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.2±0.01ms     7.9 MB/sec    1.02      5.3±0.01ms     7.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1115.3±4.09µs    14.9 MB/sec    1.01  1129.5±17.60µs    14.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    125.3±1.50µs    23.6 MB/sec    1.01    127.0±3.41µs    23.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.01ms    10.9 MB/sec    1.02      2.4±0.04ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.03ms     5.4 MB/sec    1.01      7.6±0.04ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1464.4±13.20µs    11.4 MB/sec    1.02   1488.4±7.60µs    11.2 MB/sec
formatter/numpy/globals.py                 1.00    152.1±1.74µs    19.4 MB/sec    1.01    154.4±3.15µs    19.1 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     8.0 MB/sec    1.01      3.3±0.03ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.01     10.7±0.04ms     3.8 MB/sec    1.00     10.6±0.05ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    295.1±3.69µs    10.0 MB/sec    1.00    294.3±2.50µs    10.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      4.8±0.02ms     5.3 MB/sec    1.00      4.8±0.02ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.01      5.5±0.03ms     7.4 MB/sec    1.00      5.4±0.04ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1087.5±11.52µs    15.3 MB/sec    1.00   1073.6±6.34µs    15.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    112.0±0.82µs    26.3 MB/sec    1.00    112.0±0.82µs    26.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.02ms    10.7 MB/sec    1.00      2.4±0.01ms    10.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:73 on 2023-08-03 18:22_

The docstring is kinda confusing, do we always know that there is no none-trivia before the newline, and what if there is?

---

_@konstin approved on 2023-08-03 18:22_

---

_@charliermarsh reviewed on 2023-08-03 20:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:73 on 2023-08-03 20:39_

Haha didn't you write this!??

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:74 on 2023-08-03 20:44_

Ah I see, we always call `lines_after`. Ok, got it, will change.

---

_@charliermarsh reviewed on 2023-08-03 20:44_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:73 on 2023-08-03 20:44_

git blame says i'm innocent (but it could as well have been mine)

---

_@konstin reviewed on 2023-08-03 20:44_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 20:57_

---

_Review requested from @konstin by @charliermarsh on 2023-08-03 20:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:55 on 2023-08-03 21:05_

It seemed reasonable to me to rewrite this one too for consistency, but don't feel strongly.

---

_@charliermarsh reviewed on 2023-08-03 21:05_

---

_@charliermarsh reviewed on 2023-08-03 21:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:84 on 2023-08-03 21:05_

Awkward to cast to `u32`, but the other methods in this file return `u32`, and the format builders that take the return value as argument also expect `u32` 🤷 

---

_@konstin reviewed on 2023-08-03 21:09_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:84 on 2023-08-03 21:09_

rust is build around assuming everything is `usize` and doesn't expect our u32 optimizations, so i'm fine if a bunch of casting

---

_Label `formatter` added by @charliermarsh on 2023-08-03 21:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:55 on 2023-08-04 06:00_

I prefer the old implementation to keep it consistent with `lines_before`.

Using the string-based approach also has the benefit that we can get away with much simpler code (`lines_after is 20 lines, `SimpleTokenizer` is a couple of hundred lines).

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:84 on 2023-08-04 06:01_

It may be worth adding a `SAFETY: ...` comment explaining why casting down is fine. 

---

_@MichaReiser approved on 2023-08-04 06:01_

---

_@charliermarsh reviewed on 2023-08-04 13:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:55 on 2023-08-04 13:17_

Okay sounds good. Note that either way one of these three methods will be inconsistent with the others.

---

_Merged by @charliermarsh on 2023-08-04 13:30_

---

_Closed by @charliermarsh on 2023-08-04 13:30_

---

_Branch deleted on 2023-08-04 13:30_

---
