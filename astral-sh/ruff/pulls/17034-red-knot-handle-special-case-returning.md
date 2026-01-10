```yaml
number: 17034
title: "[red-knot] Handle special case returning `NotImplemented`"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: cake/notimplemented-as-known-instance
created_at: 2025-03-28T11:48:53Z
updated_at: 2025-04-02T07:52:45Z
url: https://github.com/astral-sh/ruff/pull/17034
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Handle special case returning `NotImplemented`

---

_Pull request opened by @cake-monotone on 2025-03-28 11:48_

## Summary

Closes #16661

This PR includes two changes:

- `NotImplementedType` is now a member of `KnownClass`
- We skip `is_assignable_to` checks for `NotImplemented` when checking return types

### Limitation

```py
def f(cond: bool) -> int:
    return 1 if cond else NotImplemented
```

The implementation covers cases where `NotImplemented` appears inside a `Union`.
However, for more complex types (ex. `Intersection`) it will not worked. In my opinion, supporting such complexity is unnecessary at this point.

## Test Plan

Two `mdtest` files were updated:

- `mdtest/function/return_type.md`
- `mdtest/type_properties/is_singleton.md`

To test `KnownClass`, run:
```bash
cargo test -p red_knot_python_semantic -- types::class::
```

---

_Review requested from @carljm by @cake-monotone on 2025-03-28 11:48_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-03-28 11:48_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-28 11:48_

---

_Review requested from @dcreager by @cake-monotone on 2025-03-28 11:48_

---

_Renamed from "[red-knot] Handle special case returning NotImplemented" to "[red-knot] Handle special case returning `NotImplemented`" by @cake-monotone on 2025-03-28 11:49_

---

_Comment by @github-actions[bot] on 2025-03-28 11:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo[crates/red_knot_python_semantic/src/types/infer.rs:3627]` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo[crates/red_knot_python_semantic/src/types/infer.rs:3615]` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`

```
</details>


---

_Comment by @cake-monotone on 2025-03-28 13:39_

Handling unions may not even be necessary. A comparison between commits using `mypy-primer` showed no differences.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:275 on 2025-03-28 14:00_

```suggestion
## NotImplemented

`NotImplemented` is a special symbol in Python. It is commonly used to control the fallback behavior
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:276 on 2025-03-28 14:00_

Is this trailing backslash needed?
```suggestion
of special dunder methods.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1371 on 2025-03-28 14:12_

I don't think that this needs to be split into two cases by Python version, or needs a TODO comment.

Regardless of Python version, the "original" definition in typeshed is at `builtins._NotImplementedType`; `types.NotImplementedType` is just an alias to that. So `builtins._NotImplementedType` is the only definition we have to recognize here `try_from_file_name`. If we recognize that one (on any Python version), then the alias in `types` module will resolve correctly (once we fix https://github.com/astral-sh/ruff/issues/17032).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1186 on 2025-03-28 14:15_

I don't think we need this TODO comment. Even on newer Python versions, I think the simplest approach is to always consider `builtins._NotImplementedType` as the canonical definition of this type, since it's the one that will always exist.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1008 on 2025-03-28 14:16_

Not sure we need any of this comment, as mentioned below; I think the simplest approach will be to always consider `builtins._NotImplementedType` the canonical definition.

---

_@carljm approved on 2025-03-28 14:19_

This looks great, thank you!

I agree that the union implementation is _probably_ unnecessary in practice, but it also doesn't cost very much to include it, and it is plausible that someone might write an if-expression that conditionally returns `NotImplemented`, so I would suggest we leave it in place.

Mostly I think we can remove some of the commentary and extra complexity around `builtins._NotImplementedType` vs `types.NotImplementedType`, and just let `builtins._NotImplementedType` be canonical on every Python version. The only thing in this PR that I think really should change when we fix https://github.com/astral-sh/ruff/issues/17032 is the one failing `is_singleton` test.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_singleton.md`:102 on 2025-03-28 14:49_

I'm not exactly sure if these backslashes are required here?

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_singleton.md`:118 on 2025-03-28 14:51_

Same as above, not sure if we need the backslash here

---

_@dhruvmanila reviewed on 2025-03-28 15:00_

---

_Label `red-knot` added by @dhruvmanila on 2025-03-28 15:00_

---

_@cake-monotone reviewed on 2025-03-30 03:17_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/class.rs`:1371 on 2025-03-30 03:17_

That makes sense. That clears up my confusion Thanks! 👍 

---

_@carljm approved on 2025-03-30 18:06_

---

_Merged by @carljm on 2025-03-30 18:06_

---

_Closed by @carljm on 2025-03-30 18:06_

---

_@sharkdp reviewed on 2025-04-02 07:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1371 on 2025-04-02 07:24_

I just ran red knot on some code and saw "Object of type `_NotImplementedType` is not assignable to return type …" errors. Took me quite a while to realize that the feature implemented here only works if the target version is set to 3.9 (https://playknot.ruff.rs/c7f2fe9a-294e-4b9a-bd67-03a72eaae726), but not if it is set to 3.10 or higher (https://playknot.ruff.rs/e4d30864-6338-4b69-9962-982c330c09f4). This probably relates to the discussion here and/or to #17032. I just wanted to raise this, in case it wasn't known that the tests here only succeed on 3.9.

---

_@cake-monotone reviewed on 2025-04-02 07:37_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/class.rs`:1371 on 2025-04-02 07:37_

Hmm, that looks so weird... I'll look into it

---

_@cake-monotone reviewed on 2025-04-02 07:52_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/src/types/class.rs`:1371 on 2025-04-02 07:52_

Ahh, it was my mistake 😢
`_NotImplementedType` exists in builtins.pyi regardless of the version, so it's not really related to #17032. I simply forgot to remove the if condition.
Thanks for catching that! I'll fix it soon.

---
