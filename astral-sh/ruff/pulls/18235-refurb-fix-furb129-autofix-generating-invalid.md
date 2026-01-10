```yaml
number: 18235
title: "[`refurb`] Fix `FURB129` autofix generating invalid syntax"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-FURB129
created_at: 2025-05-20T22:48:18Z
updated_at: 2025-05-28T21:01:37Z
url: https://github.com/astral-sh/ruff/pull/18235
synced_at: 2026-01-10T18:45:04Z
```

# [`refurb`] Fix `FURB129` autofix generating invalid syntax

---

_Pull request opened by @LaBatata101 on 2025-05-20 22:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #18231

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Snapshot tests
<!-- How was it tested? -->


---

_@LaBatata101 reviewed on 2025-05-20 22:51_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:96 on 2025-05-20 22:51_

Is there a better way to unpack the parenthesized expression here? I looked but didn't find any.

When the expression has multiple parenthesis like in:
```python
with open("furb129.py") as f:
    for line in (((f))).readlines():
        pass
``` 

the fix will generate this `for line in ((f)):`, would that be the correct behavior or should it be just `f`?

---

_Comment by @github-actions[bot] on 2025-05-20 22:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:96 on 2025-05-21 00:53_

Is it necessary to remove any parentheses? Removing them when fixing `for line in(f).readlines()` would produce a syntax error, similar to #17683.

---

_@dscorbett reviewed on 2025-05-21 00:53_

---

_@LaBatata101 reviewed on 2025-05-21 01:14_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:96 on 2025-05-21 01:14_

> Is it necessary to remove any parentheses?

Not really, I guess. 

---

_@penguinolog reviewed on 2025-05-21 08:12_

---

_Review comment by @penguinolog on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:96 on 2025-05-21 08:12_

> Is it necessary to remove any parentheses? Removing them when fixing `for line in(f).readlines()` would produce a syntax error, similar to #17683.

But `for line in f.readlines()` is not incorrect syntax. Syntax error itself is `for line in(f)` block: `in` is not a function and whitespace should not be removed

---

_@LaBatata101 reviewed on 2025-05-21 12:53_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/readlines_in_for.rs`:96 on 2025-05-21 12:53_

 > But `for line in f.readlines()` is not incorrect syntax. Syntax error itself is `for line in(f)` block: `in` is not a function and whitespace should not be removed

`for line in(f)`  is not a syntax error, `in` is lexed as a keyword and not as an identifier, so `in(f)` is not a function call.



---

_Label `bug` added by @ntBre on 2025-05-28 20:45_

---

_Label `fixes` added by @ntBre on 2025-05-28 20:45_

---

_@ntBre approved on 2025-05-28 21:00_

Looks good to me, thanks!

---

_Merged by @ntBre on 2025-05-28 21:01_

---

_Closed by @ntBre on 2025-05-28 21:01_

---

_Branch deleted on 2025-05-28 21:01_

---
