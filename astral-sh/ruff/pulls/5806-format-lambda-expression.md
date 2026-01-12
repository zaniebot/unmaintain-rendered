```yaml
number: 5806
title: "Format `lambda` expression"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: expr-lambda
created_at: 2023-07-16T13:22:23Z
updated_at: 2023-07-19T12:09:18Z
url: https://github.com/astral-sh/ruff/pull/5806
synced_at: 2026-01-12T15:55:19Z
```

# Format `lambda` expression

---

_@cnpryer_

## Summary

See #5810.

Implements `ArgumentsParentheses::SkipInsideLambda` for `FormatArguments` to unblock `lambda` formatting.

## Test Plan

Existing snapshots and potentially more fixtures.

______

0.978 -> 0.98 on django similarity index

I wasn't sure if I wanted to claim this one because I was violating the orphan rule workaround by modifying the generated.rs. I noticed (1) if I regenerated them it'd modify the currently checked in generated.rs, so I figured there may have been some manual adjustments already. (2) I saw #5804 modify it manually in the same way. I looked at some of #4951 as well.

TODO:
- [x] Update generated.rs for new `ForamtArguments` `Default`
- [x] Basic implementation
- [x] Review current snapshots
- [x] Add more fixtures 
- [x] Dangling comments

---

_Renamed from "Format `lamdba` expression" to "Format `lambda` expression" by @cnpryer on 2023-07-16 13:22_

---

_Marked ready for review by @cnpryer on 2023-07-16 13:33_

---

_Comment by @github-actions[bot] on 2023-07-16 13:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1867.1±2.62µs     8.9 MB/sec    1.00   1874.5±4.06µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.7±0.64µs    14.2 MB/sec    1.00    208.2±0.21µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.04ms     3.2 MB/sec    1.00     12.9±0.04ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.01      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.0±0.54µs     6.9 MB/sec    1.01    434.7±1.84µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.2 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1396.2±1.83µs    11.9 MB/sec    1.01   1415.5±2.53µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.9±0.51µs    19.0 MB/sec    1.00    155.6±0.28µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.15ms     3.7 MB/sec    1.00     11.1±0.14ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    246.3±5.62µs    12.0 MB/sec    1.02    250.1±9.29µs    11.8 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.09ms     5.3 MB/sec    1.00      4.8±0.06ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.15ms     2.6 MB/sec    1.01     15.6±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    495.9±9.07µs     6.0 MB/sec    1.00    491.2±8.33µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.11ms     3.6 MB/sec    1.01      7.1±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.08ms     5.0 MB/sec    1.00      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1707.1±38.32µs     9.8 MB/sec    1.00  1657.0±18.64µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.1±3.18µs    15.4 MB/sec    1.00    189.4±3.01µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.03ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-07-16 18:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:38 on 2023-07-18 09:19_

Nit (I'm fine with either)

```suggestion
        write!(f, [text("lambda")])?;

        if !args.args.is_empty() {
            write!(
                f,
                [
                    space(),
                    args.format()
                        .with_options(ArgumentsParentheses::SkipInsideLambda),
                ]
            )?;
        }

        write!(f, [text(":"), space(), body.format()])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:24 on 2023-07-18 09:20_

Can we add some more examples where the parenthesized body expands over multiple lines? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 09:25_

Can we add an assertion that `dangling_comments` is empty (is this guaranteed?)

---

_@MichaReiser approved on 2023-07-18 09:25_

---

_Converted to draft by @cnpryer on 2023-07-18 11:09_

---

_@cnpryer reviewed on 2023-07-18 11:21_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:38 on 2023-07-18 11:21_

Yours is better for sure. 7473457ecaae65ac9b645262bb7587269f71a232

---

_@cnpryer reviewed on 2023-07-18 11:22_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:24 on 2023-07-18 11:22_

ed78487efe33648761550d9cf44ab08ee7c915d1

---

_@cnpryer reviewed on 2023-07-18 11:22_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 11:22_

e11be20805a7e6cbd2492b7af2483f317202677b

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 11:22_

> is this guaranteed?

Let me think about this later today.

---

_@cnpryer reviewed on 2023-07-18 11:22_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 11:32_

My initial thought is it should be since afaik `lambda (` is a syntax error.

```
SyntaxError: Unexpected token '('Ruff(E999)
```

But a couple more thoughts:
- I may not be thinking about *assignment* of dangling comments for `lambda`s.
- Maybe I can run this on more code with an assertion.

---

_@cnpryer reviewed on 2023-07-18 11:32_

---

_Comment by @cnpryer on 2023-07-18 11:37_

[See comment](https://github.com/astral-sh/ruff/pull/5806#discussion_r1266630127). In the meantime I'll mark this as ready since the debug assert is a good safeguard IIUC.

---

_Marked ready for review by @cnpryer on 2023-07-18 11:37_

---

_@konstin reviewed on 2023-07-18 16:15_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 16:15_

I managed to construct this example:
```python
a = (
    lambda  # a
           : 1
)
```
This is a pronouncedly strange edge case (it does neither make sense to have a comment there nor to break the empty lambda argument list over two lines), i.e. it doesn't matter where the comment ends up as long as it's stable, but currently we lose it which we shouldn't:
```
{
    Node {
        kind: Arguments,
        range: 10..36,
        source: `lambda  # a⏎`,
    }: {
        "leading": [],
        "dangling": [
            SourceComment {
                text: "# a",
                position: EndOfLine,
                formatted: false,
            },
        ],
        "trailing": [],
    },
}
a = lambda: 1
```

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-18 16:28_

Perfect. Thank you. I'll try to handle this today.

---

_@cnpryer reviewed on 2023-07-18 16:28_

---

_@cnpryer reviewed on 2023-07-19 00:29_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/arguments.rs`:194 on 2023-07-19 00:29_

bacf71356b7c397564b0f1f74e04406e6e034260 (I'll leave this unresolved to defer to a review)

---

_Review requested from @konstin by @cnpryer on 2023-07-19 00:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:51 on 2023-07-19 06:17_

Since the dangling comments are now handled here, it is safe to override `fmt_dangling_comments` with a function that just returns `Ok(())`. 

Unrelated to this PR: What would you think (and @konstin): Should we make `fmt_dangling_comments` always return `Ok(())` and instead have the default implementation assert that are dangling comments were formatted in debug builds? The downside is that we could potentially loose dangling comments in production because of it. But it seems error-prone having to remember that it is now safe override the function.

---

_@MichaReiser approved on 2023-07-19 06:17_

Looks good to me. Let's override `fmt_dangling_comments` and we're then good to merge

---

_@konstin reviewed on 2023-07-19 09:28_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:51 on 2023-07-19 09:28_

yes, i think we should replace the current `fmt_dangling_comments`, maybe even keep a big match which nodes are allowed to have dangling comment and which should error when they have

---

_Comment by @cnpryer on 2023-07-19 10:37_

Dang! So close

---

_@cnpryer reviewed on 2023-07-19 11:25_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:51 on 2023-07-19 11:25_

> What would you think

I think if I gave an opinion at this time it wouldn't be informed enough. I'll write this down so that later, if I can add to the conversation, I'll let you know if I have any other thoughts.

69384521d025f595e5eb95755d8993c8f5134299

---

_@cnpryer reviewed on 2023-07-19 11:40_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:57 on 2023-07-19 11:40_

```suggestion
        // Override. Dangling comments are handled in `fmt_fields`.
```
Can I get this even with the auto-merge?

---

_Merged by @MichaReiser on 2023-07-19 11:47_

---

_Closed by @MichaReiser on 2023-07-19 11:47_

---

_Branch deleted on 2023-07-19 11:49_

---
