```yaml
number: 6034
title: Placement refactor
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: placement_refactor
created_at: 2023-07-24T15:14:49Z
updated_at: 2023-07-25T09:49:07Z
url: https://github.com/astral-sh/ruff/pull/6034
synced_at: 2026-01-12T15:55:20Z
```

# Placement refactor

---

_@konstin_

## Summary

This PR is a refactoring of placement.rs. The code got more consistent, some comments were updated and some dead code was removed or replaced with debug assertions. It also contains a bugfix for the placement of end-of-branch comments with nested bodies inside try statements that occurred when refactoring the nested body loop.

## Test Plan

The existing test cases don't change. I added a couple of cases that i think should be tested but weren't, and a regression test for the bugfix


---

_Comment by @github-actions[bot] on 2023-07-24 15:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     10.1±0.35ms     4.0 MB/sec     1.00      9.9±0.42ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1994.6±104.07µs     8.3 MB/sec    1.01      2.0±0.09ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00   229.2±11.66µs    12.9 MB/sec     1.02   233.9±12.36µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.19ms     5.9 MB/sec     1.00      4.3±0.21ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.56ms     2.9 MB/sec     1.00     14.0±0.59ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.15ms     4.7 MB/sec     1.01      3.6±0.14ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   467.8±20.63µs     6.3 MB/sec     1.08    503.0±8.44µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.23ms     4.0 MB/sec     1.04      6.6±0.25ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.26ms     5.4 MB/sec     1.03      7.8±0.18ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1663.8±35.31µs    10.0 MB/sec     1.00  1558.4±62.72µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.4±8.35µs    17.1 MB/sec     1.02    175.5±9.22µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.15ms     7.7 MB/sec     1.00      3.3±0.14ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.0±0.35ms     3.1 MB/sec    1.01     13.1±0.32ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.09ms     6.5 MB/sec    1.00      2.6±0.07ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   295.6±16.50µs    10.0 MB/sec    1.01   297.9±14.71µs     9.9 MB/sec
formatter/pydantic/types.py                1.00      5.6±0.21ms     4.5 MB/sec    1.01      5.7±0.19ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.9±0.35ms     2.3 MB/sec    1.00     17.9±0.29ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.8±0.10ms     3.5 MB/sec    1.00      4.7±0.11ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   578.0±13.89µs     5.1 MB/sec    1.00   574.6±14.74µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.23ms     3.1 MB/sec    1.00      8.2±0.17ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.7±0.18ms     4.2 MB/sec    1.00      9.6±0.22ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.05ms     8.3 MB/sec    1.01      2.0±0.05ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    231.9±8.61µs    12.7 MB/sec    1.01    234.3±8.55µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.11ms     5.9 MB/sec    1.00      4.3±0.17ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-07-24 15:37_

---

_Comment by @konstin on 2023-07-24 18:04_

Current dependencies on/for this PR:
* main
  * **PR #6033** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6033" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #6034** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6034" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #6040** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6040" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/6034?utm_source=stack-comment).

---

_Comment by @konstin on 2023-07-24 20:50_

@MichaReiser do you prefer reviewing this and then a next PR changing placement.rs again or do prefer an all-in-one complete rewrite PR?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:4397 on 2023-07-25 07:02_

What's the meaning of the `with_node` suffix? Should the function also return true for `ElifElseClause`s? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:272 on 2023-07-25 07:07_

Nit: It can be helpful for reviewers to add some comments to your PR, explaining what has changed. For example. I have to diff this part line by line because I don't know whether this part contains logical or structural (`if...else` to `let ... else`), which costs me a lot of time. This is where comments from the author can guide reviews to what's most important. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:277 on 2023-07-25 07:08_

Can we move the `comment_indentation` computation after this early return? Or can we remove it entirely? It seems that `own_line_comment_after_branch` recomputes the indentation. If not, can we pass the indentation to `own_line_comment_after_branch`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:316 on 2023-07-25 07:09_

Nit: I get the `with_node` suffix now. I'm leaning towards keeping this function local to `placement.rs`. It seems rather specific to the needs of the formatter.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:465 on 2023-07-25 07:16_

Hmm... I remember that I first used `unwrap_or_default` here but then had to change it to explicitly early return if the indentation is missing. Does this no longer apply because this code now only runs for an own line comment? What if we have

```python
if test: print("True") 
    # comment
```

(or something similar)? Can you take a second look if this is indeed unproblematic?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:491 on 2023-07-25 07:17_

Ooch, I'm a big fan of using `break` with `loop` ;)

---

_@MichaReiser approved on 2023-07-25 07:19_

Nice. I didn't review line by line but I love how this unifies a lot of the individual rules (at least, the heavily lifting of them)

---

_Comment by @MichaReiser on 2023-07-25 07:20_

> @MichaReiser do you prefer reviewing this and then a next PR changing placement.rs again or do prefer an all-in-one complete rewrite PR?

I haven't looked at the next PR but I prefer smaller PRs. `placement.rs` is challenging to review.

---

_@konstin reviewed on 2023-07-25 09:46_

---

_Review comment by @konstin on `crates/ruff_python_ast/src/node.rs`:4397 on 2023-07-25 09:46_

it does!

---

_@konstin reviewed on 2023-07-25 09:48_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:277 on 2023-07-25 09:48_

afaik this shouldn't actually be an early return

---

_Merged by @konstin on 2023-07-25 09:49_

---

_Closed by @konstin on 2023-07-25 09:49_

---

_Branch deleted on 2023-07-25 09:49_

---
