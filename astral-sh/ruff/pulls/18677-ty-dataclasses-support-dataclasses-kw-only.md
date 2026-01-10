```yaml
number: 18677
title: "[ty] dataclasses: Support dataclasses.KW_ONLY"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - ty
assignees: []
merged: true
base: main
head: dataclass
created_at: 2025-06-14T18:25:05Z
updated_at: 2025-06-16T17:48:02Z
url: https://github.com/astral-sh/ruff/pull/18677
synced_at: 2026-01-10T18:45:04Z
```

# [ty] dataclasses: Support dataclasses.KW_ONLY

---

_Pull request opened by @abhijeetbodas2001 on 2025-06-14 18:25_

## Summary

Add support for `dataclasses.KW_ONLY` as specified in https://docs.python.org/3/library/dataclasses.html#dataclasses.KW_ONLY

Part of https://github.com/astral-sh/ty/issues/111

## Test Plan

More mdtests.


---

_Comment by @github-actions[bot] on 2025-06-14 18:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-06-14 21:06_

---

_Comment by @abhijeetbodas2001 on 2025-06-16 06:52_

There's one TODO here for detecting more than one field annotated with `KW_ONLY` (and then emitting a diagnostic for that, since the spec says that's not allowed), which I'm yet to figure out how to setup.

But the main functionality is now working, so I'd appreciate a review on it. But please also see the question at the end of https://github.com/astral-sh/ty/issues/111#issuecomment-2972874504

---

_Marked ready for review by @abhijeetbodas2001 on 2025-06-16 06:52_

---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-06-16 06:52_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-06-16 06:52_

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-06-16 06:52_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-06-16 06:52_

---

_@AlexWaygood approved on 2025-06-16 17:23_

Thanks, this is great!

---

_Merged by @AlexWaygood on 2025-06-16 17:27_

---

_Closed by @AlexWaygood on 2025-06-16 17:27_

---

_@abhijeetbodas2001 reviewed on 2025-06-16 17:40_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types.rs`:603 on 2025-06-16 17:40_

Do you prefer not having a function since there's only one caller?

---

_@AlexWaygood reviewed on 2025-06-16 17:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:603 on 2025-06-16 17:44_

yep. If we find other places in the codebase that need this in the future, I think we could consider adding a `Type` method, but for now it seems likely to me that the places we'll need this information are quite localised to one specific area of the codebase :-)

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types.rs`:603 on 2025-06-16 17:48_

Got it, that makes sense, thanks!

---

_@abhijeetbodas2001 reviewed on 2025-06-16 17:48_

---
