```yaml
number: 4487
title: Invert quote-style when generating code within f-strings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f-string-quote
created_at: 2023-05-18T02:47:39Z
updated_at: 2023-05-19T07:26:50Z
url: https://github.com/astral-sh/ruff/pull/4487
synced_at: 2026-01-12T15:55:15Z
```

# Invert quote-style when generating code within f-strings

---

_@charliermarsh_

## Summary

When we generate source code from an AST (via `Generator`), we have a preferred quote style, based on the inferred quote style in the file. Unfortunately, when we try to generate autofixes within f-strings, the `Generator` is unaware of the fact that we're "inside" an f-string, and so continues to use that preferred quote style, which often leads to syntax errors.

This PR modifies our logic to detect when we're inside a single-quoted f-string, and then pass in the "inverted" quote style based on whatever was used to quote that f-string. This in turn requires that we track all f-strings, and are able to map from the current expression to the surrounding f-strings.

Two considerations here:

1. The quote style that we provide to `Generator` is really a "preferred" quote style. The `Generator` will "ignore" that preference if, e.g., it lets us avoid escaping a quote within a string, similar to Black's preferences. This would be problematic within f-strings. However, f-strings can't contain backslashes (it's a syntax error?), so... it might be fine?
2. This doesn't really deal with _nested_ f-strings. I'm not sure what we want to do in those cases. The grammar is so limiting and complicated. We may want to avoid fixing issues within nested f-strings altogether?

Closes #4322.

Closes #4324.

Closes #4323.

Closes #4321.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-18 02:47_

---

_Review requested from @konstin by @charliermarsh on 2023-05-18 02:47_

---

_@charliermarsh reviewed on 2023-05-18 02:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:140 on 2023-05-18 02:48_

I don't know where this belongs -- it requires the `Context`, `Indexer`, `Locator`, and `Stylist`, so right now it lives on `Checker`...

---

_@charliermarsh reviewed on 2023-05-18 02:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:65 on 2023-05-18 02:48_

These are all mechanical.

---

_@charliermarsh reviewed on 2023-05-18 02:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:81 on 2023-05-18 02:48_

We now track f-string ranges, which lets us do a binary search to find the containing f-string whenever we need to fix within an f-string.

---

_Comment by @charliermarsh on 2023-05-18 02:49_

As part of this effort... we may want to consider `Generator` to be deprecated, or an anti-pattern. In addition to this problem (it's context-independent), it also removes trivia, like comments. It is very useful, and we don't have a great replacement for it, but...

---

_Comment by @github-actions[bot] on 2023-05-18 02:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.13ms     2.9 MB/sec    1.00     14.2±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.9±0.94µs     6.9 MB/sec    1.00    426.7±1.27µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.03ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1461.2±4.43µs    11.4 MB/sec    1.00   1460.5±3.69µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.5±0.29µs    17.9 MB/sec    1.00    164.6±1.07µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.04ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.6 MB/sec    1.01      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.8±1.00µs    15.8 MB/sec    1.01   1062.5±7.26µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.3±0.51µs    27.2 MB/sec    1.00    108.8±0.33µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.01      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.10ms     2.5 MB/sec    1.00     16.3±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.4±5.36µs     6.0 MB/sec    1.00    496.7±5.31µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.16ms     5.0 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1705.3±16.01µs     9.8 MB/sec    1.02  1732.8±13.39µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.9±2.44µs    15.1 MB/sec    1.02    199.2±4.24µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.01      3.7±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.00      7.2±0.08ms     5.7 MB/sec    1.00      7.1±0.04ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1345.3±13.25µs    12.4 MB/sec    1.00  1341.8±16.59µs    12.4 MB/sec
parser/numpy/globals.py                    1.00    137.0±3.22µs    21.5 MB/sec    1.00    137.0±2.34µs    21.5 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.03ms     8.5 MB/sec    1.00      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-05-18 12:16_

> This PR modifies our logic by "inverting" the preferred quote style whenever we're inside a single-quoted f-string. This in turn requires that we track all f-strings, and are able to map from the current expression to the surrounding f-strings.

My understanding is that the preferred quote style only reflects the most used quote sign. This does not guarantee that the source code contains no fstrings with the *not preferred* fstring. 

Edit: Your implementation handles this correctly, I think.

> This doesn't really deal with nested f-strings. I'm not sure what we want to do in those cases. The grammar is so limiting and complicated. We may want to avoid fixing issues within nested f-strings altogether?

An alternative would be to make this a `manual` fix. This won't be very helpful now because our default text renderer doesn't show fixes (we should improve our diagnostics). 

How difficult do you think would it be to add source map support to the `Generator`? We could then generate the code for the enclosing expression or statement and then extract the fstring range. They way this could work is that the generator writes a source map entry at the start and end of every node, mapping the node's start/end offset to the offset in the generated source (similar to the Printer in the formatter)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:163 on 2023-05-18 12:18_

Nit: I think it would be good to move this logic to the `Indexer`. It makes very explicit assumption about `string_ranges` and it may not be obvious to someone working on `Indexer` that some code is making these assumptions

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:118 on 2023-05-18 12:19_

I would prefer to avoid exposing too much of the assumption on how the ranges is stored and instead provide semantic methods to find an fstring, given a range.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:80 on 2023-05-18 12:20_

Can an fstring be triple quoted? If not, use an `else if` or use two separate `match` arms, one for triple-quoted strings and another for fstrings.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/helpers.rs`:24 on 2023-05-18 12:22_

For another PR: Should we move these methods to `Generator` instead, now that they always require a generator.

```
generator.unparse_expr(expr) 
// or

generator.generate_expr(expr)
```

---

_@MichaReiser approved on 2023-05-18 12:23_

---

_Comment by @charliermarsh on 2023-05-18 13:03_

> Edit: Your implementation handles this correctly, I think.

👍 Yeah, sorry, the implementation does do this correctly, but I'll improve the summary.

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:80 on 2023-05-18 13:05_

It can! We actually don't need to use those anywhere since we only care about single-quoted f-strings... But it seems strange to omit them.

---

_@charliermarsh reviewed on 2023-05-18 13:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/indexer.rs`:118 on 2023-05-18 13:05_

Good idea.

---

_@charliermarsh reviewed on 2023-05-18 13:05_

---

_@charliermarsh reviewed on 2023-05-18 13:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:163 on 2023-05-18 13:05_

Good idea.

---

_Comment by @charliermarsh on 2023-05-18 13:06_

> How difficult do you think would it be to add source map support to the Generator? We could then generate the code for the enclosing expression or statement and then extract the fstring range. They way this could work is that the generator writes a source map entry at the start and end of every node, mapping the node's start/end offset to the offset in the generated source (similar to the Printer in the formatter)

Can you elaborate on this a bit more? What does this buy us? (Genuinely asking, not objecting.)

---

_Merged by @charliermarsh on 2023-05-18 14:33_

---

_Closed by @charliermarsh on 2023-05-18 14:33_

---

_Branch deleted on 2023-05-18 14:33_

---

_@konstin reviewed on 2023-05-19 07:25_

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP018.py.snap`:139 on 2023-05-19 07:25_

that suggestion looks wrong

---

_Comment by @MichaReiser on 2023-05-19 07:26_

> Can you elaborate on this a bit more? What does this buy us? (Genuinely asking, not objecting.)

I wrote this before I understood that your fix solves the problem. The main benefit would be that it would make the Generator context aware. You won't need any special handling because you generate on a statement level. 

---
