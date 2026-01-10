```yaml
number: 13397
title: "[red-knot] add support for typing_extensions.reveal_type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/typing-extensions-reveal-type
created_at: 2024-09-18T21:19:02Z
updated_at: 2024-09-19T17:42:38Z
url: https://github.com/astral-sh/ruff/pull/13397
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] add support for typing_extensions.reveal_type

---

_Pull request opened by @carljm on 2024-09-18 21:19_

Before `typing.reveal_type` existed, there was `typing_extensions.reveal_type`. We should support both.

Also adds a test to verify that we can handle aliasing of `reveal_type` to a different name.

Adds a bit of code to ensure that if we have a union of different `reveal_type` functions (e.g. a union containing both `typing_extensions.reveal_type` and `typing.reveal_type`) we still emit the reveal-type diagnostic only once. This is probably unlikely in practice, but it doesn't hurt to handle it smoothly. (It comes up now because we don't support `version_info` checks yet, so `typing_extensions.reveal_type` is actually that union.)


---

_Label `red-knot` added by @carljm on 2024-09-18 21:19_

---

_Review requested from @MichaReiser by @carljm on 2024-09-18 21:19_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-18 21:19_

---

_Comment by @github-actions[bot] on 2024-09-18 21:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:710 on 2024-09-19 01:08_

This solves the specific case, but not the general case. In general, there'll be a large number of symbols that could come from `typing` or `typing_extension`, and we won't care which. I think we should add a separate method to `FunctionType` (`FunctionType::is_typing_symbol`?) where `foo.is_typing_symbol("X")` returns `true` if the qualified name of the type of `foo` is either `typing.X` or `typing_extensions.X` (and the module search path is a standard-library search path). That will make it less likely (though of course not impossible) in the future that we'll forget to account for the possibility that the symbol might come from `typing_extensions` rather than `typing`

---

_@AlexWaygood reviewed on 2024-09-19 01:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:862 on 2024-09-19 04:17_

```suggestion
                    && matches!(module.name(), "typing" | "typing_extensions")
```

---

_@AlexWaygood approved on 2024-09-19 04:18_

---

_@AlexWaygood reviewed on 2024-09-19 04:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:862 on 2024-09-19 04:26_

oops, this should fix the build failure introduced by my previous suggestion:

```suggestion
                    && matches!(&**module.name(), "typing" | "typing_extensions")
```

---

_@AlexWaygood reviewed on 2024-09-19 04:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:862 on 2024-09-19 04:27_

or this:

```suggestion
                    && matches!(module.name().as_str(), "typing" | "typing_extensions")
```

---

_Merged by @carljm on 2024-09-19 04:39_

---

_Closed by @carljm on 2024-09-19 04:39_

---

_Branch deleted on 2024-09-19 04:39_

---

_@MichaReiser reviewed on 2024-09-19 06:14_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:783 on 2024-09-19 06:14_

I wonder if this demonstrates a more general problem where we probably don't want to emit one diagnostic for each union member if some of them are not callable. 

For example type script groups the error:

```ts
let a: number | string;

a();
```

```
This expression is not callable.
  No constituent of type 'string | number' is callable.(2349)
```

and I think that's better than having multiple diagnostics with the same range.


---

_@carljm reviewed on 2024-09-19 17:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:783 on 2024-09-19 17:42_

I understand the idea, but I'm confused by the comment, because I think the code here already implements exactly what you're asking for? We already consolidate all not-callable errors from calling a union and emit just a single diagnostic naming all the not-callable elements of the union.

The one thing we don't do is have a special error message for the case when all the union elements are not callable; in the case you show from typescript we would currently emit `Union elements string, number of type 'string | number' are not callable.`. But this additional case would be easy to add if we want it.

This wasn't implemented in this PR, it was implemented in the original PR that added union-call support: https://github.com/astral-sh/ruff/pull/13384

---
