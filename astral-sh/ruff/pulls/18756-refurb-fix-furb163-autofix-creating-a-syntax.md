```yaml
number: 18756
title: "[`refurb`] Fix `FURB163` autofix creating a syntax error for `yield` expressions"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-FURB163
created_at: 2025-06-18T14:46:50Z
updated_at: 2025-06-23T13:11:27Z
url: https://github.com/astral-sh/ruff/pull/18756
synced_at: 2026-01-10T18:39:08Z
```

# [`refurb`] Fix `FURB163` autofix creating a syntax error for `yield` expressions

---

_Pull request opened by @LaBatata101 on 2025-06-18 14:46_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #18747

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Label `bug` added by @ntBre on 2025-06-18 17:39_

---

_@ntBre approved on 2025-06-18 17:47_

Looks good, thanks! I tested a couple of other potentially-tricky cases like `await` and `if` expressions, but I think `yield` and `yield from` are the only problematic forms. We just need to resolve the merge conflicts, and then this is good to go.

---

_Comment by @LaBatata101 on 2025-06-18 19:51_

> Looks good, thanks! I tested a couple of other potentially-tricky cases like `await` and `if` expressions, but I think `yield` and `yield from` are the only problematic forms. We just need to resolve the merge conflicts, and then this is good to go.

Done!

---

_Review requested from @ntBre by @LaBatata101 on 2025-06-18 19:51_

---

_Comment by @github-actions[bot] on 2025-06-18 19:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-06-20 06:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:153 on 2025-06-20 06:24_

Could we use `OperatorPrecedence` here instead of hard coding `Yield` and `YieldFrom`

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:153 on 2025-06-20 13:58_

Is there any benefit of using `OperatorPrecedence` over the `matches!(arg, Expr::Yield(_) | Expr::YieldFrom(_))`?

---

_@LaBatata101 reviewed on 2025-06-20 13:58_

---

_@ntBre reviewed on 2025-06-20 14:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:153 on 2025-06-20 14:12_

I guess it would be more general in case something changes in the future and also make it easier to find precedence related checks. I haven't actually used `OperatorPrecedence` myself, but it looks like `Yield` has the lowest precedence, so something like `OperatorPrecedence::from(arg) <= OperatorPrecedence::Yield` should be equivalent to the `matches!` call?

Is that what you had in mind @MichaReiser?

---

_@MichaReiser reviewed on 2025-06-21 16:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:153 on 2025-06-21 16:23_

Looking at the fix. I'm also wondering if we could simply preserve any existing parentheses because that would fix it too. 


> OperatorPrecedence::from(arg) <= OperatorPrecedence::Yield

Roughly. Ideally there would be a `,` `OperatorPrecedence` but that doesn't exist. The main advantage is that this rule would be correct if we ever end up introducing a new `OperatorPrecedence` that binds lower than `Yield` (and we can check all `Yield` usages if it binds one level higher than `Yield`).

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-21 21:33_

---

_@MichaReiser approved on 2025-06-23 08:12_

Thank you

---

_Merged by @MichaReiser on 2025-06-23 08:13_

---

_Closed by @MichaReiser on 2025-06-23 08:13_

---

_Branch deleted on 2025-06-23 13:11_

---
