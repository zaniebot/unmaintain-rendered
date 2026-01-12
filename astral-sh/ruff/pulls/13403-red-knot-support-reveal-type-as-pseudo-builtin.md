```yaml
number: 13403
title: "[red-knot] support reveal_type as pseudo-builtin"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/builtin-reveal-type
created_at: 2024-09-19T00:07:20Z
updated_at: 2024-09-19T14:58:10Z
url: https://github.com/astral-sh/ruff/pull/13403
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] support reveal_type as pseudo-builtin

---

_@carljm_

Support using `reveal_type` without importing it, as implied by the type spec and supported by existing type checkers.

We use `typing_extensions.reveal_type` for the implicit built-in; this way it exists on all Python versions. (It imports from `typing` on newer Python versions.)

Emits an "undefined name" diagnostic whenever `reveal_type` is referenced in this way (in addition to the revealed-type diagnostic when it is called). This follows the mypy example (with `--enable-error-code unimported-reveal`) and I think provides a good (and easily understandable) balance for user experience. If you are using `reveal_type` for quick temporary debugging, the additional undefined-name diagnostic doesn't hinder that use case. If we make the revealed-type diagnostic a non-failing one, the undefined-name diagnostic can still be a failing diagnostic, helping prevent accidentally leaving it in place. For any use cases where you want to leave it in place, you can always import it to avoid the undefined-name diagnostic.

In the future, we can easily provide configuration options to a) turn off builtin-reveal_type altogether, and/or b) silence the undefined-name diagnostic when using it, if we have users on either side (loving or hating pseudo-builtin `reveal_type`) who are dissatisfied with this compromise.


---

_Label `red-knot` added by @carljm on 2024-09-19 00:07_

---

_Review requested from @MichaReiser by @carljm on 2024-09-19 00:07_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-19 00:07_

---

_Comment by @github-actions[bot] on 2024-09-19 00:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-19 06:10_

> Emits an "undefined name" diagnostic whenever reveal_type is referenced in this way (in addition to the revealed-type diagnostic when it is called). 

IMO, this is fairly confusing because it has obviously been picked up by the type checker. I think this should be its own diagnostic that provides additional context on why it is reported as an error. 

There's another downside to this approach that I'm not yet able to fully answer but it does impose restrictions on how we can sort diagnostics because you probably want that the `reveal_type` diagnostic comes before the `undefined` diagnostic. 

Let's say the file has a bunch of typing errors and the user uses `reveal_type` to debug what's going on. Knot sorts the diagnostics by file, severity (errors first), and location. That means that the reveal type comes at the very bottom and the user might only see the errors and be confused why `reveal_type` is undefined.

---

_Comment by @carljm on 2024-09-19 13:58_

> I think this should be its own diagnostic that provides additional context on why it is reported as an error.

I agree, I'll update the diagnostic to provide more context. Mypy does this too.

> you probably want that the `reveal_type` diagnostic comes before the `undefined` diagnostic.

No, in general it will always be the other way around. They apply to different locations. The undefined diagnostic applies to the location where you reference the name `return_type`; the reveal diagnostic applies to where you call it (probably even more specifically to the first argument of the call.) So the location of the undefined diagnostic will always be before the location of the reveal diagnostic. In the extreme (though unlikely in practical usage) case, they could be arbitrarily far apart, if you do `rt = reveal_type` in one place and then `rt(some_val)` later. I think this is quite intuitive and as it should be.

> Knot sorts the diagnostics by file, severity (errors first), and location.

I hope we don't ever do this, I would find it very confusing as a user. Error output on CLI should be sorted by location in file, so that related things are together. Sorting by severity is making unjustified assumptions about what is more visible, and what the user wants to see, and just makes it harder to find the relevant output.

> reveal type comes at the very bottom and the user might only see the errors

There's an assumption here that earlier output is more likely to be seen. But this is not true for CLI usage, at least not for me. If there's a lot of output, the _last_ output is what I'm most likely to see, not the first.

> and be confused why `reveal_type` is undefined

I don't think this is an issue if the diagnostic clarifies the situation. More specifically, I don't think seeing the reveal diagnostic helps to clarify why `reveal_type` is undefined, so I don't think it matters; the undefined diagnostic needs to be responsible to clarify itself anyway.

---

_@MichaReiser approved on 2024-09-19 14:40_

Thanks for updating the diagnostic.

---

_Merged by @carljm on 2024-09-19 14:58_

---

_Closed by @carljm on 2024-09-19 14:58_

---

_Branch deleted on 2024-09-19 14:58_

---
