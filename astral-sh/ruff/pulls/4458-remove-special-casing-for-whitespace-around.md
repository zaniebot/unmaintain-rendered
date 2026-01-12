```yaml
number: 4458
title: "Remove special-casing for whitespace-around-@"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unary
created_at: 2023-05-16T20:28:21Z
updated_at: 2023-05-17T15:53:56Z
url: https://github.com/astral-sh/ruff/pull/4458
synced_at: 2026-01-12T15:55:15Z
```

# Remove special-casing for whitespace-around-@

---

_@charliermarsh_

## Summary

Follow-up to #4454. @MichaReiser pointed out that we seemed to be missing the whitespace-optional handling from pycodestyle. I started to implement that, but then realized that... I think our logic is correct? With the exception that pycodestyle doesn't let you flag missing whitespace for the first token in a logical line (since they require `need_space` to be set, which is initialized to `False`), which effectively solves the case in which a decorator is used.

It's a little hard to reason about what pycodestyle is doing (I think Micha's code is clearer), but I don't really see how the whitespace is "optional" in the arithmetic cases (e.g., it triggers for `1+2`).


---

_@charliermarsh reviewed on 2023-05-16 20:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:74 on 2023-05-16 20:28_

I put this whole thing in an `if-let` which causes an enormous diff. It felt cleaner than if `if-else`.

---

_@charliermarsh reviewed on 2023-05-16 20:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:90 on 2023-05-16 20:28_

Removed `TokenKind::At`.

---

_Comment by @github-actions[bot] on 2023-05-16 20:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.9±0.53ms  1989.3 KB/sec    1.00     20.8±0.57ms  2004.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.9±0.16ms     3.4 MB/sec    1.00      4.8±0.16ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   603.5±25.10µs     4.9 MB/sec    1.01   607.4±31.62µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.5±0.31ms     3.0 MB/sec    1.00      8.3±0.31ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.04      9.9±0.31ms     4.1 MB/sec    1.00      9.6±0.29ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.09ms     7.8 MB/sec    1.00      2.1±0.13ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.5±12.51µs    12.2 MB/sec    1.00   242.6±14.20µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.4±0.17ms     5.8 MB/sec    1.00      4.3±0.19ms     5.9 MB/sec
parser/large/dataset.py                    1.00      7.7±0.17ms     5.3 MB/sec    1.00      7.7±0.23ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1506.6±57.13µs    11.1 MB/sec    1.01  1520.7±56.61µs    10.9 MB/sec
parser/numpy/globals.py                    1.00    152.5±7.48µs    19.4 MB/sec    1.01    154.6±7.46µs    19.1 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.10ms     7.8 MB/sec    1.02      3.4±0.13ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.24ms     2.4 MB/sec    1.00     16.8±0.25ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.7±7.66µs     5.9 MB/sec    1.01    503.2±6.02µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.7 MB/sec    1.01      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.01      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1720.6±16.06µs     9.7 MB/sec    1.01  1743.2±15.66µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.8±5.16µs    15.1 MB/sec    1.04    202.8±6.07µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.04      3.8±0.33ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.9±0.04ms     5.9 MB/sec    1.05      7.2±0.05ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1306.6±16.36µs    12.7 MB/sec    1.04  1355.2±14.10µs    12.3 MB/sec
parser/numpy/globals.py                    1.00    134.4±2.66µs    22.0 MB/sec    1.02    137.2±2.25µs    21.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.8 MB/sec    1.05      3.0±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@youknowone reviewed on 2023-05-16 21:35_

---

_Review comment by @youknowone on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:74 on 2023-05-16 21:35_

URL for review https://github.com/charliermarsh/ruff/pull/4458/files?w=1

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:74 on 2023-05-17 06:00_

Thanks for looking into this again and tracking it down. Right, I removed the condition because it was unclear to me why it is needed and no test started failing after removing it

I think we can move this logic out of the hot loop, helping us to reduce the overall branching:

```rust
let mut tokens = line.tokens().iter().peekable();
let first_token = tokens.by_ref().find_map(|t| {
	let kind = token.kind();
	!kind.is_trivia()).then_some(kind)
});
let Some(mut prev_token) = first_token else { return; }
let mut parens = if matches!(prev_token, TokenKind::Lpar | TokenKind::Lambda) { 1 } else { 0 };

while let Some(token) = tokens.next() {
	...
}
```

But it may also not be worth it, I would assume that the branch predictor is able to predict the right branch in most cases. 

---

_@MichaReiser approved on 2023-05-17 06:00_

---

_Merged by @charliermarsh on 2023-05-17 15:32_

---

_Closed by @charliermarsh on 2023-05-17 15:32_

---

_Branch deleted on 2023-05-17 15:32_

---
