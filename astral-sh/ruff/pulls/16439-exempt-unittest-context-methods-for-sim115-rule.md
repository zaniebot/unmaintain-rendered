```yaml
number: 16439
title: Exempt unittest context methods for SIM115 rule
type: pull_request
state: merged
author: adamchainz
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: adamchainz/sim115
created_at: 2025-02-28T11:59:26Z
updated_at: 2025-02-28T16:48:21Z
url: https://github.com/astral-sh/ruff/pull/16439
synced_at: 2026-01-12T15:55:55Z
```

# Exempt unittest context methods for SIM115 rule

---

_@adamchainz_

## Summary

Exempt calls to the three context methods in unittest, allowing some false positives where identical method names are used.

Closes #16438.

## Test Plan

Added test cases to Python file, updated snapshots.


---

_Comment by @github-actions[bot] on 2025-02-28 12:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:130 on 2025-02-28 14:22_

```suggestion
    if !matches!(attr, "enterClassContext" | "enterContext" | "enterAsyncContext") {
```

But also, isn't this check redundant since it's also checked on the last line?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:138 on 2025-02-28 14:24_

```suggestion
    matches!(
        (id, attr),
        ("cls", "enterClassContext") | ("self", "enterContext" | "enterAsyncContext")
    )
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:79 on 2025-02-28 14:25_

would you mind moving these (and the new import) to the bottom of the fixture file, then regenerating the snapshot? Adding them to the middle results in a _lot_ of line numbers changing in the snapshot, which makes it very hard to review, unfortunately :-(

---

_@AlexWaygood reviewed on 2025-02-28 14:26_

Thank you!

---

_@adamchainz reviewed on 2025-02-28 15:48_

---

_Review comment by @adamchainz on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:130 on 2025-02-28 15:48_

Aye, that it is. Removing.

---

_@adamchainz reviewed on 2025-02-28 15:52_

---

_Review comment by @adamchainz on `crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs`:138 on 2025-02-28 15:52_

Thanks, that’s much better. Still a Rust noob here.

---

_Review comment by @adamchainz on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM115.py`:79 on 2025-02-28 15:52_

Done. Wasn't sure of the convention but that makes sense.

---

_@adamchainz reviewed on 2025-02-28 15:52_

---

_Label `bug` added by @AlexWaygood on 2025-02-28 16:24_

---

_Label `rule` added by @AlexWaygood on 2025-02-28 16:24_

---

_@AlexWaygood approved on 2025-02-28 16:29_

Thank you!

---

_Merged by @AlexWaygood on 2025-02-28 16:29_

---

_Closed by @AlexWaygood on 2025-02-28 16:29_

---

_Branch deleted on 2025-02-28 16:48_

---
