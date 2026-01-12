```yaml
number: 9016
title: "Introduce `StringLike` enum"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/string-like
created_at: 2023-12-05T20:33:33Z
updated_at: 2023-12-07T16:49:36Z
url: https://github.com/astral-sh/ruff/pull/9016
synced_at: 2026-01-10T23:40:55Z
```

# Introduce `StringLike` enum

---

_Pull request opened by @dhruvmanila on 2023-12-05 20:33_

## Summary

This PR introduces a new `StringLike` enum which is a narrow type to indicate string-like nodes. These includes the string literals, bytes literals, and the literal parts of f-strings.

The main motivation behind this is to avoid repetition of rule calling in the AST checker. We add a new `analyze::string_like` function which takes in the enum and calls all the respective rule functions which expects atleast 2 of the variants of this enum.

I'm open to discarding this if others think it's not that useful at this stage as currently only 3 rules require these nodes.

As suggested [here](https://github.com/astral-sh/ruff/pull/8835#discussion_r1414746934) and [here](https://github.com/astral-sh/ruff/pull/8835#discussion_r1414750204).

## Test Plan

`cargo test`


---

_Comment by @dhruvmanila on 2023-12-05 20:33_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-12-05 20:36_

---

_Comment by @github-actions[bot] on 2023-12-05 20:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2023-12-06 21:52_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-12-06 21:52_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-12-06 21:52_

---

_@charliermarsh reviewed on 2023-12-07 00:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:1276 on 2023-12-07 00:49_

Maybe just embed this in `analyze::expression` rather than as a separate call here?

---

_@charliermarsh approved on 2023-12-07 00:49_

No strong opinion, seems reasonable.

---

_@dhruvmanila reviewed on 2023-12-07 01:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:1276 on 2023-12-07 01:14_

This might be silly but wouldn't that mix the concerns? I'm fine either way.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/expression.rs`:401 on 2023-12-07 02:52_

Should `StringLike` have an `as_str()` method for easy comparision?

---

_@MichaReiser approved on 2023-12-07 02:52_

---

_@dhruvmanila reviewed on 2023-12-07 05:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/expression.rs`:401 on 2023-12-07 05:12_

I don't think so as it also contains bytes literals and implicitly concatenated strings which can possibly do an allocation.

---

_Merged by @dhruvmanila on 2023-12-07 16:39_

---

_Closed by @dhruvmanila on 2023-12-07 16:39_

---

_Branch deleted on 2023-12-07 16:39_

---
