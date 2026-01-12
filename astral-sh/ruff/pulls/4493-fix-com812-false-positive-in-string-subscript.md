```yaml
number: 4493
title: Fix COM812 false positive in string subscript
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-com812-false-positive
created_at: 2023-05-18T14:00:15Z
updated_at: 2023-10-17T11:07:23Z
url: https://github.com/astral-sh/ruff/pull/4493
synced_at: 2026-01-12T02:32:41Z
```

# Fix COM812 false positive in string subscript

---

_Pull request opened by @JonathanPlasse on 2023-05-18 14:00_

- Close #4480

---

_Comment by @github-actions[bot] on 2023-05-18 14:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     15.2±0.03ms     2.7 MB/sec    1.00     14.8±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.00ms     4.5 MB/sec    1.00      3.7±0.02ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01    381.9±1.67µs     7.7 MB/sec    1.00    376.6±1.32µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.04      7.7±0.01ms     5.3 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1599.3±2.67µs    10.4 MB/sec    1.00   1541.8±2.08µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    172.4±0.35µs    17.1 MB/sec    1.00    167.6±0.41µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.07ms     7.5 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.03      6.1±0.01ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1156.0±1.72µs    14.4 MB/sec    1.02   1181.1±2.21µs    14.1 MB/sec
parser/numpy/globals.py                    1.00    120.8±0.38µs    24.4 MB/sec    1.01    122.3±0.87µs    24.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.01      2.5±0.01ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.27ms     2.4 MB/sec    1.00     17.0±0.27ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.9 MB/sec    1.01      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.4±9.66µs     5.9 MB/sec    1.01    506.0±6.83µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.01      7.1±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.4±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1752.9±23.83µs     9.5 MB/sec    1.00  1757.6±17.70µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.6±4.89µs    14.9 MB/sec    1.03    202.7±5.55µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.01      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.00      6.7±0.06ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1277.4±16.37µs    13.0 MB/sec    1.01  1288.4±17.66µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    132.5±3.82µs    22.3 MB/sec    1.00    132.0±2.00µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.01      2.9±0.06ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_commas/rules.rs`:58 on 2023-05-18 14:17_

We may want to introduce a new token type for this. Looking further down, it looks like we'll now treat `"foo(" as the start of a function call? Which, is maybe fine, since that's invalid anyway, but seems conceptually wrong.

```rust
// Update the comma context stack.
match token.type_ {
    TokenType::OpeningBracket => match (prev.type_, prev_prev.type_) {
        (TokenType::Named, TokenType::Def) => {
            stack.push(Context::new(ContextType::FunctionParameters));
        }
        (TokenType::Named | TokenType::ClosingBracket, _) => {
            stack.push(Context::new(ContextType::CallArguments));
        }
```

---

_@charliermarsh reviewed on 2023-05-18 14:17_

---

_@charliermarsh reviewed on 2023-05-18 14:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_commas/rules.rs`:58 on 2023-05-18 14:30_

Thank you 🙏 

---

_Merged by @charliermarsh on 2023-05-18 14:35_

---

_Closed by @charliermarsh on 2023-05-18 14:35_

---

_Branch deleted on 2023-10-17 11:07_

---
