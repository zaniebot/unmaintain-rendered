```yaml
number: 4454
title: Avoid flagging missing whitespace for decorators
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/dec
created_at: 2023-05-16T16:46:50Z
updated_at: 2023-05-16T17:39:32Z
url: https://github.com/astral-sh/ruff/pull/4454
synced_at: 2026-01-12T03:50:03Z
```

# Avoid flagging missing whitespace for decorators

---

_Pull request opened by @charliermarsh on 2023-05-16 16:46_

_No description provided._

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/token_kind.rs`:177 on 2023-05-16 16:50_

I think it would be good to instead create a new function or move this method to a function because the name unary is now misleading (@ isn't an unary operator)

---

_@MichaReiser reviewed on 2023-05-16 16:51_

---

_@MichaReiser reviewed on 2023-05-16 16:52_

This is a deviation from pycodestyle, right?

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/token_kind.rs`:177 on 2023-05-16 16:52_

Neither is `TokenKind::Star` in this context, no? E.g., is `*` a unary operator in `foo(*a)`?

---

_@charliermarsh reviewed on 2023-05-16 16:52_

---

_Comment by @charliermarsh on 2023-05-16 16:52_

This is a fix that corrects a deviation from pycodestyle. It brings our behavior in-line with pycodestyle.

---

_@MichaReiser reviewed on 2023-05-16 16:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/token_kind.rs`:177 on 2023-05-16 16:52_

That's correct. The blame is on me 🙈

---

_@charliermarsh reviewed on 2023-05-16 16:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/token_kind.rs`:177 on 2023-05-16 16:53_

Though I guess `**` is handled as a special-case in the rule (`if kind.is_unary() || kind == TokenKind::DoubleStar`) -- I'll just do that too for `@`.

---

_Comment by @charliermarsh on 2023-05-16 16:54_

(Sorry for the wordy answer, I can't tell if you're asking whether the old or new behavior is the deviation.)

---

_@charliermarsh reviewed on 2023-05-16 16:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/token_kind.rs`:177 on 2023-05-16 16:55_

I removed star too .

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 16:55_

 An you explain the fix. It's unclear to me how it works. I would have expected that the @ always goes into the else branch that requires no space, and thus, doesn't create any diagnostics 

---

_@MichaReiser reviewed on 2023-05-16 16:56_

---

_@charliermarsh reviewed on 2023-05-16 16:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 16:59_

`@` can be used as a binary operator in Python -- it represents matrix multiplication:

```
#: E225
i = 1 @2
#: E225
i = 1@ 2
```

So equivalently to `*` or `+`, we need to detect whether we're seeing a decorator, or a binary operator.

---

_@charliermarsh reviewed on 2023-05-16 17:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 17:00_

I think in practice, for this particular piece of syntax, it will be a "unary" token IFF it's the first token in the logical line.

---

_@MichaReiser reviewed on 2023-05-16 17:05_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 17:05_

Oh wow, I didn't know! 

Hmm, I'm wondering right now if my refactored implementation misses the handling for `WS_OPTIONAL`. Because the `@` is part of the `is_whitespace` required, which returns `NeedsSpace::Yes` rather than setting it to `NeedsSpace::Optional`

https://github.com/PyCQA/pycodestyle/blob/f6500e2170406b664a721b5e3a0f5d66bcb757e1/pycodestyle.py#L108

---

_Comment by @github-actions[bot] on 2023-05-16 17:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.18ms     2.4 MB/sec    1.00     16.6±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.01      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.0±4.44µs     5.8 MB/sec    1.00    506.7±3.88µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.01      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.07ms     5.1 MB/sec    1.00      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.9±20.76µs     9.6 MB/sec    1.01  1738.3±16.46µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.9±2.70µs    15.3 MB/sec    1.02    195.8±2.63µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.2 MB/sec    1.02      3.6±0.03ms     7.1 MB/sec
parser/large/dataset.py                    1.02      6.4±0.05ms     6.4 MB/sec    1.00      6.3±0.05ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.01  1247.3±14.00µs    13.4 MB/sec    1.00  1233.7±14.80µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    127.2±1.95µs    23.2 MB/sec    1.00    126.7±1.72µs    23.3 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.03ms     9.4 MB/sec    1.00      2.7±0.03ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.8±0.34ms     2.2 MB/sec    1.00     18.7±0.35ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.7±0.13ms     3.6 MB/sec    1.00      4.5±0.12ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   534.6±10.92µs     5.5 MB/sec    1.00   526.8±11.08µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.26ms     3.3 MB/sec    1.00      7.8±0.13ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.27ms     4.6 MB/sec    1.00      8.9±0.13ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1830.3±40.73µs     9.1 MB/sec    1.01  1840.5±24.71µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±6.52µs    14.4 MB/sec    1.01    206.1±6.98µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.06ms     6.6 MB/sec    1.00      3.9±0.06ms     6.6 MB/sec
parser/large/dataset.py                    1.02      7.0±0.10ms     5.8 MB/sec    1.00      6.9±0.10ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.02  1330.2±31.84µs    12.5 MB/sec    1.00  1301.7±28.86µs    12.8 MB/sec
parser/numpy/globals.py                    1.03    135.7±3.54µs    21.7 MB/sec    1.00    132.4±2.30µs    22.3 MB/sec
parser/pydantic/types.py                   1.02      3.0±0.04ms     8.6 MB/sec    1.00      2.9±0.04ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-16 17:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 17:14_

Hmm, I will take a look in a quick follow-up PR (or you can, if you're already working on it?).

---

_Merged by @charliermarsh on 2023-05-16 17:15_

---

_Closed by @charliermarsh on 2023-05-16 17:15_

---

_Branch deleted on 2023-05-16 17:15_

---

_@MichaReiser reviewed on 2023-05-16 17:19_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 17:19_

I'm not and would appreciate it if you can have a look. I believe we're missing the whole optional branch because `pycodestyle` has no explicit handling of the `@` in the `unary` branch.

---

_@MichaReiser reviewed on 2023-05-16 17:39_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:96 on 2023-05-16 17:39_

Okay, I now remember briefly what is happening. The arithmetic and bitwise aren't optional. It's just that they are different rules (handled here https://github.com/PyCQA/pycodestyle/blob/f6500e2170406b664a721b5e3a0f5d66bcb757e1/pycodestyle.py#LL914C26-L920C66)

What I don't understand right now is why `@` would require any special treatment (more specifically, why we need where pycodestyle has none)

---
