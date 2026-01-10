```yaml
number: 16425
title: Avoid caching files with unsupported syntax errors
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - preview
  - cache
assignees: []
merged: true
base: main
head: brent/fix-syntax-error-caching
created_at: 2025-02-27T22:36:31Z
updated_at: 2025-02-28T08:58:13Z
url: https://github.com/astral-sh/ruff/pull/16425
synced_at: 2026-01-10T19:49:01Z
```

# Avoid caching files with unsupported syntax errors

---

_Pull request opened by @ntBre on 2025-02-27 22:36_

## Summary

This PR includes the new `Parsed::unsupported_syntax_errors` field when determining if a `LinterResult` has a syntax error. This prevents caching files with version-related syntax errors, which led to these errors only being reported on the first ruff run.

Fixes https://github.com/astral-sh/ruff/issues/16417 in combination with https://github.com/astral-sh/ruff/pull/16419, modulo the discussion split off into https://github.com/astral-sh/ruff/issues/16418.

## Test Plan

New CLI test that reported no errors in the second case on `main` (see the first commit).


---

_Label `bug` added by @ntBre on 2025-02-27 22:36_

---

_Label `preview` added by @ntBre on 2025-02-27 22:36_

---

_Comment by @github-actions[bot] on 2025-02-27 22:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-02-28 07:11_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/linter.rs`:448 on 2025-02-28 07:11_

I wonder if we should have a separate version of `is_valid` method that checks both `ParseError` and `UnsupportedSyntaxError` which can then be used here. I'm not sure what to name the method though as both are syntax errors at the end 😅

---

_@dhruvmanila approved on 2025-02-28 07:13_

---

_Label `cache` added by @dhruvmanila on 2025-02-28 07:14_

---

_@MichaReiser approved on 2025-02-28 07:31_

This looks great. I did a quick check that we aren't using the `LintResult::has_syntax_errors` for anything other than caching. 

Can we update the documentation on `LinterResult` to make it explicit that this includes both *syntax* and unsupported syntax errors.

---

_Merged by @MichaReiser on 2025-02-28 08:58_

---

_Closed by @MichaReiser on 2025-02-28 08:58_

---

_Branch deleted on 2025-02-28 08:58_

---
