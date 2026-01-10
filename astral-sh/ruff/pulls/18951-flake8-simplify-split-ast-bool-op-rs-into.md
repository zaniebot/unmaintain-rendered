```yaml
number: 18951
title: "[`flake8-simplify`] Split `ast_bool_op.rs` into multiple files"
type: pull_request
state: closed
author: MeGaGiGaGon
labels:
  - internal
assignees: []
base: main
head: split-ast_bool_op
created_at: 2025-06-26T05:47:20Z
updated_at: 2025-07-11T22:55:13Z
url: https://github.com/astral-sh/ruff/pull/18951
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-simplify`] Split `ast_bool_op.rs` into multiple files

---

_Pull request opened by @MeGaGiGaGon on 2025-06-26 05:47_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

While trying to work on #15584, and the other issues I've found with some of these lints, I've become frustrated with how hard it is to navigate/reason about the lints inside `ast_bool_op.rs` due to there being 6 lints in one file that are not all related. This PR splits them out into their own individual files.

I split a bit more than might have been necessary, as there are 4 "groups" to the lints in `ast_bool_op.rs`:
- SIM101 DuplicateIsinstanceCall
- SIM109 CompareWithTuple
- SIM220 ExprAndNotExpr + SIM221 ExprOrNotExpr
- SIM222 ExprOrTrue + SIM223 ExprAndFalse

I still decided to split them all out for consistency, but that did necessitate the creation of a `helper.rs` file since `SIM220` + `SIM221` share `is_same_expr`, and `SIM222` + `SIM223` share `ContentAround, is_short_circuit`, so I would be open to re-combining those two groups into their own files if desired.

If this PR goes over well, I may make several more follow up PRs splitting other frustrating combined-yet-unrelated lints into their own files.

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected.

---

_Comment by @github-actions[bot] on 2025-06-26 05:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2025-06-26 08:50_

---

_Comment by @MichaReiser on 2025-06-26 08:52_

> If this PR goes over well, I may make several more follow up PRs splitting other frustrating combined-yet-unrelated lints into their own files.

I'm not sure it's worth pre-emptively extracting all those rules. It does come at the cost of making it harder to follow changes (git blame/history). That's why I wouldn't split rules to which we don't plan to make any changes in the foreseeable future. Splitting rules that you plan to work on seems fine.

---

_Comment by @ntBre on 2025-06-27 17:37_

Thanks, I'll give this a review soon. I agree with Micha about not doing this pre-emptively, but it's fine if you're already working on the rules.

---

_Comment by @MeGaGiGaGon on 2025-06-29 06:29_

I’ve been thinking about it more, and I don’t think what I’m planning warrants splitting this out yet over the loss of history, so I’m going to close this PR.

---

_Closed by @MeGaGiGaGon on 2025-06-29 06:29_

---

_Branch deleted on 2025-07-11 22:55_

---
