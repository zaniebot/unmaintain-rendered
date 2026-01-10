```yaml
number: 16210
title: "[ruff] Resolve type alias for annotations (RUF012)"
type: pull_request
state: closed
author: vladNed
labels:
  - bug
assignees: []
base: main
head: bugfix/ruf012-does-not-respect-type-aliases
created_at: 2025-02-17T12:18:45Z
updated_at: 2025-04-28T07:19:40Z
url: https://github.com/astral-sh/ruff/pull/16210
synced_at: 2026-01-10T19:03:00Z
```

# [ruff] Resolve type alias for annotations (RUF012)

---

_Pull request opened by @vladNed on 2025-02-17 12:18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Check on mutable class default was not resolving the annotation that was a type alias thus the default values might have been flagged as mutable.

Added a helper function similar to `map_subscript` in order to check bindings on annotation to resolve to the final type.

Resolves #16174 

## Test Plan
- Checked with type aliases with both mutable and immutable 
- Checked the original behavior was intact.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-02-17 12:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @vladNed on 2025-02-17 12:47_

figured that there should be a loop for type inference to resolve until final if a alias is mapped to another alias.

---

_Comment by @vladNed on 2025-02-17 15:18_

added a small improvement, should be good for review now

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:210 on 2025-02-18 07:05_

I think we can avoid the clone here
```suggestion
    let Some(expr_name) = expr.as_name_expr() else {
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:224 on 2025-02-18 07:09_

Should we use `find_assigned_value` instead? This will only look in the current scope and will work with forward references as well.

https://github.com/astral-sh/ruff/blob/f58a54f043aab836706a22eb3f9b4d450477ba99/crates/ruff_python_semantic/src/analyze/typing.rs#L1137-L1151

---

_@dhruvmanila reviewed on 2025-02-18 07:11_

---

_Label `bug` added by @dhruvmanila on 2025-02-18 07:11_

---

_@vladNed reviewed on 2025-02-19 07:41_

---

_Review comment by @vladNed on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:224 on 2025-02-19 07:41_

@dhruvmanila I tested with `find_assigned_value` but its not fixing the forwarded value. If you import the type it does not resolves it completely.

I tried using multiple method with `ResolvedReferences` and with `resolve_qualified_name` but I didn't had any success on this. 

Is there any function that resolves a `FromImport` to an `Exp` somehow ? 

---

_@dhruvmanila reviewed on 2025-03-03 06:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:224 on 2025-03-03 06:06_

Sorry for the late reply, I somehow missed this in the notification.

Can you provide an example of what you're trying to test here?

> Is there any function that resolves a `FromImport` to an `Exp` somehow ?

Do you mean to follow the import and resolve it to the expression that defines it? That's not possible as it requires multi-file analysis.

---

_Comment by @MichaReiser on 2025-04-28 07:19_

Thank you for your work @vladNed. I'm closing this PR because it has become stale. Anyone interested to work on this, feel free to open a new PR with the changes.

---

_Closed by @MichaReiser on 2025-04-28 07:19_

---
