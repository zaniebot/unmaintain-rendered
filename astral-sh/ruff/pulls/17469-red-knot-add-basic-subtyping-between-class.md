```yaml
number: 17469
title: "[red-knot] Add basic subtyping between class literal and callable"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: subtyping-class-literal-callable
created_at: 2025-04-18T18:38:06Z
updated_at: 2025-04-25T20:55:11Z
url: https://github.com/astral-sh/ruff/pull/17469
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add basic subtyping between class literal and callable

---

_Pull request opened by @MatthewMckee4 on 2025-04-18 18:38_

## Summary

This covers step 1 from https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable

Part of astral-sh/ty#129

## Test Plan

Update is_subtype_of.md and is_assignable_to.md

---

_Comment by @github-actions[bot] on 2025-04-18 18:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-04-18 18:43_

To note: currently we don't emit an error for this
```py
class A:
    def __new__(cls) -> A:
        return object.__new__(cls)

    def __init__(self, x: int) -> None:
        self.x = x
```



---

_Comment by @MatthewMckee4 on 2025-04-18 19:46_

I'm not sure this will have a great impact without our understanding of `Self` 

---

_Label `red-knot` added by @ntBre on 2025-04-18 21:02_

---

_Marked ready for review by @MatthewMckee4 on 2025-04-21 16:09_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-21 16:09_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-21 16:09_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-21 16:09_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-21 16:09_

---

_Converted to draft by @MatthewMckee4 on 2025-04-21 16:16_

---

_Marked ready for review by @MatthewMckee4 on 2025-04-21 16:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1145 on 2025-04-21 17:38_

I agree that this should not error, but for a different reason -- I think (as discussed in Discord) that the text in the spec about ignoring certain metaclass `__call__` methods for purposes of converting a constructor to a callable is making unwarranted assumptions that can lead to false positive errors. I think it is more correct to always respect the signature of the metaclass `__call__` method.

If this causes problems in practice (including with the spec conformance suite, and we can't effectuate an update to the spec), we can consider a future change. But for now I think we should unconditionally use metaclass `__call__` if present (which I think will make this test pass as written and remove the need for the TODO comment?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1168 on 2025-04-21 17:41_

I'm not sure it makes sense to include this test at all, and if we do, it should not behave as asserted here.

The `Self` annotation above is in the context of `class MetaWithSelfReturn`, so it is requiring that the `__call__` method return an instance of `MetaWithSelfReturn`, not an instance of `C`. So `C` should not be a subtype of any callable type returning `C`.

I think we should just omit this test for now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1142 on 2025-04-21 17:44_

As discussed above, I think for now let's just always use the metaclass `__call__` signature, if present, and add a comment `// TODO: this intentionally diverges from step 1 in https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable, which makes unwarranted assumptions`

---

_@carljm reviewed on 2025-04-21 17:44_

Thank you!

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1168 on 2025-04-21 21:38_

Ah yes my bad, thanks

---

_@MatthewMckee4 reviewed on 2025-04-21 21:38_

---

_@carljm approved on 2025-04-21 22:26_

Looks great, thank you!

---

_Merged by @carljm on 2025-04-21 22:29_

---

_Closed by @carljm on 2025-04-21 22:29_

---

_Branch deleted on 2025-04-25 20:55_

---
