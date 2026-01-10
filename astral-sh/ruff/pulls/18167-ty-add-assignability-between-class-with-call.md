```yaml
number: 18167
title: "[ty] Add assignability between class with `__call__` class attribute and callable"
type: pull_request
state: closed
author: med1844
labels:
  - ty
assignees: []
base: main
head: call-as-attribute-fix
created_at: 2025-05-19T01:31:42Z
updated_at: 2025-05-23T03:16:54Z
url: https://github.com/astral-sh/ruff/pull/18167
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Add assignability between class with `__call__` class attribute and callable

---

_Pull request opened by @med1844 on 2025-05-19 01:31_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

There are some classes that's callable but defined via `__call__` as attributes, e.g. [pytorch `Module`](https://github.com/pytorch/pytorch/blob/main/torch/nn/modules/module.py#L1906).

This PR should add support for checking assignability for such classes. Not sure if we should do subtyping though.

## Test Plan

<!-- How was it tested? -->

I added tests into `is_assignable_to.md`. Referred to this [`pyright` PR](https://github.com/microsoft/pyright/pull/5799) and this [`mypy` PR](https://github.com/python/mypy/pull/11150).

I tested multiple possible forms of having `__call__` attribute, and check if ty accepts & rejects callable types correctly.

## Things to Notice

- First time contributing, so please don't hesitate to point out anything that looks off!
- Regarding class `A` in the test: [`pyright`](https://github.com/microsoft/pyright/pull/5799) seems to reject `__call__` as instance variables when it comes to callable check, while `mypy` appears to accept it. The type of `a.__call__` checked by `ty` is `Unknown`, though I personally think it should be `Unknown | A.method1` with the `self` parameter already filled in. I'm open to discussing which approach would be best here.
- Regarding class `C` in the test. `pyright` doesn't allow assigning `c` into callable with `self` filled. However it does allow assigning `b` into callable (no explicit type hint on `__call__`). [link](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQDK1JwAUKwMbkDO3UAQgC5WUUVAAm1YFAjUYACzDiAjAApuzYPQqDMKGAEooAWgB8emMLHWoIOQFcQKKBXbWA%2Bu46kSnqAF4ZOUUVdk4ePgIrMUlpWQUlNQ0Weg5dVEMTcwzom1sHJygONzFPbzJPXWIyShoAbTqmFIsAXXoMloCghNDWWKhgVTTCH1rqBo72-RaDXQA6BfYKLv5VA04ugjXWQYp1wY4DIA)

Thanks for your time and feedback!

---

_Review requested from @carljm by @med1844 on 2025-05-19 01:31_

---

_Review requested from @AlexWaygood by @med1844 on 2025-05-19 01:31_

---

_Review requested from @sharkdp by @med1844 on 2025-05-19 01:31_

---

_Review requested from @dcreager by @med1844 on 2025-05-19 01:31_

---

_Label `ty` added by @AlexWaygood on 2025-05-19 02:39_

---

_Comment by @github-actions[bot] on 2025-05-19 05:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-assignment] strawberry/federation/scalar.py:133:9: Object of type `_T | None` is not assignable to `((...) -> Unknown) | None`
- error[invalid-argument-type] strawberry/types/scalar.py:110:29: Argument to bound method `__init__` is incorrect: Expected `(Any, /) -> Any`, found `_T`
- error[invalid-assignment] strawberry/types/scalar.py:209:9: Object of type `_T | None` is not assignable to `((...) -> Unknown) | None`
- Found 436 diagnostics
+ Found 433 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/ext.py:168:27: Argument to bound method `call` is incorrect: Expected `(...) -> Any`, found `Any | Undefined`
- Found 234 diagnostics
+ Found 233 diagnostics

```
</details>


---

_Comment by @carljm on 2025-05-22 05:37_

Thank you for the PR!

I spent a bit of time looking at this today. It's a bit of a can of worms, because the behavior (partially) implemented in this PR is that `Callable` types bind as method descriptors when used to annotate a `__call__` attribute (that is, their first argument is bound to `self` when accessed from an instance), but we currently don't implement the assumption that `Callable` types have a `__get__` method or bind as method descriptors. This assumption doesn't seem right in general, since non-descriptor callable types (e.g. an instance of a class with a `__call__` method) are also subtypes of `Callable`. But assuming callable types are _not_ method descriptors can also lead to inferring the wrong signature of the callable when accessed as attribute (e.g. if a function object, which is a method descriptor, is assigned to the callable-typed attribute.)

I say "partially" above because we really need to be consistent about the signature we use for an object when deciding its subtyping/assignability vs Callable, and the signature we use when actually type-checking a call of the object. This means that I don't think we should implement this change in these subtyping/assignability arms at all; whatever we do here should be implemented at a deeper layer, in `try_call_dunder_get`, where we decide how a type behaves as a descriptor.

I've pushed a few initial changes here: added subtyping tests, corrected the assignability tests for the behavior I think we want, and cleaned up some pre-existing mistakes in the subtyping/assignability arms for instance vs callable. For one thing, we need to look up dunder methods (such as `__call__`) on the class, not the instance, and we were previously doing that wrong. Since we consider an annotated class attribute with no assigned value (e.g. `__call__: Callable[...]`) to declare an instance-only attribute, we shouldn't respect that as a usable `__call__`. Also, for the consistency reason described above, we shouldn't have the existing special handling of `BoundMethod` in those arms. We should only have the generic delegation fallback (to compare the type of `__call__` to the callable type).

The tests are currently failing because I'm still considering how to best make them pass.

I tried locally implementing always assuming callable types are method descriptors, because this seems to be what mypy and pyright do. It's not hard to implement this as a special case in `try_call_dunder_get`, but it has a significant blast radius -- it breaks a number of existing cases where we synthesize callable types as class attributes, such as data classes and named tuples. We could probably adjust to this by adding self arguments in all those cases.

I might still do this, but it occurred to me that we could also wait for https://github.com/astral-sh/ruff/pull/18242 to land, so that we can explicitly have both method-descriptor and non-method-descriptor callable types. Then we'd have the option of being even smarter here: for a callable-annotated class attribute where we see an RHS assigned to it, we could check whether that RHS is a method descriptor type or not; if it is, we could use a method-descriptor callable type for the declaration, otherwise we could use a non-method-descriptor callable type.

TODO:
- [ ] add tests for callable-annotated `__call__` where a non-method-descriptor (e.g. staticmethod) is assigned
- [ ] add tests for callable-annotated "methods" that are not `__call__`
- [ ] add tests for actually calling a callable-annotated method (to show we treat it as having the same signature we consider it assignable to), not just for assignability/subtyping
- [ ] make the tests pass

---

_Comment by @carljm on 2025-05-22 05:46_

> it occurred to me that we could also wait for #18242 to land, so that we can explicitly have both method-descriptor and non-method-descriptor callable types. Then we'd have the option of being even smarter here

On second thought, I think it's pretty weird to interpret the exact same annotated type differently depending on the RHS of the assignment. So maybe we shouldn't do this.

Probably tomorrow I'll revisit the "always assume callable types are method descriptors" option and see how that works out.

---

_Comment by @carljm on 2025-05-22 18:47_

As a first step, I extracted the simpler fixes we definitely need here into #18260 (with you credited as co author) so we can consider here what other changes we might want to make in terms of bound-method-descriptor assumptions. 

---

_Comment by @carljm on 2025-05-22 19:58_

I think #18260 fixes the core issue this PR was intended to fix. E.g. the pytorch case doesn't care about bound-method descriptor or not, because it uses a `...` signature; it just cares that we honor assignability to Callable based on `__call__`, and after #18260 we do. The bound-method descriptor question is a somewhat separate issue that was raised by the way the tests were written here. I've written that up for later follow-up in https://github.com/astral-sh/ty/issues/491, but I don't think it's an immediate priority. So I'm going to close this PR for now.

---

_Closed by @carljm on 2025-05-22 19:58_

---

_Comment by @med1844 on 2025-05-23 03:16_

Thanks for that amazing analysis! I definitely missed cases for functions that's not bounded methods and didn't realize how deep this rabbit hole is until I read the follow up issue and discussion. I learnt a lot, thank you so much.

I will try to contribute to some more well-defined issues then.

---
