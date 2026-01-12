```yaml
number: 17116
title: "[red-knot] Don't infer Todo for quite so many tuple type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-tuples
created_at: 2025-04-01T12:13:25Z
updated_at: 2025-04-01T14:44:05Z
url: https://github.com/astral-sh/ruff/pull/17116
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Don't infer Todo for quite so many tuple type expressions

---

_@AlexWaygood_

## Summary

I noticed we were inferring `Todo` as the declared type for annotations such as `x: tuple[list[int], list[int]]`. This PR reworks our annotation parsing so that we instead infer `tuple[Todo, Todo]` for this annotation, which is quite a bit more precise.

## Test Plan

Existing mdtest updated.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-01 12:13_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-01 12:13_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-01 12:13_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-01 12:13_

---

_Comment by @github-actions[bot] on 2025-04-01 12:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/pgen.py:187:16: Object of type `tuple[dict, None | Unknown]` is not assignable to return type `tuple[@Todo(generics), str]`
- Found 211 diagnostics
+ Found 212 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-04-01 12:20_

link to the primer hit: https://github.com/psf/black/blob/2c135edf3732a8efcc89450446fcaa7589e2a1c8/src/blib2to3/pgen2/pgen.py#L164-L187

I think it's probably a false positive, but a single false positive is probably worth it here IMO. This gives us better understanding for a lot of `tuple` annotations that we were giving up on parsing before.

---

_@dcreager approved on 2025-04-01 14:39_

> I think it's probably a false positive, but a single false positive is probably worth it here IMO. This gives us better understanding for a lot of `tuple` annotations that we were giving up on parsing before.

There's an assert at the end of that function that should narrow away the `None` from the second element of the return tuple. If it's quick to figure out why that's happening, I think it would be worth tackling here. But if it's unrelated to the tuple change (i.e. do we also not narrow `None | Unknown` to `Unknown` correctly on its own?) then I agree that the one false positive is worth it here.

---

_Comment by @AlexWaygood on 2025-04-01 14:43_

Ah, we don't do type narrowing for `assert` statements yet! So that explains it.

---

_Merged by @AlexWaygood on 2025-04-01 14:44_

---

_Closed by @AlexWaygood on 2025-04-01 14:44_

---

_Branch deleted on 2025-04-01 14:44_

---
