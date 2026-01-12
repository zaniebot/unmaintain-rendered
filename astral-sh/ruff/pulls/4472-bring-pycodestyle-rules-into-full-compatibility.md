```yaml
number: 4472
title: Bring pycodestyle rules into full compatibility (on SciPy)
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pycodestyle
created_at: 2023-05-17T14:48:15Z
updated_at: 2023-05-17T17:06:02Z
url: https://github.com/astral-sh/ruff/pull/4472
synced_at: 2026-01-12T03:50:03Z
```

# Bring pycodestyle rules into full compatibility (on SciPy)

---

_Pull request opened by @charliermarsh on 2023-05-17 14:48_

## Summary

This PR contains a handful of bug fixes that were necessary to get our pycodestyle rules to output identical diagnostics to pycodestyle itself when run on the SciPy repo (which I'm using as "a large, diverse codebase that does not use Black and includes some known violations").

For posterity, here's the script I used to diff against SciPy, which could in theory be applied to any other project: https://gist.github.com/charliermarsh/3f2855f33960c92306c91d003b6237dc.


---

_Comment by @charliermarsh on 2023-05-17 14:48_

I'm going to carve a couple of these out into separate PRs, so draft for now.

---

_Comment by @github-actions[bot] on 2023-05-17 14:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.8±0.11ms     2.8 MB/sec    1.00     14.4±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.5±1.49µs     6.9 MB/sec    1.00    428.3±1.25µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.05ms     4.2 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.03      7.0±0.10ms     5.8 MB/sec    1.00      6.7±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1479.2±2.43µs    11.3 MB/sec    1.00   1460.6±2.32µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.76µs    18.0 MB/sec    1.00    163.6±3.36µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.01ms     7.4 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1067.9±0.76µs    15.6 MB/sec    1.00   1063.2±1.82µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    109.8±0.34µs    26.9 MB/sec    1.00    108.5±0.21µs    27.2 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.01     20.3±0.57ms  2047.5 KB/sec     1.00     20.2±0.59ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.18ms     3.2 MB/sec     1.00      5.2±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   620.1±30.62µs     4.8 MB/sec     1.00   618.0±27.92µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.6±0.31ms     3.0 MB/sec     1.00      8.5±0.26ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.36ms     4.0 MB/sec     1.00     10.2±0.33ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.8 MB/sec     1.00      2.1±0.11ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.1±13.60µs    11.5 MB/sec     1.00   256.3±11.90µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.14ms     5.7 MB/sec     1.00      4.5±0.17ms     5.7 MB/sec
parser/large/dataset.py                    1.03      8.5±0.21ms     4.8 MB/sec     1.00      8.3±0.16ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.02  1645.0±125.33µs    10.1 MB/sec    1.00  1613.4±63.32µs    10.3 MB/sec
parser/numpy/globals.py                    1.02    168.1±8.40µs    17.6 MB/sec     1.00   164.8±17.97µs    17.9 MB/sec
parser/pydantic/types.py                   1.04      3.7±0.17ms     6.9 MB/sec     1.00      3.6±0.12ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-05-17 14:57_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-17 14:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/logical_lines.rs`:60 on 2023-05-17 14:57_

I've done this before too, but I think it's incorrect -- doesn't this mean "The flag needs to contain both OPERATOR _and_ PUNCTUATION"?

---

_@charliermarsh reviewed on 2023-05-17 14:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/logical_lines.rs`:59 on 2023-05-17 14:58_

(Was also missing BRACKET.)

---

_@charliermarsh reviewed on 2023-05-17 14:58_

---

_@charliermarsh reviewed on 2023-05-17 14:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:57 on 2023-05-17 14:58_

I think this was just an oversight without a test-case.

---

_@charliermarsh reviewed on 2023-05-17 14:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace.rs`:69 on 2023-05-17 14:58_

Empirically, pycodestyle seems to allow continuations:

```py
#: Okay
a = (1,\
2)
```

We could deviate and remove this change, I don't feel strongly, but it was necessary to achieve compatibility.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/token_kind.rs`:324 on 2023-05-17 14:59_

pycodestyle doesn't seem to include or enforce this -- we were flagging this as invalid space around the tilde:

```py
spam[ ~ham]
```

---

_@charliermarsh reviewed on 2023-05-17 14:59_

---

_@charliermarsh reviewed on 2023-05-17 14:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/token_kind.rs`:244 on 2023-05-17 14:59_

I think this was just missing (`==`).

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:375 on 2023-05-17 15:01_

Probably the most "controversial" change here -- don't treat the first line here as containing trailing whitespace (between the colon and comment):

```py
if x:  # Foo
  ...
```

Without this change, we flag the following as containing invalid trailing space around the `[`:

```py
#: Okay
x = [  #
    'some value',
]
```

Another option is to change all clients of this method to lookahead and _not_ flag when the next token is a comment.



---

_@charliermarsh reviewed on 2023-05-17 15:01_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/logical_lines.rs`:60 on 2023-05-17 15:29_

Yes it does. What we want here is to use `intersects` (could be faster than using an `||` with two function calls.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:375 on 2023-05-17 15:32_

I think the question is whether this should apply to all rules or not:
* Yes -> having it here is fine
* No -> Handle it inside of the rule.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/token_kind.rs`:324 on 2023-05-17 15:33_

Nit: Can we move this method to the rule that uses it because `TokenKind` is generic and `is_bitwise_or_shift` now no longer returns `true` for all bitwise operators.

---

_@MichaReiser approved on 2023-05-17 15:34_

---

_Merged by @charliermarsh on 2023-05-17 16:51_

---

_Closed by @charliermarsh on 2023-05-17 16:51_

---

_Branch deleted on 2023-05-17 16:51_

---
