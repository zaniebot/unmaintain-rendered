```yaml
number: 16052
title: "[`flake8-type-checking`] Avoid `TC004` false positive where the runtime definition is provided by `__getattr__`"
type: pull_request
state: merged
author: Daverball
labels:
  - bug
assignees: []
merged: true
base: main
head: bugfix/tc004-module-level-dunder-getattr
created_at: 2025-02-09T13:42:29Z
updated_at: 2025-02-09T16:30:03Z
url: https://github.com/astral-sh/ruff/pull/16052
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-type-checking`] Avoid `TC004` false positive where the runtime definition is provided by `__getattr__`

---

_@Daverball_

Fixes #16045

## Summary

Module-level `__getattr__` is too dynamic for us to understand, so if our only runtime reference to a typing only import happens inside `__all__`, we should assume a module-level `__getattr__` will provide a runtime accessible fallback, since we can't prove that it doesn't.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-02-09 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-02-09 16:09_

Just to double-check I understand the current behaviour of this rule correctly. This kind of thing currently triggers `TC004`, because the _only_ definition of `Any` is in a `TYPE_CHECKING` block, but `Any` is used at runtime:

```py
import typing

if typing.TYPE_CHECKING:
    from typing import Any

X: typing.TypeAlias = Any
```

But this kind of thing does _not_ trigger `TC004`, because it is _also_ defined in a non-`TYPE_CHECKING` branch:

```py
import typing

if typing.TYPE_CHECKING:
    from typing import Any
else:
    from typing import Any

X: typing.TypeAlias = Any
```

Is that correct? If so, it might be good to adjust the rule's documentation to clarify this. The behaviour makes sense to me, but I assumed from the docs that all definitions in `TYPE_CHECKING` blocks were prohibited if the definition was used at runtime; it wasn't clear to me that conditional declarations like this (where there were also runtime definitions alongside the `TYPE_CHECKING` definitions) were also permitted.

Something like this might do the trick:

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs b/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs
index 3fe243cdc..d5fea43cb 100644
--- a/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs
+++ b/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs
@@ -16,11 +16,12 @@ use crate::rules::flake8_type_checking::helpers::{filter_contained, quote_annota
 use crate::rules::flake8_type_checking::imports::ImportBinding;
 
 /// ## What it does
-/// Checks for runtime imports defined in a type-checking block.
+/// Checks for imports that are required at runtime but are only defined in
+/// type-checking blocks.
 ///
 /// ## Why is this bad?
-/// The type-checking block is not executed at runtime, so the import will not
-/// be available at runtime.
+/// The type-checking block is not executed at runtime, so if the only definition
+/// of a symbol is in a type-checking block, it will not be available at runtime.
 ///
 /// If [`lint.flake8-type-checking.quote-annotations`] is set to `true`,
 /// annotations will be wrapped in quotes if doing so would enable the
```

---

_@AlexWaygood approved on 2025-02-09 16:09_

LGTM other than my suggested improvement to the docs

---

_Comment by @Daverball on 2025-02-09 16:24_

@AlexWaygood Correct, I updated the docs accordingly

---

_Comment by @AlexWaygood on 2025-02-09 16:24_

Thank you!

---

_Label `bug` added by @AlexWaygood on 2025-02-09 16:24_

---

_Renamed from "[`flake8-type-checking`] Avoid `TC004` false positive with `__getattr__`" to "[`flake8-type-checking`] Avoid `TC004` false positive where the runtime definition is provided by `__getattr__`" by @AlexWaygood on 2025-02-09 16:25_

---

_Merged by @AlexWaygood on 2025-02-09 16:27_

---

_Closed by @AlexWaygood on 2025-02-09 16:27_

---

_Branch deleted on 2025-02-09 16:27_

---
