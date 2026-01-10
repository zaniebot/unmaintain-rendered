```yaml
number: 18113
title: "Bump `py-fuzzer` Dependencies"
type: pull_request
state: merged
author: maxmynter
labels:
  - ci
assignees: []
merged: true
base: main
head: update-fuzzed-deps
created_at: 2025-05-15T06:07:21Z
updated_at: 2025-05-15T14:47:37Z
url: https://github.com/astral-sh/ruff/pull/18113
synced_at: 2026-01-10T18:51:01Z
```

# Bump `py-fuzzer` Dependencies

---

_Pull request opened by @maxmynter on 2025-05-15 06:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->
Closes: #17427 

## Summary
Bumps dependencies of `py-fuzzer` to latest version on PyPI and regenerate `uv.lock`.
  
Corrected the example in the docstring to use `--only-new-bugs` instead of `--new-bugs-only` to match the actual argument name.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Ran all the commands mentioned in the `--help` and checked that none errors. 
<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @maxmynter on 2025-05-15 06:07_

---

_Comment by @github-actions[bot] on 2025-05-15 06:19_

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

_Label `ci` added by @MichaReiser on 2025-05-15 06:26_

---

_@AlexWaygood approved on 2025-05-15 14:47_

Thank you! I double-checked, and the script continues to find ty panics in the same set of seeds that it does on `main`.

---

_Merged by @AlexWaygood on 2025-05-15 14:47_

---

_Closed by @AlexWaygood on 2025-05-15 14:47_

---
