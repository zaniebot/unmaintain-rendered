```yaml
number: 20002
title: "[ty] correctly ignore field specifiers when not specified"
type: pull_request
state: merged
author: leandrobbraga
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: issue-890
created_at: 2025-08-20T12:44:06Z
updated_at: 2025-08-20T19:01:18Z
url: https://github.com/astral-sh/ruff/pull/20002
synced_at: 2026-01-10T17:46:21Z
```

# [ty] correctly ignore field specifiers when not specified

---

_Pull request opened by @leandrobbraga on 2025-08-20 12:44_

This commit corrects the type checker's behavior when handling `dataclass_transform` decorators that don't explicitly specify `field_specifiers`. According to [PEP 681 (Data Class Transforms)](https://peps.python.org/pep-0681/#dataclass-transform-parameters), when `field_specifiers` is not provided, it defaults to an empty tuple, meaning no field specifiers are supported and `dataclasses.field`/`dataclasses.Field` calls should be ignored.

Fixes https://github.com/astral-sh/ty/issues/980


---

_Review requested from @carljm by @leandrobbraga on 2025-08-20 12:44_

---

_Review requested from @AlexWaygood by @leandrobbraga on 2025-08-20 12:44_

---

_Review requested from @sharkdp by @leandrobbraga on 2025-08-20 12:44_

---

_Review requested from @dcreager by @leandrobbraga on 2025-08-20 12:44_

---

_Comment by @github-actions[bot] on 2025-08-20 12:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 12:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-20 13:08_

---

_Label `bug` added by @AlexWaygood on 2025-08-20 13:08_

---

_@carljm approved on 2025-08-20 18:17_

This looks good to me, thank you!

Clearly when we add full `field_specifiers` support, we will probably replace/remove this simple boolean flag. But this improves the current behavior, and doesn't make the future changes more difficult.

@sharkdp are you okay with this temporary fix?

---

_Comment by @carljm on 2025-08-20 18:33_

Going to go ahead and merge this, it's a small PR, there's nothing here that we can't easily change again.

Thank you for the fix!

---

_Merged by @carljm on 2025-08-20 18:33_

---

_Closed by @carljm on 2025-08-20 18:33_

---

_Comment by @sharkdp on 2025-08-20 19:01_

> @sharkdp are you okay with this temporary fix?

Yes, of course. Thank you for this contribution!

---
