```yaml
number: 16827
title: "[red-knot] Refactor `property_tests.rs` into `property_tests` module structure"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: cake/modulize-property-tests
created_at: 2025-03-18T10:02:55Z
updated_at: 2025-03-18T12:59:14Z
url: https://github.com/astral-sh/ruff/pull/16827
synced_at: 2026-01-12T15:55:58Z
```

# [red-knot] Refactor `property_tests.rs` into `property_tests` module structure

---

_@cake-monotone_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

For now, `property_tests.rs` has grown larger and larger, making the file difficult to read and maintain.  

Although the code has been split, the test paths and full names remain unchanged. There are no changes affecting test execution.


---

_Review requested from @carljm by @cake-monotone on 2025-03-18 10:02_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-03-18 10:02_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-18 10:02_

---

_Review requested from @dcreager by @cake-monotone on 2025-03-18 10:02_

---

_Comment by @github-actions[bot] on 2025-03-18 10:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests/tys.rs`:1 on 2025-03-18 11:05_

I think I'd be inclined to either:
- Rename this to something more descriptive like `type_generation.rs` or
- Rename it to something more general like `setup.rs` and combine the code in the `db.rs` submodule into this module

---

_@AlexWaygood reviewed on 2025-03-18 11:05_

Nice, I hadn't realised how large this file had become

---

_Label `red-knot` added by @MichaReiser on 2025-03-18 12:48_

---

_@cake-monotone reviewed on 2025-03-18 12:51_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/property_tests/tys.rs`:1 on 2025-03-18 12:51_

Sounds great! I couldn't come up with a better name than the one you suggested.

---

_@AlexWaygood approved on 2025-03-18 12:58_

---

_Merged by @AlexWaygood on 2025-03-18 12:59_

---

_Closed by @AlexWaygood on 2025-03-18 12:59_

---
