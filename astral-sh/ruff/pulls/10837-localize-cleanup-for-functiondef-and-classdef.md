```yaml
number: 10837
title: Localize cleanup for FunctionDef and ClassDef
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/simplify
created_at: 2024-04-08T19:15:55Z
updated_at: 2024-04-11T23:35:35Z
url: https://github.com/astral-sh/ruff/pull/10837
synced_at: 2026-01-10T22:37:01Z
```

# Localize cleanup for FunctionDef and ClassDef

---

_Pull request opened by @carljm on 2024-04-08 19:15_

## Summary

Came across this code while digging into the semantic model with @AlexWaygood, and found it confusing because of how it splits `push_scope` from the paired `pop_scope` (took me a few minutes to even figure out if/where we were popping the pushed scope). Since this "cleanup" is already totally split by node type, there doesn't seem to be any gain in having it as a separate "step" rather than just incorporating it into the traversal clauses for those node types.

I left the equivalent cleanup step alone for the expression case, because in that case it is actually generic across several different node types, and due to the use of the common `visit_generators` utility there isn't a clear way to keep the pushes and corresponding pops localized.

Feel free to just reject this if I've missed a good reason for it to stay this way!

## Test Plan

Tests and clippy.

---

_@charliermarsh reviewed on 2024-04-08 19:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:916 on 2024-04-08 19:16_

Nit: should we remove this, and change the next comment ("Step 4" to "Step 3")?

---

_@charliermarsh approved on 2024-04-08 19:17_

---

_Comment by @charliermarsh on 2024-04-08 19:17_

Do you mind adding the "Internal" label to this one? We have our release infrastructure configured such that PRs labeled "Internal" are automatically filtered out from the release notes, so saves us some time later.

---

_Label `internal` added by @carljm on 2024-04-08 19:18_

---

_@carljm reviewed on 2024-04-08 19:18_

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:916 on 2024-04-08 19:18_

I actually did that initially, and then I reverted it because the "four steps" are a consistent and common pattern throughout this file, and they are always the same, so I thought it better to leave the empty marker and stay consistent.

---

_Comment by @carljm on 2024-04-08 19:18_

Gah, one of these days I'll actually remember to tag my own PR 🙄 

---

_@carljm reviewed on 2024-04-08 19:19_

---

_Review comment by @carljm on `crates/ruff_linter/src/checkers/ast/mod.rs`:916 on 2024-04-08 19:19_

But I can remove the empty step and re-number if you prefer that

---

_@charliermarsh reviewed on 2024-04-08 19:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:916 on 2024-04-08 19:23_

No, on second thought, you're right!

---

_Comment by @github-actions[bot] on 2024-04-08 19:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-04-08 19:29_

---

_Closed by @carljm on 2024-04-08 19:29_

---

_Branch deleted on 2024-04-08 19:29_

---

_Comment by @davisagli on 2024-04-09 05:12_

Hey there, I'm lurking because I wanted to see what @carljm is up to in his first days at Astral. :) We have a similar scheme for release notes based on labels in a project I work on, and we use https://github.com/agilepathway/label-checker to set a failed check on the PR if it doesn't have one of the required labels.

---

_Comment by @carljm on 2024-04-09 05:58_

Hi @davisagli !

I just brought up this automation idea the other day :) But then I withdrew it; I think it would make sense for a private project, but for a project with lots of contributors like ruff, where most PR authors don't even have the rights to set labels, I think it's a bad contributor experience (and a bad reviewer experience!) for every PR to by default get the big red X until a maintainer comes along to set a label. I'd rather have the maintainers need to remember to do it. 

---

_Comment by @T-256 on 2024-04-11 23:35_

(Just my thoughts and possible solutions on mentioned issues at here)

> most PR authors don't even have the rights to set labels

Isn't it possible to specify this for label-checker to validate if PR author has label right or not?



> get the big red X until a maintainer comes along to set a label.

I think it could be as _Skipped_ task instead of _big red X_

---
