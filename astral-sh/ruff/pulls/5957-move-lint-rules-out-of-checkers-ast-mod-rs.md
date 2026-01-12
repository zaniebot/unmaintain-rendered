```yaml
number: 5957
title: "Move lint rules out of `checkers/ast/mod.rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/analyzer-methods
created_at: 2023-07-21T21:41:07Z
updated_at: 2023-07-24T19:46:18Z
url: https://github.com/astral-sh/ruff/pull/5957
synced_at: 2026-01-12T15:55:20Z
```

# Move lint rules out of `checkers/ast/mod.rs`

---

_@charliermarsh_

## Summary

This PR attempts to draw some basic separation between the `Checker`'s traversal responsibilities (traversing the AST, building the semantic model) and its calling-out-to-lint-rule responsibilities. It doesn't try to introduce any sophisticated API. Instead, it just moves all of the lint rule calls out of `checkers/ast/mod.rs` and into methods in a new `analyze` module. (There are four remaining lint rules in `Checker`, but I'll remove those in future PRs.)

I'm not trying to "solve" our lint rule API here. Instead, I'm trying to make two improvements:

1. `checkers/ast/mod.rs` has just gotten way too large, and people work in it all the time. Prior to this PR, it was 5.5k lines, which led to significant lags in my editor and made it really hard to reason about the parts that are _actually_ important. (I like big files, but this one crossed the line for me.) Now, it's < 2,000 lines, and the code is much more focused.
2. I want to avoid accidentally adding lint rules in the "wrong" parts of the traversal. By confining lint rule invocations to these "analyze" calls, we'll avoid (e.g.) putting them in the binding phase.

If accepted, I'll update the docs prior to merging.


---

_@charliermarsh reviewed on 2023-07-21 21:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/arg.rs`:27 on 2023-07-21 21:41_

These are just taking the `// Step 1: Analysis` blocks from the existing `checkers/ast/mod.rs` and moving them into a method.

---

_@charliermarsh reviewed on 2023-07-21 21:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/bindings.rs`:7 on 2023-07-21 21:42_

This is `Checker::check_bindings`, but moved out of the file (and out of the struct). Arguably it should be an associative function, but it felt weird to put multiple `impl` blocks for `Checker` in so many different files. Anyway, IMO this is fine.

---

_Comment by @charliermarsh on 2023-07-21 21:43_

To be clear: no behavioral changes. This PR is mostly grabbing chunks of code from `checkers/ast/mod.rs`, moving them into methods in the `ast::analyze` module, and calling those methods instead of inlining the lint rules.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-21 21:43_

---

_Marked ready for review by @charliermarsh on 2023-07-21 21:43_

---

_Comment by @github-actions[bot] on 2023-07-21 21:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.8±0.49ms     3.5 MB/sec    1.03     12.2±0.36ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.11ms     6.9 MB/sec    1.00      2.4±0.12ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   266.9±11.39µs    11.1 MB/sec    1.04   277.1±15.00µs    10.7 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.23ms     5.0 MB/sec    1.06      5.4±0.30ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.02     18.0±0.57ms     2.3 MB/sec    1.00     17.6±0.58ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.5±0.24ms     3.7 MB/sec    1.00      4.4±0.16ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   581.2±46.45µs     5.1 MB/sec    1.04   606.3±80.78µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.30ms     3.1 MB/sec    1.00      8.1±0.25ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.29ms     4.5 MB/sec    1.04      9.5±0.36ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1903.4±49.31µs     8.7 MB/sec    1.03  1951.2±55.83µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.1±8.31µs    13.2 MB/sec    1.02    229.2±8.71µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.14ms     6.2 MB/sec    1.06      4.4±0.40ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     15.2±1.04ms     2.7 MB/sec    1.10     16.7±0.68ms     2.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.9±0.18ms     5.7 MB/sec    1.05      3.1±0.15ms     5.4 MB/sec
formatter/numpy/globals.py                 1.00   347.4±23.94µs     8.5 MB/sec    1.02   353.3±26.04µs     8.4 MB/sec
formatter/pydantic/types.py                1.00      6.7±0.36ms     3.8 MB/sec    1.03      7.0±0.32ms     3.7 MB/sec
linter/all-rules/large/dataset.py          1.01     23.3±0.87ms  1788.2 KB/sec    1.00     23.1±0.83ms  1799.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.43ms     2.9 MB/sec    1.06      6.1±0.25ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   687.4±42.57µs     4.3 MB/sec    1.02   700.4±46.26µs     4.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.53ms     2.6 MB/sec    1.09     10.8±0.41ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.6±0.59ms     3.2 MB/sec    1.01     12.8±0.50ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.10ms     6.9 MB/sec    1.03      2.5±0.11ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   297.6±19.87µs     9.9 MB/sec    1.00   294.1±14.01µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.7±0.66ms     4.5 MB/sec    1.03      5.9±0.43ms     4.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-07-22 14:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/arg.rs`:8 on 2023-07-22 14:19_

What I mostly need feedback on is whether this API and module structure is fine or if we prefer something else. Like we could do something trait-based (but it won't work for some of the other methods in this module, like `scopes`, which checks _all_ the scopes.) Or we could make these methods on `Checker` but just implement them in separate files for convenience (but then we risk method name clashes across `impl` blocks).

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:1231 on 2023-07-22 18:59_

Nit: is it possible to move the push of diagnostics inside the rule everywhere? Shortens this file a bit, and might be helpful if later to have the consistency if macros will be used

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:1224 on 2023-07-22 19:00_

Same as below

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:1242 on 2023-07-22 19:00_

Ifs can be combined

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:1374 on 2023-07-22 19:02_

Nit: rules could be grouped/sorted by their arguments (here `expr`, `bool_op`). 

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/scopes.rs`:36 on 2023-07-22 19:07_

Simplify as `!checker.is_stub && ...`

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/scopes.rs`:47 on 2023-07-22 19:10_

Combine ifs with `||`

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/scopes.rs`:236 on 2023-07-22 19:12_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/scopes.rs`:258 on 2023-07-22 19:16_

Can this be moved inside the second check below ? Doesn't seem to be used in the other rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:38 on 2023-07-22 19:17_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:54 on 2023-07-22 19:18_

See earlier, move in rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:64 on 2023-07-22 19:18_

Move in rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:92 on 2023-07-22 19:18_

Move in rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:100 on 2023-07-22 19:18_

Move in rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:111 on 2023-07-22 19:19_

See above

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:124 on 2023-07-22 19:19_

See above

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:172 on 2023-07-22 19:20_

Group by arguments required. Suggested:
```
// Checks that require `args`, `keywords`
```


---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:177 on 2023-07-22 19:21_

Move in check 

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:190 on 2023-07-22 19:21_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:195 on 2023-07-22 19:21_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:223 on 2023-07-22 19:21_

Move in rule 

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:242 on 2023-07-22 19:22_

Move in rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:251 on 2023-07-22 19:22_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:260 on 2023-07-22 19:22_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:377 on 2023-07-22 19:24_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:384 on 2023-07-22 19:24_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:394 on 2023-07-22 19:25_

Move in same check below

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:408 on 2023-07-22 19:25_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:422 on 2023-07-22 19:25_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:431 on 2023-07-22 19:25_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:441 on 2023-07-22 19:26_

Use else 

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:530 on 2023-07-22 19:28_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:557 on 2023-07-22 19:29_

Combine first condition of this loop

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:560 on 2023-07-22 19:29_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:622 on 2023-07-22 19:29_

Combine with first condition of loop

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:636 on 2023-07-22 19:30_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:718 on 2023-07-22 19:30_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:672 on 2023-07-22 19:31_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:711 on 2023-07-22 19:32_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:728 on 2023-07-22 19:32_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:737 on 2023-07-22 19:33_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:786 on 2023-07-22 19:33_

Beginning of loop

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:801 on 2023-07-22 19:34_

Beginning of loop

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:863 on 2023-07-22 19:34_

Stub already checked above, group conditions

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:894 on 2023-07-22 19:40_

Combine with previous condition

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:903 on 2023-07-22 19:40_

Combine above

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:908 on 2023-07-22 19:40_

Combine above

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:923 on 2023-07-22 19:41_

Combine all some expr

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:942 on 2023-07-22 19:42_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:983 on 2023-07-22 19:43_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1005 on 2023-07-22 19:45_

Can we combine this if let? It's used multiple times

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1039 on 2023-07-22 19:45_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1175 on 2023-07-22 19:46_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1180 on 2023-07-22 19:46_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1266 on 2023-07-22 19:47_

^

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1270 on 2023-07-22 19:48_

Checker.enabled?

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1281 on 2023-07-22 19:48_

Checker.enabled()

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1294 on 2023-07-22 19:48_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1303 on 2023-07-22 19:49_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:1357 on 2023-07-22 19:49_

Combine with above

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/arg.rs`:10 on 2023-07-22 19:51_

Move diagnostic push inside rule consistently 

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/bindings.rs`:20 on 2023-07-22 19:51_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/bindings.rs`:21 on 2023-07-22 19:52_

Move inside rule

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/bindings.rs`:59 on 2023-07-22 19:52_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/definitions.rs`:140 on 2023-07-22 19:53_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:155 on 2023-07-22 19:55_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:182 on 2023-07-22 19:56_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:202 on 2023-07-22 19:56_

Combine ifs

---

_Review comment by @sbrugman on `crates/ruff/src/checkers/ast/analyze/expr.rs`:447 on 2023-07-22 19:58_

Combine ifs

---

_@sbrugman reviewed on 2023-07-22 19:59_

Going over the complete code reveals some small suggestions for improvements to pave the way for metaprogramming (mostly mechanical refactoring):
- Sometimes a condition is checked multiple times within the same block (e.g. is_stub)
- Sometimes if conditions can be combined. This is equivalent, but is more readable as it removed one level of indentation. Logical operators are short-circuited in Rust, so it doesn't come at computational cost.
- Grouping/sorting checks by the arguments needed may allow use in the future to automate away the boilerplate (e.g. turning off paths without any active checks), and helps spotting potential errors.
- Rules are not consistently returning/adding diagnostics to the checker. Consistency is preferable. Checker is then always the first argument.

(I've not got deep enough knowledge of rust design patterns to address the high-level API choices unfortunately)

Open to hear your thoughts, I might be completely off on some points. Happy to contribute.

---

_@charliermarsh reviewed on 2023-07-23 00:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/expr.rs`:1231 on 2023-07-23 00:37_

(Yes, that's my preferred pattern and we should change rule to do so where we can.)

---

_Comment by @konstin on 2023-07-23 10:03_

i like breaking up the checker!

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/for_loops.rs`:8 on 2023-07-24 07:07_

Nit: 
```suggestion
pub(crate) fn deferred_for_loops(checker: &mut Checker) {
```

I was confused why the function has no node argument.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/body.rs`:8 on 2023-07-24 07:07_

```suggestion
pub(crate) fn suite(suite: &[Stmt], checker: &mut Checker) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/expr.rs`:23 on 2023-07-24 07:08_

Nit
```suggestion
pub(crate) fn expression(expr: &Expr, checker: &mut Checker) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/analyze/stmt.rs`:22 on 2023-07-24 07:08_

Nit
```suggestion
pub(crate) fn statement(stmt: &Stmt, checker: &mut Checker) {
```

---

_@MichaReiser approved on 2023-07-24 07:08_

I haven't reviewed in detail because I assume most of the changes are just moves. 

I would probably have preferred to have all rules in a single file because we now have may files that barely contain anything. 

Future: Ideally the rules wouldn't accept `Checker` because it means that we have cyclic dependencies: 

* Checker depends on `analyze` which depends on all rules
* `analyze` and all rules depend on `Checker`

---

_@charliermarsh reviewed on 2023-07-24 19:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/analyze/bindings.rs`:20 on 2023-07-24 19:07_

I intentionally like to keep the `checker.enabled` checks separate from any preconditions for the rule. I find it easier to read, and I have a perhaps-mistaken belief that it might help with future refactors when we make the rule API more declarative.

---

_Merged by @charliermarsh on 2023-07-24 19:20_

---

_Closed by @charliermarsh on 2023-07-24 19:20_

---

_Branch deleted on 2023-07-24 19:20_

---
