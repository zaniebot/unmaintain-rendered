```yaml
number: 16190
title: Add doc about usedforsecurity flag for S324
type: pull_request
state: merged
author: Skylion007
labels:
  - documentation
assignees: []
merged: true
base: main
head: skylion007/add-doc-usedforsecurity-S324
created_at: 2025-02-16T16:13:47Z
updated_at: 2025-02-16T18:06:56Z
url: https://github.com/astral-sh/ruff/pull/16190
synced_at: 2026-01-10T19:57:22Z
```

# Add doc about usedforsecurity flag for S324

---

_Pull request opened by @Skylion007 on 2025-02-16 16:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Provides documentation about the FIPS compliant flag for Python hashlib `usedforsecurity`
Fixes #16188 

## Test Plan

<!-- How was it tested? -->
* pre-commit hooks

---

_Comment by @github-actions[bot] on 2025-02-16 16:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-02-16 17:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/hashlib_insecure_hash_functions.rs`:61 on 2025-02-16 17:15_

nit: I kind of wish there were a way for us to link to this part of the documentation instead:
> Changed in version 3.9: All hashlib constructors take a keyword-only argument usedforsecurity with default value True. A false value allows the use of insecure and blocked hashing algorithms in restricted environments. False indicates that the hashing algorithm is not used in a security context, e.g. as a non-cryptographic one-way compression function.

But the section you picked *does* mention FIPS, where this doesn't, so I think it's okay.

---

_@ntBre approved on 2025-02-16 17:17_

Thanks! I double checked and we even have tests for `usedforsecurity`, so this is great to have in the docs.

---

_@ntBre reviewed on 2025-02-16 17:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/hashlib_insecure_hash_functions.rs`:46 on 2025-02-16 17:21_

On second thought, maybe we should add some more of the language from the Python docs here like 

```suggestion
/// or add `usedforsecurity=False` if the hashing algorithm is not used in a security context, e.g.
/// as a non-cryptographic one-way compression function:
```

---

_Merged by @ntBre on 2025-02-16 18:06_

---

_Closed by @ntBre on 2025-02-16 18:06_

---
