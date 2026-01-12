```yaml
number: 5705
title: Fix format named expr comments
type: pull_request
state: closed
author: davidszotten
labels:
  - formatter
assignees: []
draft: true
base: main
head: fix-format-named-expr-comments
created_at: 2023-07-12T08:50:44Z
updated_at: 2023-08-23T23:16:59Z
url: https://github.com/astral-sh/ruff/pull/5705
synced_at: 2026-01-12T02:45:38Z
```

# Fix format named expr comments

---

_Pull request opened by @davidszotten on 2023-07-12 08:50_

fixes #5695

---

_@davidszotten reviewed on 2023-07-12 08:53_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:29 on 2023-07-12 08:53_

i tried making a single `write!` call with these `if`s inside, but that doesn't work since `space()` and `soft_line_break` have different types. is there some trick to make this work? (or do we prefer multiple `write` calls?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:29 on 2023-07-12 08:58_

There's `format_with` but personally i like this style better, multiple write calls are fine here, if required we can still optimize later

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:32 on 2023-07-12 08:59_

What's the reason for this being a soft line break?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:38 on 2023-07-12 09:00_

What happens when there are dangling_item_comments but no leading leading_value_comments? this would make a good test case

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/trivia.rs`:189 on 2023-07-12 09:01_

[walrus is correct](https://docs.python.org/3/whatsnew/3.8.html#assignment-expressions)

---

_@konstin approved on 2023-07-12 09:02_

---

_Review requested from @MichaReiser by @konstin on 2023-07-12 09:02_

---

_Comment by @github-actions[bot] on 2023-07-12 09:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.01ms     4.9 MB/sec    1.01      8.5±0.01ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1899.9±2.13µs     8.8 MB/sec    1.01   1917.8±4.14µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    204.7±0.37µs    14.4 MB/sec    1.01    207.1±0.27µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.01ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.00     14.1±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.2±1.05µs     8.1 MB/sec    1.00    364.0±1.13µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1465.7±2.64µs    11.4 MB/sec    1.00   1452.3±2.85µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.4±0.23µs    18.9 MB/sec    1.00    155.4±0.66µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.00ms     8.0 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.45ms     3.7 MB/sec    1.10     12.2±0.67ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.13ms     6.8 MB/sec    1.09      2.7±0.13ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   289.3±16.98µs    10.2 MB/sec    1.01   291.9±24.53µs    10.1 MB/sec
formatter/pydantic/types.py                1.00      5.7±0.28ms     4.5 MB/sec    1.01      5.8±0.30ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.04     18.1±1.51ms     2.2 MB/sec    1.00     17.4±0.44ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.39ms     3.7 MB/sec    1.04      4.7±0.26ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.4±19.13µs     5.8 MB/sec    1.17   596.6±40.30µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.36ms     3.4 MB/sec    1.10      8.3±0.33ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.74ms     4.4 MB/sec    1.03      9.5±0.35ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1969.2±76.88µs     8.5 MB/sec    1.00  1932.5±84.95µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   233.4±14.19µs    12.6 MB/sec    1.00   230.1±13.11µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.13ms     6.4 MB/sec    1.01      4.0±0.19ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@davidszotten reviewed on 2023-07-12 09:15_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:32 on 2023-07-12 09:15_

as opposed to?

---

_@davidszotten reviewed on 2023-07-12 09:21_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:38 on 2023-07-12 09:21_

good catch. definitely something not what i expected. will continue investigating

---

_@konstin reviewed on 2023-07-12 10:05_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_named_expr.rs`:32 on 2023-07-12 10:05_

a `hard_line_break`, since as far as i can tell we always want to break after the comments if there are any. I realized it  doesn't make a difference here because a trailing comment will expand the group, but it might be easier to read

---

_@MichaReiser reviewed on 2023-07-12 11:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:13 on 2023-07-12 11:42_

Hey. Just wanted to let you know that I'm working on changing `NeedsParentheses` to pass through the parent node. This should make it straightforward to implement the correct parentheses logic for the walrus operator. 

---

_@davidszotten reviewed on 2023-07-12 13:32_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:13 on 2023-07-12 13:32_

👍 

atm i can't even figure out what's causing these parens to be added.  parenthesize gets set to "ifbreaks" and nothing is breaking...

---

_@MichaReiser reviewed on 2023-07-12 13:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:13 on 2023-07-12 13:55_

This PR ships the infrastructure: https://github.com/astral-sh/ruff/pull/5708/

The parentheses are added because `FormatExpr` first calls into `NeedsParentheses` to determine if parentheses are necessary

https://github.com/astral-sh/ruff/blob/33a91773f7a8c60c2f1cdb84127feabad8c6d84f/crates/ruff_python_formatter/src/expression/mod.rs#L65

And the implementation of `NamedExpr` returns `Always`

https://github.com/astral-sh/ruff/blob/33a91773f7a8c60c2f1cdb84127feabad8c6d84f/crates/ruff_python_formatter/src/expression/expr_named_expr.rs#L39-L44

We should implement the same rules as in https://github.com/psf/black/blob/d1248ca9beaf0ba526d265f4108836d89cf551b7/src/black/linegen.py#L1381-L1395 (requires access to the parent node)

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:13 on 2023-07-12 14:06_

aah. thanks

do i understand correctly that his is separate to (i.e. won't help with) parens for generator comprehensions (that we were discussing yesterday)?

---

_@davidszotten reviewed on 2023-07-12 14:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/named_expr.py`:13 on 2023-07-12 14:12_

That's correct. The problem is related but needs a separate fix (and I first need to figure out what even the *expected* output is... I have suspicion I know how it's supposed to behave but I haven't verified it yet)

---

_@MichaReiser reviewed on 2023-07-12 14:12_

---

_Label `formatter` added by @konstin on 2023-07-16 18:19_

---

_Comment by @charliermarsh on 2023-08-23 22:36_

I suspect this can now be closed as the originating issue is resolved.

---

_Closed by @MichaReiser on 2023-08-23 23:16_

---
