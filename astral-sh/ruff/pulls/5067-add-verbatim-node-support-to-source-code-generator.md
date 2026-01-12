```yaml
number: 5067
title: "Add \"verbatim\" node support to source code generator"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/verbatim
created_at: 2023-06-13T22:50:03Z
updated_at: 2023-07-14T03:07:34Z
url: https://github.com/astral-sh/ruff/pull/5067
synced_at: 2026-01-12T03:30:21Z
```

# Add "verbatim" node support to source code generator

---

_Pull request opened by @charliermarsh on 2023-06-13 22:50_

## Summary

One of our primary autofix mechanisms is the `Generator`, which takes an AST, and "unparses" it to generate source code.

This is a convenient mechanism, because it allows for a structured editing approach: rather than manipulating text directly, you can just modify the AST in a type-safe way, and rely on the `Generator` to generate the approach code to mimic your edit.

For example, if you want to change `x is True` to `x == True`, you can just create a new `Expr::Compare` node and change the operator type, rather than lexing to find `is`, etc.

The main downside of this approach -- and it's a big one -- is that it doesn't preserve formatting or trivia. That is, it doesn't preserve anything that isn't captured by the AST.

One common example here is raw strings:

```py
not (re.search(r"^.:\\Users\\[^\\]*\\Downloads\\.*") is None)
```

Raw strings are not part of the AST. Instead, the AST stores the parsed string as a constant. So when we "unparse" this, we remove the `r` prefix, and add a ton of backslashes to the string -- all just to change `is` to `==`.

This PR proposes an improvement to the source code generator to support "verbatim" nodes, which should be formatted by reading their existing representations from source. In the above example, we can model the fix is: `${verbatim left expression} == ${verbatim right expression}`. Rather than generating source code "all the way down", we just generate the parts we need, and then insert source code verbatim for existing expressions that we're manipulating. Thus, we'd preserve the raw string when formatting the left node.

Though we'll still lose comments in some cases (if they exist between expressions), this does seem like a strict improvement over our current system. The main reasons _not_ to pursue it are:

- In order to add a new `ExprVerbatim` node to the AST (specifically for the `Generator`), we have to create an entire parallel struct hierarchy (see `verbatim.rs`, similar to our existing `comparable.rs`). It's just a lot of code, though it's all boilerplate and perhaps could even be generated some day.
- We have a lot of usages of `Generator` today, and we need to go through and migrate them all to this new API. Probably not too bad, but it will take time.
- It still doesn't guarantee that we'll preserve all trivia.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-13 22:50_

---

_Review requested from @konstin by @charliermarsh on 2023-06-13 22:50_

---

_@charliermarsh reviewed on 2023-06-13 22:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/verbatim_generator.rs`:1 on 2023-06-13 22:51_

This is an exact copy of `generator.rs`, but with the `ExprVerbatim` case added, and it uses the AST from `verbatim` instead of the RustPython AST.

If we like this approach, then the plan would be to remove `Generator` and migrate all usages to this instead. This would then be the new `Generator` (we wouldn't need the "verbatim" prefix).

---

_@charliermarsh reviewed on 2023-06-13 22:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/verbatim.rs`:3 on 2023-06-13 22:52_

This is basically copied from `comparable.rs` where we have a similar hierarchy but for creating hashable AST nodes. The difference is that this adds `ExprVerbatim` and `StmtVerbatim` to the hierarchy, and removes some things that don't matter for formatting (like `ExprContext`).

---

_Comment by @charliermarsh on 2023-06-13 22:52_

I'm looking for feedback on this concept before investing any more work in the migration.

---

_Comment by @github-actions[bot] on 2023-06-13 23:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.4±0.02ms     6.4 MB/sec    1.00      6.3±0.07ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1327.9±4.68µs    12.5 MB/sec    1.00   1319.3±3.33µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    127.0±0.15µs    23.2 MB/sec    1.00    127.4±0.17µs    23.2 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.02ms     9.7 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.3±0.08ms     2.8 MB/sec    1.00     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.3±0.96µs     6.9 MB/sec    1.00    427.6±0.80µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.00      6.0±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.7±3.15µs    11.2 MB/sec    1.00   1480.7±2.90µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.38µs    18.0 MB/sec    1.00    163.8±0.55µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.7±0.32ms     4.7 MB/sec    1.03      8.9±0.30ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1791.1±78.81µs     9.3 MB/sec    1.03  1841.4±57.15µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    182.5±8.39µs    16.2 MB/sec    1.00   182.1±10.27µs    16.2 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.16ms     6.9 MB/sec    1.00      3.7±0.14ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.01     19.0±0.64ms     2.1 MB/sec    1.00     18.9±0.55ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.21ms     3.5 MB/sec    1.03      4.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.4±24.03µs     5.1 MB/sec    1.03   594.1±21.71µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.28ms     3.2 MB/sec    1.07      8.5±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02      9.8±0.30ms     4.1 MB/sec    1.00      9.6±0.27ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.09ms     8.1 MB/sec    1.00      2.0±0.10ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.03   234.1±12.67µs    12.6 MB/sec    1.00    226.8±7.54µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.18ms     5.7 MB/sec    1.00      4.5±0.19ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-14 06:16_

Without having touched a lot of the existing generator code, this looks good to me.

Implementation-wise, would wrapping `Expr` either in an enum (`Verbatim` and `Default` variants) or through a `MaybeVerbatim` trait (accepts both `Expr` and `Verbatim<T: Node>`, `&Expr` -> `&impl MaybeVerbatim<Expr>`) work? This would avoid duplicating the files

---

_Comment by @MichaReiser on 2023-06-19 09:03_

I do like the idea. It seems a reasonable compromise between having an AST vs a CST.  A few questions/notes

* I think it should be possible to preserve the in-between comments if we come up with an explicit convention on when to associate comments to which nodes. This will not be perfect for e.g. trailing body comments. Rome used swifts convention (https://github.com/rome/tools/issues/1718)

1.    A token owns all of its trailing trivia up to, but not including, the next newline character.
2.    Looking backward in the text, a token owns all of the leading trivia up to and including the first newline character.


Is there a possibility that verbatim nodes and operator precedence doesn't play well together? E.g. you change the operator of a nested binary expression. Will the generator insert the parentheses in the right place? 

From my experience with verbatim nodes on the formatter. Is there a chance that a verbatim node may not have the right indent, e.g. when changing indentation and formatting a compound statement as verbatim.

**Implementation**

* A little less ergonomic, but would it instead be possible to keep a list of node-ranges that should be formatted as verbatim? This should be cheap to check if the ranges are stored in sorted order because the generator iterates the nodes in pre-order. It would remove the need for duplicating all nodes

---

_Comment by @charliermarsh on 2023-07-14 03:07_

We'll return to this at some point.

---

_Closed by @charliermarsh on 2023-07-14 03:07_

---
