```yaml
number: 18077
title: "[ty] Check assignments to implicit global symbols are assignable to the types declared on `types.ModuleType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/moduletype-declarations
created_at: 2025-05-13T19:28:58Z
updated_at: 2025-05-13T20:37:23Z
url: https://github.com/astral-sh/ruff/pull/18077
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Check assignments to implicit global symbols are assignable to the types declared on `types.ModuleType`

---

_@AlexWaygood_

## Summary

Currently we'll happily let you do this without complaining about it:

```py
__doc__ = 42
```

But this is unsound! All modules are instances of `types.ModuleType`, and typeshed states that all `types.ModuleType` instances have a `__doc__` attribute of type `str | None`. `Literal[42]` is not assignable to `str | None`, so we should not permit this.

This PR fixes this soundness hole by falling back to declarations on `types.ModuleType` when checking assignments in the global scope. Cc. @tekknolagi (cf. https://github.com/astral-sh/ruff/pull/18071#issuecomment-2877485872).

I also fix an issue in this PR where we would incorrectly report a symbol as being unbound if it is declared as global in an inner scope and it is an implicit `ModuleType` global in the global scope, e.g.

```py
def f():
    global __loader__
    print(__loader__)  # we previously emitted `[unresolved-reference]` here
```

## Test Plan

mdtests added


---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 19:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-13 19:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-13 19:28_

---

_Label `bug` added by @AlexWaygood on 2025-05-13 19:28_

---

_Label `ty` added by @AlexWaygood on 2025-05-13 19:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2025-05-13 19:35_

this is also unsound, I suppose? I think it would need a fix in a different area of the code, though (we just wouldn't permit the redeclaration if it's an implicit `ModuleType` attribute in the global scope and it's not assignable to the type on `ModuleType`)

---

_@AlexWaygood reviewed on 2025-05-13 19:35_

---

_Comment by @github-actions[bot] on 2025-05-13 19:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2025-05-13 19:39_

Yes, was just commenting on this. I think we could probably just forbid re-declaring these? (I also don't think this is a priority that we should spend time on now, it's not likely to impact many users. We should probably add a TODO comment for it, though.)

---

_@AlexWaygood reviewed on 2025-05-13 19:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2025-05-13 19:44_

(I'll add a TODO and do this as a followup if we agree)

---

_@carljm approved on 2025-05-13 19:52_

Looks good!

---

_@AlexWaygood reviewed on 2025-05-13 20:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:54 on 2025-05-13 20:29_

It's not that hard, so I ended up just fixing this rather than adding a TODO. See https://github.com/astral-sh/ruff/pull/18077/commits/e9b73f001f65e92908e5b4a5990f1e8c26fa6016

---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 20:29_

---

_Comment by @AlexWaygood on 2025-05-13 20:29_

(Re-requesting review just for the changes pushed in https://github.com/astral-sh/ruff/pull/18077/commits/e9b73f001f65e92908e5b4a5990f1e8c26fa6016)

---

_@carljm approved on 2025-05-13 20:36_

---

_Merged by @AlexWaygood on 2025-05-13 20:37_

---

_Closed by @AlexWaygood on 2025-05-13 20:37_

---

_Branch deleted on 2025-05-13 20:37_

---
