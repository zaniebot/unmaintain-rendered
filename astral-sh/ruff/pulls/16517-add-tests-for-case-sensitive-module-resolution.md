```yaml
number: 16517
title: Add tests for case-sensitive module resolution
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/case-sensitive-module-resolver-tests
created_at: 2025-03-05T12:42:15Z
updated_at: 2025-03-06T09:19:25Z
url: https://github.com/astral-sh/ruff/pull/16517
synced_at: 2026-01-12T15:55:55Z
```

# Add tests for case-sensitive module resolution

---

_@MichaReiser_

## Summary

Python's module resolver is case sensitive. 

This PR adds mdtests that assert that our module resolution is case sensitive. 

The tests currently all pass because our in memory file system is case sensitive. 
I'll add support for using the real file system to the mdtest framework in a separate PR.

This PR also adds support for specifying extra search paths to the mdtest framework.

## Test Plan
The tests fail when running them using the real file system.


---

_Review requested from @carljm by @MichaReiser on 2025-03-05 12:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-05 12:42_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-05 12:42_

---

_Label `red-knot` added by @MichaReiser on 2025-03-05 12:42_

---

_Converted to draft by @MichaReiser on 2025-03-05 12:47_

---

_Marked ready for review by @MichaReiser on 2025-03-05 12:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/case_sensitive.md`:16 on 2025-03-05 17:26_

nit: I think using a class named `A` in these tests is confusing for no benefit, when it could be named `C` or anything else

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/case_sensitive.md`:111 on 2025-03-05 17:30_

```suggestion
## Incorrect extension casing
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/case_sensitive.md`:123 on 2025-03-05 17:31_

I don't love having our mdtests start to use an essentially-random mix of `py` vs `python`, but IIRC your editor makes it hard for you to use `py`?

---

_@carljm approved on 2025-03-05 17:33_

---

_@AlexWaygood reviewed on 2025-03-05 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:122 on 2025-03-05 17:42_

I think the assertion message may now need to be updated to mention `python`? Which means we may also want to change the message so it talks about "language tags" rather than "file types"

---

_@MichaReiser reviewed on 2025-03-05 17:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/import/case_sensitive.md`:123 on 2025-03-05 17:43_

RustRover always suggests `python` and not `py`. It's also hard-coded in my brain because that's what I used consistently everywhere before red knot (there are more than 103 matches for ```python in our code base). 

---

_Merged by @MichaReiser on 2025-03-06 09:19_

---

_Closed by @MichaReiser on 2025-03-06 09:19_

---

_Branch deleted on 2025-03-06 09:19_

---
