```yaml
number: 8063
title: "New `Singleton` enum for `PatternMatchSingleton` node"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/match-pattern-singleton
created_at: 2023-10-19T13:47:57Z
updated_at: 2023-12-05T20:33:45Z
url: https://github.com/astral-sh/ruff/pull/8063
synced_at: 2026-01-10T23:40:55Z
```

# New `Singleton` enum for `PatternMatchSingleton` node

---

_Pull request opened by @dhruvmanila on 2023-10-19 13:47_

## Summary

This PR adds a new `Singleton` enum for the `PatternMatchSingleton` node.

Earlier the node was using the `Constant` enum but the value for this pattern can only be either `None`, `True` or `False`. With the coming PR to remove the `Constant`, this node required a new type to fill in.

This also has the benefit of narrowing the type down to only the possible values for the node as evident by the removal of `unreachable`.

## Test Plan

Update the AST snapshots and run `cargo test`.


---

_Comment by @dhruvmanila on 2023-10-19 13:48_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment).

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor/preorder.rs`:49 on 2023-10-19 18:04_

I don't think this should be defined 🤔 

---

_Label `internal` added by @dhruvmanila on 2023-10-19 19:30_

---

_@dhruvmanila reviewed on 2023-10-19 19:39_

---

_Marked ready for review by @dhruvmanila on 2023-10-19 19:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-10-19 19:39_

---

_Comment by @github-actions[bot] on 2023-10-19 19:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:330 on 2023-10-19 23:33_

Nit: I find `Singleton` a confusing name. I assume it comes from Python's AST definition?

Or should this match the literal case? https://peps.python.org/pep-0622/#literal-patterns

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:3142 on 2023-10-19 23:35_

What about constant's in other positions? e.g. do we call this function when visiting a `ExprConstant` too?

---

_@MichaReiser reviewed on 2023-10-19 23:35_

---

_@dhruvmanila reviewed on 2023-10-20 03:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:330 on 2023-10-20 03:09_

> I assume it comes from Python's AST definition?

Yes, and in turn ours as both uses the "Singleton" suffix.

It is for the literal pattern but only a subset of it - `None`, `True`, `False`. The string and number literal are represented using `PatternMatchValue` while the singletons are using `PatternMatchSingleton`.

https://play.ruff.rs/2834cfa6-4d0d-4aaf-bbd2-1bd722fdd2ba

---

_@dhruvmanila reviewed on 2023-10-20 03:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/node.rs`:3142 on 2023-10-20 03:15_

Yes, we do:

https://github.com/astral-sh/ruff/blob/db9d767841f8e5a1ec49275e6731d7c21777c256/crates/ruff_python_ast/src/node.rs#L2681-L2687

I'm not sure if there should be a `visit_singleton` or not. Do we define visitor methods for the enum values as well? Wouldn't they be accounted for when visiting the enclosing node? Looking at `Constant`, we do.

---

_@MichaReiser reviewed on 2023-10-20 04:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node.rs`:3142 on 2023-10-20 04:41_

I'm not sure. I guess it can be useful when the same non-nodes may have different parent nodes but I would be okay only having visitors for nodes. @charliermarsh should have more context on why `visit_constant` exists.

---

_@MichaReiser approved on 2023-10-20 04:48_

I'm not a huge fan of the `Singleton` terminology because it only makes me thing about the singleton pattern but it is in line with Python's terminology.  It also seem to make sense from a technical perspective (I didn't know that) because `None`, `True`, and `False` are indeed all singleton values 🤯 

The one alternative I could think of is a `KeywordLiteral`. 

---

_Comment by @dhruvmanila on 2023-10-20 10:49_

> It also seem to make sense from a technical perspective (I didn't know that) because `None`, `True`, and `False` are indeed all singleton values 🤯

Ah yes, I should've written that in the PR description.

> The one alternative I could think of is a `KeywordLiteral`.

This might be confusing as other keywords are not part of it.


---

_Closed by @dhruvmanila on 2023-10-30 05:40_

---

_Reopened by @dhruvmanila on 2023-10-30 05:40_

---

_Merged by @dhruvmanila on 2023-10-30 05:48_

---

_Closed by @dhruvmanila on 2023-10-30 05:48_

---

_Branch deleted on 2023-10-30 05:48_

---
