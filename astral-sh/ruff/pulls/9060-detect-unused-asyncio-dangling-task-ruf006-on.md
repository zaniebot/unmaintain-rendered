```yaml
number: 9060
title: "Detect `unused-asyncio-dangling-task` (`RUF006`) on unused assignments"
type: pull_request
state: merged
author: asafamr-mm
labels:
  - bug
assignees: []
merged: true
base: main
head: unused-asyncio-dangling-task
created_at: 2023-12-08T21:04:22Z
updated_at: 2023-12-23T19:04:37Z
url: https://github.com/astral-sh/ruff/pull/9060
synced_at: 2026-01-10T23:07:18Z
```

# Detect `unused-asyncio-dangling-task` (`RUF006`) on unused assignments

---

_Pull request opened by @asafamr-mm on 2023-12-08 21:04_


## Summary

Fixes #8863 : Detect asyncio-dangling-task (RUF006) when discarding return value 

## Test Plan

added new two testcases, changed result of an old one that was made more specific

---

_Comment by @github-actions[bot] on 2023-12-08 21:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2023-12-08 22:15_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1565 on 2023-12-08 22:15_

Why this change? This doesn't match the pattern we use to add a diagnostic for other rules.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1565 on 2023-12-08 22:24_

Probably because it _is_ the pattern we use for the deferred scopes diagnostics, since they can't take a `mut` on `Checker` :)

I'd probably prefer changing the rule to return `Option<Diagnostic>` though, and then pushing onto the appropriate vector. That should work fine for both call-sites.

---

_@charliermarsh reviewed on 2023-12-08 22:24_

---

_@charliermarsh reviewed on 2023-12-08 22:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:130 on 2023-12-08 22:24_

Nice job figuring this out.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:130 on 2023-12-08 22:27_

Can you try using `typing::analyze::resolve_assignment` here instead? It will also handle `AnnAssign`.

---

_@charliermarsh reviewed on 2023-12-08 22:27_

---

_@charliermarsh reviewed on 2023-12-08 22:27_

Nice job! A few small comments. Thank you so much for taking this on.

---

_@zanieb reviewed on 2023-12-09 01:02_

---

_Review comment by @zanieb on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1565 on 2023-12-09 01:02_

:D Don't listen to me listen to Charlie.

---

_@asafamr-mm reviewed on 2023-12-09 20:09_

---

_Review comment by @asafamr-mm on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1565 on 2023-12-09 20:09_

👍 changed to `Option<Diagnostic>` 

---

_@asafamr-mm reviewed on 2023-12-09 20:49_

---

_Review comment by @asafamr-mm on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:130 on 2023-12-09 20:49_

Hmm, not sure how to do it without some refactoring.
I currently use assignment value expression which (if I'm not mistaken) is discarded after processing in `resolve_assignment`?

For now I've extended the pattern match to include AnnAssign as done in [unnecessary_list_cast](https://github.com/asafamr-mm/ruff/blob/00bcfc87c8b5d1380f4902d4019205b2840caac6/crates/ruff_linter/src/rules/perflint/rules/unnecessary_list_cast.rs#L120 ) - but would gladly change it 

---

_@charliermarsh approved on 2023-12-09 21:05_

Thanks again!

---

_Renamed from "RUF006 unused asyncio dangling task should trigger " to "Detect `unused-asyncio-dangling-task` (`RUF006`) on unused assignments" by @charliermarsh on 2023-12-09 21:06_

---

_Label `bug` added by @charliermarsh on 2023-12-09 21:06_

---

_Merged by @charliermarsh on 2023-12-09 21:10_

---

_Closed by @charliermarsh on 2023-12-09 21:10_

---

_Comment by @hauntsaninja on 2023-12-23 17:38_

I think this PR has caused ruff to start complaining even when assignments are to globals and nonlocals. Is that intentional?

---

_Comment by @charliermarsh on 2023-12-23 18:16_

@hauntsaninja - probably not — if you share an example I’m happy to ensure it’s fixed in the next release.

---

_Comment by @hauntsaninja on 2023-12-23 19:04_

Thanks, filed https://github.com/astral-sh/ruff/issues/9262 !

---
