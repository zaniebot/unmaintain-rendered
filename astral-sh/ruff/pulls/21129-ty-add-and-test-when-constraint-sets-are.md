```yaml
number: 21129
title: "[ty] Add and test when constraint sets are satisfied by their typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/satisfies
created_at: 2025-10-29T18:45:56Z
updated_at: 2025-10-31T14:53:39Z
url: https://github.com/astral-sh/ruff/pull/21129
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Add and test when constraint sets are satisfied by their typevars

---

_@dcreager_

This PR adds a new `satisfied_by_all_typevar` method, which implements one of the final steps of actually using these dang constraint sets. Constraint sets exist to help us check assignability and subtyping of types in the presence of typevars. We construct a constraint set describing the conditions under which assignability holds between the two types. Then we check whether that constraint set is satisfied for the valid specializations of the relevant typevars (which is this new method).

We also add a new `ty_extensions.ConstraintSet` method so that we can test this method's behavior in mdtests, before hooking it up to the rest of the specialization inference machinery.

---

_Label `internal` added by @dcreager on 2025-10-29 18:45_

---

_Label `ty` added by @dcreager on 2025-10-29 18:45_

---

_Comment by @github-actions[bot] on 2025-10-29 18:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-29 18:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @dcreager on 2025-10-29 20:13_

---

_Review requested from @carljm by @dcreager on 2025-10-29 20:13_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-29 20:13_

---

_Review requested from @sharkdp by @dcreager on 2025-10-29 20:13_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-29 20:13_

---

_@MichaReiser reviewed on 2025-10-29 21:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:58 on 2025-10-29 21:22_

I don't think I understand why `T <= Super` satisfies `T <= Base`, given that `Super` isn't a subtype of `Base`. Or am I reading this the wrong way around? Or is the `Never` part important?

---

_@MichaReiser reviewed on 2025-10-29 21:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:62 on 2025-10-29 21:23_

I don't think I understand the why but I trust you on this

---

_@MichaReiser reviewed on 2025-10-29 21:25_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:80 on 2025-10-29 21:25_

Hmm, same here. Why is `Super` satisfying the constraint, given that it isn't `Base` or `Unrelated`?

---

_@MichaReiser reviewed on 2025-10-29 21:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:110 on 2025-10-29 21:32_

Gosh... it took me way too long to notice the difference to the unbounded example on line 40... We don't pass any type var here...

Should we use a different method name for this? It seems very subtle, but maybe it's obvious for someone more familiar with all this

---

_Review comment by @MichaReiser on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:78 on 2025-10-29 21:33_

I'd find it helpful to document the behavior when `inferable` is empty. Naively, I'd assume it always returns `true` or `false` but the actual behavior seems to be different?

---

_@MichaReiser reviewed on 2025-10-29 21:33_

---

_@dcreager reviewed on 2025-10-30 18:15_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:110 on 2025-10-30 18:15_

I can make it a keyword-only parameter, which I think will call out the difference more clearly

---

_@dcreager reviewed on 2025-10-30 19:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:58 on 2025-10-30 19:16_

We discussed this sync — the key point is that the implication is in the other direction. (The typevar bounds/constraints need to imply the constraint set being checked.) So here, `T` must specialize to a subtype of `Base`. `Base` is a subtype of `Super`, so every valid specialization of `T` is also a subtype of `Super`. (Technically, this test is in the inferable section, so we only need _one_ valid specialization of `T` to satisfy the constraint set. In this case, _every_ valid specialization does, so you'll see that this same test also holds down in the non-inferable section.)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:62 on 2025-10-30 19:20_

This is trying to show that `Never` is sneaky, especially for inferable typevars. Since `T` is inferable, we only need _one_ specialization to satisfy the constraint set. `Never` is a valid specialization, since `Never ≤ Base`. And `Never ≤ Unrelated`, so the constraint set is satisfied for the `T = Never` case. That's enough for an inferable typevar.

The corresponding test in the non-inferable section fails, though, since there we need _all_ valid specializations to satisfy the constraint set. And `T = Base` is a counter-example where it doesn't.

This tells me that it might help to change the structure of this file? Right now I have inferable/non-inferable as the top-level sections, and different kinds of typevar bounds/constraints as (unlabeled) subsections. Maybe instead I should have "unbounded", "bound", and "constrained" as the top-level sections, to more clearly call out how the behavior is different for inferable vs non-inferable?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:80 on 2025-10-30 19:21_

Same as above. This is in inferable position, so we only need one specialization of `T` to satisfy the constraint set. `T = Base` does, so the check passes. Down below in the non-inferable section, the check fails, because `T = Unrelated` is a valid specialization that doesn't satisfy the constraint set.

---

_@dcreager reviewed on 2025-10-30 19:21_

---

_@dcreager reviewed on 2025-10-30 20:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:62 on 2025-10-30 20:48_

> This tells me that it might help to change the structure of this file?

I did this, and added more explanatory comments for each example. Hopefully that helps clarify the logic a bit.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-30 20:48_

---

_@dcreager reviewed on 2025-10-30 20:49_

---

_Review comment by @dcreager on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:78 on 2025-10-30 20:49_

Done

---

_@MichaReiser reviewed on 2025-10-30 21:49_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:20 on 2025-10-30 21:49_

This is great. Thanks for adding it

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:64 on 2025-10-30 21:53_

I've a slight preference towards making `inferable` optional over this somewhat magical `()`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:175 on 2025-10-30 23:14_

I'm still somewhat confused by the DSL here but I (maybe?) finally figured out how to read this?

Is my understanding correct that this results in:

```
Never <= any(Base, Unrelated) <= Super
```

which is true because `Base` satisfies this constraint.

And the next example is:

```
Never < forall(Base, Unrelated) < Super
```

which is false, because `Unrelated` doesn't satisfy this constraint.


The part I'm struggling with right now is what a real-world example of `static_assert(ConstraintSet.range(Never, T, Super).satisfied_by_all_typevars(inferable=tuple[T]))` would look like?



---

_@MichaReiser approved on 2025-10-30 23:18_

Thanks again for taking the time to explain this to me during our phone call. The reasoning in the mdtests make sense to me. Altough I'm still finding the DSL slightly confusing. But maybe that just is because I fail to map it to an example using Python

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:175 on 2025-10-31 14:02_

> I'm still somewhat confused by the DSL here but I (maybe?) finally figured out how to read this?

I would formalize it differently, but I think your notation leads to the right understanding:

> Is my understanding correct that this results in:
> 
> ```
> Never <= any(Base, Unrelated) <= Super
> ```
> 
> which is true because `Base` satisfies this constraint.

Yes

> And the next example is:
> 
> ```
> Never < forall(Base, Unrelated) < Super
> ```
> 
> which is false, because `Unrelated` doesn't satisfy this constraint.

This should use `<=` instead of `<` like the first example, but otherwise yes.

> The part I'm struggling with right now is what a real-world example of `static_assert(ConstraintSet.range(Never, T, Super).satisfied_by_all_typevars(inferable=tuple[T]))` would look like?

I'm actually not sure what Python I could write that would result in this check either! We need something that wants to check that an instance of `T` is assignable to `Super`. The non-inferable case is easy:

```py
def f[T: (Base, Unrelated)](t: T):
    x: Super = t
```

But in the `T` inferable case, we would be invoking this function. And I'm not sure what Python code would lead to a `T ≤ Super` check.

The problem is that I can't limit myself to implementing these algorithms for constraint set checks that have obvious Python analogues. 😅 

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:64 on 2025-10-31 14:04_

Done

---

_@dcreager reviewed on 2025-10-31 14:29_

---

_@dcreager reviewed on 2025-10-31 14:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:175 on 2025-10-31 14:51_

> But in the `T` inferable case, we would be invoking this function. And I'm not sure what Python code would lead to a `T ≤ Super` check.

Ah maybe via the return type:

```py
def f[T: (Base, Unrelated)]() -> T:
    raise NotImplementedError

x: Super = f()
```

(Disregard the fact that there is no real function body that we could write that would satisfy that signature!)

---

_Merged by @dcreager on 2025-10-31 14:53_

---

_Closed by @dcreager on 2025-10-31 14:53_

---

_Branch deleted on 2025-10-31 14:53_

---
