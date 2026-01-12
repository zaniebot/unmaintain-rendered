```yaml
number: 6563
title: "Add support for multi-character operator tokens to `SimpleTokenizer`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tokenizer
created_at: 2023-08-14T16:14:24Z
updated_at: 2023-08-16T13:09:20Z
url: https://github.com/astral-sh/ruff/pull/6563
synced_at: 2026-01-12T15:55:21Z
```

# Add support for multi-character operator tokens to `SimpleTokenizer`

---

_@charliermarsh_

## Summary

Allows for proper lexing of tokens like `->`.

The main challenge is to ensure that our forward and backwards representations are the same for cases like `===`. Specifically, we want that to lex as `==` followed by `=` regardless of whether it's a forwards or backwards lex. To do so, we identify the range of the sequential characters (the full span of `===`), lex it forwards, then return the last token.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-14 16:14_

---

_@charliermarsh reviewed on 2023-08-14 16:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/tokenizer.rs`:559 on 2023-08-14 16:14_

These are copied over from the "full" lexer.

---

_Review request for @MichaReiser removed by @charliermarsh on 2023-08-14 16:27_

---

_Converted to draft by @charliermarsh on 2023-08-14 16:27_

---

_Comment by @charliermarsh on 2023-08-14 16:27_

Need to look into formatter failures.

---

_Comment by @github-actions[bot] on 2023-08-14 16:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.2±0.18ms     9.6 MB/sec     1.01      4.3±0.30ms     9.5 MB/sec
formatter/numpy/ctypeslib.py               1.05   859.1±33.90µs    19.4 MB/sec     1.00   814.5±41.11µs    20.4 MB/sec
formatter/numpy/globals.py                 1.06     88.6±4.94µs    33.3 MB/sec     1.00    83.6±11.12µs    35.3 MB/sec
formatter/pydantic/types.py                1.05  1638.0±104.77µs    15.6 MB/sec    1.00  1556.0±50.58µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     11.8±0.43ms     3.5 MB/sec     1.06     12.4±0.42ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.1±0.10ms     5.3 MB/sec     1.05      3.3±0.14ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   455.8±19.80µs     6.5 MB/sec     1.00   451.4±18.88µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.17ms     4.2 MB/sec     1.02      6.2±0.26ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.04      6.3±0.32ms     6.5 MB/sec     1.00      6.0±0.14ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1377.6±75.83µs    12.1 MB/sec     1.04  1427.6±55.64µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    174.8±7.33µs    16.9 MB/sec     1.00    170.8±9.14µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.14ms     9.3 MB/sec     1.03      2.8±0.14ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.07ms     9.4 MB/sec    1.04      4.5±0.09ms     9.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   848.0±22.77µs    19.6 MB/sec    1.01   860.4±13.92µs    19.4 MB/sec
formatter/numpy/globals.py                 1.00     85.7±1.89µs    34.4 MB/sec    1.02     87.4±2.45µs    33.8 MB/sec
formatter/pydantic/types.py                1.00  1692.7±29.28µs    15.1 MB/sec    1.03  1739.6±58.66µs    14.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.17ms     3.2 MB/sec    1.00     12.6±0.14ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.4±5.98µs     6.7 MB/sec    1.00    438.5±4.26µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.09ms     3.9 MB/sec    1.00      6.5±0.08ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.09ms     5.8 MB/sec    1.00      6.9±0.08ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1478.0±12.91µs    11.3 MB/sec    1.00  1482.7±18.35µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.8±2.63µs    16.9 MB/sec    1.01    175.8±2.23µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.08ms     8.1 MB/sec    1.00      3.1±0.03ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-08-14 16:57_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-14 16:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:720 on 2023-08-15 07:22_

Identifiers are the most common tokens. It can make sense to have an extra branch at the top that handles identifiers (similar to lexer)

```
c if is_ascii_identifier_start(c) => self.lex_identifier(c)?,
```

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:873 on 2023-08-15 07:24_

Naming this `cursor` is confusing because it is the lexer. Rename to `forward_lexer`?

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:132 on 2023-08-15 07:26_

These (and some others like `:`, `!`) are never ambiguous. We can return the first token right away when backward lexing.

---

_@MichaReiser requested changes on 2023-08-15 07:28_

What's the motivation for supporting all tokens? The very intention of the `SimpleTokenizer` was to **not** support everything. 

We should avoid the backward, forward lexing for tokens that do not require it.



---

_Comment by @charliermarsh on 2023-08-15 12:51_

The motivation is unchanged from https://github.com/astral-sh/ruff/pull/6518: the `SimpleTokenizer` should be capable of lexing all contents _between_ any two AST nodes, and so it should support any tokens that can appear between nodes. This PR is a reaction to your suggestion to lex `->` as a single node (https://github.com/astral-sh/ruff/pull/6518#pullrequestreview-1576202408), and it felt strange to me to special-case `->` when that thinking applies equally to `**`, `!=`, etc., so I included all of those tokens here.

How do you want to proceed? I don't feel strongly about merging, it doesn't enable any new use-cases since we already support those tokens (they're just lexed as individual characters, not composite tokens). This does lead to some oddities, e.g., it's a bit odd that we use the `SimpleTokenizer` to identify `**` by looking for two `*` tokens in the formatter -- but again, this doesn't enable us to lex anything we couldn't lex before.

---

_Comment by @MichaReiser on 2023-08-15 13:14_

> The motivation is unchanged from #6518: the `SimpleTokenizer` should be capable of lexing all contents _between_ any two AST nodes, and so it should support any tokens that can appear between nodes. This PR is a reaction to your suggestion to lex `->` as a single node ([#6518 (review)](https://github.com/astral-sh/ruff/pull/6518#pullrequestreview-1576202408)), and it felt strange to me to special-case `->` when that thinking applies equally to `**`, `!=`, etc., so I included all of those tokens here.
> 
> How do you want to proceed? I don't feel strongly about merging, it doesn't enable any new use-cases since we already support those tokens (they're just lexed as individual characters, not composite tokens). This does lead to some oddities, e.g., it's a bit odd that we use the `SimpleTokenizer` to identify `**` by looking for two `*` tokens in the formatter -- but again, this doesn't enable us to lex anything we couldn't lex before.

Thanks for the explanation and sorry, I forgot about that. That makes sense. It means we need to be able to lex all operators. 

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 03:18_

---

_Comment by @charliermarsh on 2023-08-16 04:09_

No prob! Updated.

---

_@MichaReiser approved on 2023-08-16 06:34_

---

_Merged by @charliermarsh on 2023-08-16 13:09_

---

_Closed by @charliermarsh on 2023-08-16 13:09_

---

_Branch deleted on 2023-08-16 13:09_

---
