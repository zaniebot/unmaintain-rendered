```yaml
number: 20814
title: "[ty] Fix false-positive diagnostics on `super()` calls"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/not-so-super
created_at: 2025-10-11T20:59:49Z
updated_at: 2025-10-13T11:14:27Z
url: https://github.com/astral-sh/ruff/pull/20814
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix false-positive diagnostics on `super()` calls

---

_Pull request opened by @AlexWaygood on 2025-10-11 20:59_

## Summary

This PR fixes a variety of false-positive `invalid-super-argument` diagnostics that we can emit on `main`. These aren't _that_ common in the ecosystem at the moment, but they will become much more common when we infer a good type for `self` in method bodies. https://github.com/astral-sh/ruff/pull/20723#issuecomment-3372209543 indicates that if we added an improved type of `self` right now, it would lead to nearly 500 new `invalid-super-argument` diagnostics being added, and I'd guess that nearly all of those are false positives.

There were two problems with our current logic: this `match` was not an exhaustive `match`, and was returning `None` for a large number of types. In reality, nearly all objects are valid as the second argument to a `super(x, y)` call as long as they are an instance or subclass of the first argument. (The same is true if the arguments are provided implicitly by the interpreter, as is usually the case.) This PR makes the `match` exhaustive, and significantly reduces the number of types which we disallow as the second argument to `super()`: we now only disallow "purely structural" types such as `Callable` types and synthesized protocols:

https://github.com/astral-sh/ruff/blob/7064c38e53714edd76a9c2f80fed67b38a0b7c54/crates/ty_python_semantic/src/types.rs#L11332-L11359

The second problem relates to this `is_subclass_of` call here:

https://github.com/astral-sh/ruff/blob/7064c38e53714edd76a9c2f80fed67b38a0b7c54/crates/ty_python_semantic/src/types.rs#L11450-L11454

With our current implementation of `is_subclass_of`, `list[Unknown]` is not recognised as being a "subclass of" `Sequence[int]`, for example, because `Sequence[int]` does not literally appear in the MRO of `list[Unknown]` (`Sequence[Unknown]` does). We've [discussed](https://github.com/astral-sh/ty/issues/512#issuecomment-2923625966) [elsewhere](https://github.com/astral-sh/ty/issues/492#issuecomment-2906000565) that this is a bit of a footgun since it doesn't really correspond to the subclass relationship between class objects at runtime, but for now I just work around the footgun :-)

Fixes https://github.com/astral-sh/ty/issues/987, fixes https://github.com/astral-sh/ty/issues/697

## Test plan

Mdtests and snapshots

---

_Label `ty` added by @AlexWaygood on 2025-10-11 20:59_

---

_Comment by @github-actions[bot] on 2025-10-11 21:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-11 21:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_tempfile.py:349:22: error[invalid-super-argument] `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call
- src/anyio/_core/_tempfile.py:370:22: error[invalid-super-argument] `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call
- src/anyio/_core/_tempfile.py:377:22: error[invalid-super-argument] `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call
- Found 224 diagnostics
+ Found 221 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/mixins.py:260:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/mixins.py:278:18: error[invalid-super-argument] `Self@pop` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self@pop)` call
+ src/werkzeug/datastructures/mixins.py:280:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 383 diagnostics
+ Found 384 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/package_base.py:278:9: error[invalid-super-argument] `Unknown & ~<Protocol with members 'executables'>` is not an instance or subclass of `<class 'DetectablePackageMeta'>` in `super(<class 'DetectablePackageMeta'>, Unknown & ~<Protocol with members 'executables'>)` call
- lib/spack/spack/vendor/typing_extensions.py:842:39: error[invalid-super-argument] `Unknown & <Protocol with members '_subs_tree'>` is not an instance or subclass of `Unknown` in `super(Unknown, Unknown & <Protocol with members '_subs_tree'>)` call
- Found 7523 diagnostics
+ Found 7521 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/util.py:124:13: error[invalid-super-argument] `Unknown & ~<class 'AbstractBaseWrapper'>` is not an instance or subclass of `<class 'AbstractBaseWrapper'>` in `super(<class 'AbstractBaseWrapper'>, Unknown & ~<class 'AbstractBaseWrapper'>)` call
- Found 245 diagnostics
+ Found 244 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/testing/tempfile_pool.py:20:29: error[invalid-super-argument] `Unknown & ~<Protocol with members '_instance'>` is not an instance or subclass of `<class 'NamedTemporaryFilePool'>` in `super(<class 'NamedTemporaryFilePool'>, Unknown & ~<Protocol with members '_instance'>)` call
- Found 568 diagnostics
+ Found 567 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/_post_coinit/unknwn.py:294:33: error[invalid-super-argument] `Unknown & _compointer_base` is not an instance or subclass of `<class '_compointer_base'>` in `super(<class '_compointer_base'>, Unknown & _compointer_base)` call
- Found 403 diagnostics
+ Found 402 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:327:34: error[invalid-super-argument] `type[JSONOptions]` is not an instance or subclass of `<class 'JSONOptions'>` in `super(<class 'JSONOptions'>, type[JSONOptions])` call
- Found 515 diagnostics
+ Found 514 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/testing/_raises_group.py:543:9: error[invalid-super-argument] `RaisesGroup[ExcT_1@__init__ | BaseExcT_1@__init__ | BaseExceptionGroup[BaseExcT_2@__init__]]` is not an instance or subclass of `<class 'RaisesGroup'>` in `super(<class 'RaisesGroup'>, RaisesGroup[ExcT_1@__init__ | BaseExcT_1@__init__ | BaseExceptionGroup[BaseExcT_2@__init__]])` call
- Found 648 diagnostics
+ Found 647 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreterbase/baseobjects.py:56:9: error[invalid-super-argument] `type[InterpreterObject]` is not an instance or subclass of `<class 'InterpreterObject'>` in `super(<class 'InterpreterObject'>, type[InterpreterObject])` call
- Found 886 diagnostics
+ Found 885 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 51 diagnostics
+ Found 52 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_internal/websockets.py:134:9: error[invalid-super-argument] `Self@_proxy_connect` is not an instance or subclass of `<class 'WebsocketProxyConnect'>` in `super(<class 'WebsocketProxyConnect'>, Self@_proxy_connect)` call
+ src/prefect/context.py:202:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_copy`, found `ContextModel`
+ src/prefect/context.py:308:20: error[invalid-return-type] Return type does not match returned value: expected `Self@__aenter__`, found `AsyncClientContext`
- src/prefect/context.py:199:15: error[invalid-super-argument] `Self@model_copy` is not an instance or subclass of `<class 'ContextModel'>` in `super(<class 'ContextModel'>, Self@model_copy)` call
- src/prefect/context.py:308:20: error[invalid-super-argument] `Self@__aenter__` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@__aenter__)` call
- src/prefect/context.py:316:20: error[invalid-super-argument] `Self@__aexit__` is not an instance or subclass of `<class 'AsyncClientContext'>` in `super(<class 'AsyncClientContext'>, Self@__aexit__)` call
- Found 3250 diagnostics
+ Found 3248 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/egraph.py:338:20: error[invalid-super-argument] `type[_ExprMetaclass]` is not an instance or subclass of `<class '_ExprMetaclass'>` in `super(<class '_ExprMetaclass'>, type[_ExprMetaclass])` call
- Found 1370 diagnostics
+ Found 1369 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/response.py:67:27: error[invalid-super-argument] `type[Unknown]` is not an instance or subclass of `<class 'MutableJsonResponse'>` in `super(<class 'MutableJsonResponse'>, type[Unknown])` call
- Found 2708 diagnostics
+ Found 2707 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/physics/control/lti.py:414:16: error[invalid-super-argument] `Unknown & ~<class 'LinearTimeInvariant'>` is not an instance or subclass of `<class 'LinearTimeInvariant'>` in `super(<class 'LinearTimeInvariant'>, Unknown & ~<class 'LinearTimeInvariant'>)` call
- sympy/physics/control/lti.py:722:15: error[invalid-super-argument] `Unknown & ~<class 'TransferFunctionBase'>` is not an instance or subclass of `<class 'TransferFunctionBase'>` in `super(<class 'TransferFunctionBase'>, Unknown & ~<class 'TransferFunctionBase'>)` call
- sympy/physics/control/lti.py:5454:19: error[invalid-super-argument] `Unknown & ~<class 'StateSpaceBase'>` is not an instance or subclass of `<class 'StateSpaceBase'>` in `super(<class 'StateSpaceBase'>, Unknown & ~<class 'StateSpaceBase'>)` call
- Found 14031 diagnostics
+ Found 14028 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-10-12 19:01_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-12 19:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-12 19:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-12 19:01_

---

_Label `bug` added by @AlexWaygood on 2025-10-12 19:01_

---

_Comment by @AlexWaygood on 2025-10-12 19:04_

With this PR, the `super()` implementation becomes large enough that we should move it to a separate `types::bound_super` submodule. I haven't done that in this PR, to keep the diff somewhat readable, but I can do it in a followup.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:11320 on 2025-10-13 08:31_

should this (and similar messages below) specify "which is not an instance or a subclass of …", if possible?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:11493 on 2025-10-13 08:55_

A version that avoids the additional allocation would be:
```suggestion
                return Ok(union
                    .elements(db)
                    .iter()
                    .copied()
                    .try_fold(UnionBuilder::new(db), |builder, element| {
                        delegate_to(element).map(|ty| builder.add(ty))
                    })?
                    .build());
```

---

_@sharkdp approved on 2025-10-13 08:56_

Thank you!

I mostly reviewed the implementation and trust you on the semantics here (or to pull someone in for a second review if you are unsure about parts of it).

---

_@AlexWaygood reviewed on 2025-10-13 10:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11493 on 2025-10-13 10:11_

Oh, nice! This branch was unchanged from `main`, but this seems like a better way of doing this for sure

---

_Merged by @AlexWaygood on 2025-10-13 10:57_

---

_Closed by @AlexWaygood on 2025-10-13 10:57_

---

_Branch deleted on 2025-10-13 10:57_

---

_Comment by @AlexWaygood on 2025-10-13 11:00_

> I mostly reviewed the implementation and trust you on the semantics here (or to pull someone in for a second review if you are unsure about parts of it).

I checked all the `reveal_type` calls against the actual type at runtime that you get in the REPL.

Some parts of this I'm slightly unsure about are:
- _Should_ we be banning abstract/structural types as the second argument to `super()`? I don't really know what our behaviour should be if we should allow them. Anyway, we banned them before this PR (but with a worse error message), so this PR didn't really change the status quo there. If others disagree with it and have a good suggestion for what our behaviour should be there, I can make a followup PR and rip it out
- Some of the error messages could probably still be improved (but again, I think they're better than they were before this PR)
- It's not sound in general to upcast a `TypedDict` to a `dict` type like I'm doing there, but I _think_ it's okay in this instance? Anyway, that's an extreme edge case that probably nobody will actually come across in reality

I think we can easily iterate on any of these, which is why I merged despite being unsure about them 😄

---
