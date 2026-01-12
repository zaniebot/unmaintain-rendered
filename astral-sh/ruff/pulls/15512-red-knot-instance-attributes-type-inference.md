```yaml
number: 15512
title: "[red-knot] Instance attributes: type inference clarifications"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/instance-attributes-clarify-inference
created_at: 2025-01-15T19:43:57Z
updated_at: 2025-01-15T20:17:57Z
url: https://github.com/astral-sh/ruff/pull/15512
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Instance attributes: type inference clarifications

---

_@sharkdp_

## Summary

Some clarifications in the instance-attributes tests, mostly regarding type inference behavior following [this discussion](https://github.com/astral-sh/ruff/pull/15474#discussion_r1917044566)


---

_Label `red-knot` added by @sharkdp on 2025-01-15 19:43_

---

_Review requested from @carljm by @sharkdp on 2025-01-15 19:43_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-15 19:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-15 19:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 19:47_

```suggestion
# assignment and the use here), but pyright and mypy support this, presumably to provide features
```

I can't remember where pyre ended up on this; @carljm can refresh my memory here :-) I believe they started off being very strict about soundness here, but it resulted in a lot of user confusion and not a significant increase in type safety, so then loosened their rules somewhat. But I don't know exactly where they ended up at the end of the day. When we were chatting to the pyre developers at PyCon, my recollection is that they said they regretted ever going down that road, because users had such a strong expectation of type narrowing in code like this.

---

_@AlexWaygood approved on 2025-01-15 19:49_

---

_Comment by @github-actions[bot] on 2025-01-15 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2025-01-15 19:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 19:57_

I think (and Pyre playground testing just now confirmed) Pyre does the narrowing but then reverts it after an expression that seems likely to execute arbitrary code elsewhere (e.g. a call expression). This is also very much a heuristic, and one that results in behavior that is surprising to users ("why did adding this call that doesn't involve variable `c_instance` suddenly change the inferred type of `c_instance.pure_instance_variable4`??"), so I'm not particularly fond of that approach either.

I agree that we will probably just want to follow mypy and pyright here.

---

_@carljm reviewed on 2025-01-15 19:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:77 on 2025-01-15 19:58_

```suggestion
# assignment and the use here), but pyright and mypy support this.
# In conclusion, this could be `bool` but should probably be `Literal[False]`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:122 on 2025-01-15 19:58_

```suggestion
# Not that we would use this in static analysis, but for a more realistic example, let's actually
```

---

_@carljm approved on 2025-01-15 19:59_

---

_@sharkdp reviewed on 2025-01-15 20:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 20:03_

@AlexWaygood In which sense does mypy support this? I don't see a narrowed type here: https://mypy-play.net/?mypy=latest&python=3.13&gist=4cc6d2748f364c614965b72becf538e7

Do I need to use an example that does not involve literals?

---

_@sharkdp reviewed on 2025-01-15 20:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 20:04_

> Do I need to use an example that does not involve literals?

Yes. Yes, I do. For unions, it works.

---

_@carljm reviewed on 2025-01-15 20:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 20:05_

Yes, mypy is very sparing in its willingness to infer literals. If you try an optional type like `int | None` for an attribute, then assign `3` to it, mypy will narrow to `int`.

---

_@AlexWaygood reviewed on 2025-01-15 20:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:75 on 2025-01-15 20:05_

Yeah, mypy never narrows instance types to literal types, but you can see that it narrows the types of attributes in exactly the same way it allows you narrow the types of local variables https://mypy-play.net/?mypy=latest&python=3.13&gist=c218144f43fd07fd2a2c93c142471b8f

---

_Merged by @sharkdp on 2025-01-15 20:17_

---

_Closed by @sharkdp on 2025-01-15 20:17_

---

_Branch deleted on 2025-01-15 20:17_

---
