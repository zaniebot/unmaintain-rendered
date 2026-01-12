```yaml
number: 14002
title: Add failing tests for augmented assignments with partial binding
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/union-tests
created_at: 2024-10-30T17:26:53Z
updated_at: 2024-10-30T18:22:36Z
url: https://github.com/astral-sh/ruff/pull/14002
synced_at: 2026-01-12T15:55:46Z
```

# Add failing tests for augmented assignments with partial binding

---

_@charliermarsh_

## Summary

These cases aren't handled correctly yet -- some of them are waiting on refactors to `Unbound` before fixing. Part of #12699.

---

_Marked ready for review by @charliermarsh on 2024-10-30 17:33_

---

_Review requested from @carljm by @charliermarsh on 2024-10-30 17:33_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-30 17:33_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-30 17:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:86 on 2024-10-30 17:41_

```suggestion
# TODO should emit a diagnostic warning that `Foo` might not have an `__iadd__` method
reveal_type(f)  # revealed: int
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:88 on 2024-10-30 17:42_

nit

```suggestion
## Partially bound with `__add__`
```

---

_Label `red-knot` added by @MichaReiser on 2024-10-30 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:108 on 2024-10-30 17:42_

nit

```suggestion
## Partially bound target union
```

---

_@AlexWaygood approved on 2024-10-30 17:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:95 on 2024-10-30 18:01_

I think `other: int` is wrong for what you're intending to test here, since it would actually mean that the binary fallback should also fail and `str` would not be possible.
```suggestion
    def __add__(self, other: str) -> str:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:118 on 2024-10-30 18:02_

In this case I think you need `other: int` here for `int` to be part of the result union?
```suggestion
        def __iadd__(self, other: int) -> int:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:149 on 2024-10-30 18:04_

Shouldn't this be `str | float`?

Fixing this doesn't have a dependency on removing `Type::Unbound`, right?

---

_@carljm approved on 2024-10-30 18:10_

Looks good, thank you! A few comments that should be addressed before landing (unless I missed something.)

A couple other cases also occurred to me looking at this, I added a checklist to https://github.com/astral-sh/ruff/issues/12699 of things I'm aware of right now that are to-be-done here.

---

_@charliermarsh reviewed on 2024-10-30 18:18_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:95 on 2024-10-30 18:18_

Sorry, yes, you're right. (Wouldn't be enforced today IIUC but yes agree.)

---

_@charliermarsh reviewed on 2024-10-30 18:19_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:149 on 2024-10-30 18:19_

Yeah, I think this is a TODO in binary operation handling, but... I included it anyway.

---

_@charliermarsh reviewed on 2024-10-30 18:19_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:149 on 2024-10-30 18:19_

```suggestion
# TODO(charlie): This should be `str | float`.
```

---

_Merged by @charliermarsh on 2024-10-30 18:22_

---

_Closed by @charliermarsh on 2024-10-30 18:22_

---

_Branch deleted on 2024-10-30 18:22_

---
