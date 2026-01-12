```yaml
number: 19337
title: "[ty] Combine CallArguments and CallArgumentTypes"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/merge-arguments
created_at: 2025-07-14T18:51:05Z
updated_at: 2025-07-15T14:21:00Z
url: https://github.com/astral-sh/ruff/pull/19337
synced_at: 2026-01-12T15:56:37Z
```

# [ty] Combine CallArguments and CallArgumentTypes

---

_@dcreager_

We previously had separate `CallArguments` and `CallArgumentTypes` types in support of our two-phase call binding logic. `CallArguments` would store only the arity/kind of each argument (positional, keyword, variadic, etc). We then performed parameter matching using only this arity/kind information, and then infered the type of each argument, placing the result of this second phase into a new `CallArgumentTypes`.

In #18996, we will need to infer the types of splatted arguments _before_ performing parameter matching, since we need to know the argument type to accurately infer its length, which informs how many parameters the splatted argument is matched against.

That makes this separation of Rust types no longer useful. This PR merges everything back into a single `CallArguments`. In the case where we are performing two-phase call binding, the types will be initialized to `None`, and updated to the actual argument type during the second `check_types` phase.

_[This is a refactoring in support of fixing the merge conflicts on #18996. I've pulled this out into a separate PR to make it easier to review in isolation.]_

---

_Review requested from @carljm by @dcreager on 2025-07-14 18:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-14 18:51_

---

_Review requested from @sharkdp by @dcreager on 2025-07-14 18:51_

---

_Label `internal` added by @dcreager on 2025-07-14 18:51_

---

_Label `ty` added by @dcreager on 2025-07-14 18:51_

---

_Comment by @github-actions[bot] on 2025-07-14 18:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @carljm on 2025-07-14 20:19_

Haven't looked at the code here yet, but I'm curious if defaulting to `Type::Unknown` is the best option here, vs using an `Option<Type<'db>>` Rust type? It seems like we might at some point regret the semantic muddiness of the former, where in principle `Type::Unknown` could mean "we haven't tried to infer any type for this argument yet", or it could equally well mean "we inferred a type for this argument, and the type that we found is "Unknown".

---

_Comment by @dcreager on 2025-07-14 20:28_

> Haven't looked at the code here yet, but I'm curious if defaulting to `Type::Unknown` is the best option here, vs using an `Option<Type<'db>>` Rust type? It seems like we might at some point regret the semantic muddiness of the former, where in principle `Type::Unknown` could mean "we haven't tried to infer any type for this argument yet", or it could equally well mean "we inferred a type for this argument, and the type that we found is "Unknown".

Yeah I started that way but it was shaping up to be a much more invasive change, especially in the argument expansion code. The only thing we'd use that distinction for at the moment is to verify that we've already inferred the type of a splatted argument, like [here](https://github.com/astral-sh/ruff/blob/87f2ff9516dccddc385b4ed136700284ca3d1979/crates/ty_python_semantic/src/types/infer.rs#L4592).

Rather, I think a better approach in the spirit of your suggestion would be to keep separate `CallArguments` and `CallArgumentTypes`, but where the first has a `Vec<Option<Type<'db>>>` and the second has a `Vec<Type<'db>>`. (Maybe the types would deserve different names at that point? `CallArgumentsBuilder` and `CallArguments`?)

---

_Comment by @dcreager on 2025-07-14 20:57_

> Yeah I started that way but it was shaping up to be a much more invasive change, especially in the argument expansion code.

Tried it again and with some cleanup I had done in the argument expansion code it looks better on the second attempt!  Done

---

_@dhruvmanila approved on 2025-07-15 06:16_

Looks good, thank you!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:168 on 2025-07-15 07:51_

Would it make sense to use a constructor function on `CallArguments` instead of initializing the fields directly so that we can assert that `argumetns` and `types` have the same len

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/arguments.rs`:190 on 2025-07-15 07:52_

NIt: I think I would prefer an explicit method on `CallArguments` instead of using `From` here which makes it clear that all types will be uninitialized.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:4744 on 2025-07-15 07:53_

Let's add a debug assertion that `arguments`, `arguments_forms`, and `ast_arguments` all have the same length

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:7813 on 2025-07-15 07:54_

Nit: You could add a `arguments.types()` method that returns an iterator over types

---

_@MichaReiser approved on 2025-07-15 07:54_

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-15 09:13_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:190 on 2025-07-15 13:54_

Turns out this wasn't being used! Removed

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:4744 on 2025-07-15 13:55_

done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:168 on 2025-07-15 13:56_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:7813 on 2025-07-15 14:01_

Done (had to name it `iter_types` since there was already a `types` accessor that returns a `&[Option<Type<'db>>]`

---

_@dcreager reviewed on 2025-07-15 14:01_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-15 14:03_

---

_Merged by @dcreager on 2025-07-15 14:20_

---

_Closed by @dcreager on 2025-07-15 14:20_

---

_Branch deleted on 2025-07-15 14:21_

---
