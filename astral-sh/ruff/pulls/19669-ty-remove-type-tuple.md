```yaml
number: 19669
title: "[ty] Remove `Type::Tuple`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/remove-tuples
created_at: 2025-07-31T19:05:38Z
updated_at: 2025-08-11T21:03:34Z
url: https://github.com/astral-sh/ruff/pull/19669
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Remove `Type::Tuple`

---

_Pull request opened by @AlexWaygood on 2025-07-31 19:05_

(This PR is easiest to review commit-by-commit. The first commit achieves all the improvements to semantics; following commits are focussed on improving performance.)

## Summary

This PR removes the `Tuple` variant from our `Type` enum. Instead, `Tuple` types are now all represented as `Type::NominalInstance`s with a certain `TupleSpec` stored as part of their generic specialisation.

Removing a `Type` variant leads to simpler code; the more `Type` variants we have, the more complex methods like `Type::has_relation_to` and `Type::is_disjoint_from` become. The main motivation for making this change is not to do with code complexity, however. Instead, it's to refocus how we think about tuple types.

Tuple types are obviously special in the Python type system in many ways. But one way in which they are exactly the same as all other `NominalInstance` type (and different to `Literal` types such as `StringLiteral`, `BytesLiteral` and `BooleanLiteral`) is that the type `tuple[int, str]` does not only include runtime objects where the `__class__` is _exactly_ `tuple`; it also includes instances of _subclasses_ of `tuple[int, str]`. Explicit subclasses of specialised tuples are somewhat rare (though, as the ecosystem report on this PR shows, not as rare as you might think!) -- but tuple subclasses nonetheless appear an awful lot in Python code, since `NamedTuple`s are popular, and every `NamedTuple` class is a tuple subclass. In order to achieve consistent and principled behaviour, all special-casing we apply to `tuple[int, str]` should ideally also be applied to subclasses of `tuple[int, str]`; but this is not something we've done up till now, and it's highly awkward to achieve this if instances of the tuple subclass are represented internally as `Type::NominalInstance`s when the tuple supertype is represented internally as a `Type::Tuple`. Using `Type::NominalInstance` to represent both the tuple type and any subclasses of the tuple type means that it becomes easy to apply special casing to subclasses, and forces us to think explicitly about whether any special casing we add for tuples should apply to "_just_ that tuple type" (though there's really no such thing, since the tuple type is a superset of all its subtypes) or to subtypes of that tuple type too.

Removing `Type::Tuple` therefore has the effect that all our tuple special casing is now applied to tuple subclasses as well as "exact tuple types":
- We now infer precise types from slicing instances of tuple subclasses
- Tuple subclasses can now be unpacked in the same way as tuples
- Comparisons are now precisely inferred between instances of tuple subclasses in the same way as between instances of tuples. This allows us to consolidate our special casing for `sys.version_info`: rather than having dedicated branches in multiple places to handle `sys.version_info` comparisons, we now just treat `sys.version_info` as what it really is at runtime: an instance of a tuple subclass with a very particular tuple spec.
- Tuple subclasses are now understood as sometimes being single-valued types in the same way as "exact tuple types".

Representing all tuple types using `Type::NominalInstance` also appears to have had the unexpected (but very welcome!) effect of improving our ability to solve TypeVars in several situations that showed up in the primer report (and for which I've added regression tests).

#19560, which is stacked on top of this, extends our special-casing for tuples and tuple subclasses to _named_ tuples by inserting precise tuple types into the MRO of `NamedTuple` classes (currently, we model all `NamedTuple` classes as inheriting from `tuple[Unknown, ...]`, which avoids false positives but at the cost of many false negatives).

## Test Plan

Many mdtests added.

I haven't added any new tests specifically for precise comparison inference between instances of tuple subclasses, because this is already tested extensively (both explicitly and implicitly) by our support for `sys.version_info` branches. Our ability to infer `sys.version_info >= (3, 9)` as either `True` or `False` now just directly relies on us seeing that the `sys._version_info` class in typeshed is a tuple subclass (with a special-cased spec), and therefore taking the tuple-and-tuple-subclass path in `TypeInferenceBuilder::infer_binary_type_comparison()`.

## Ecosystem analysis

See https://github.com/astral-sh/ruff/pull/19669#issuecomment-3155770703 for a full analysis. Overall, I think it looks great. The typing conformance diff also shows that this PR brings us closer to conformance with the typing spec.

## Performance impact

Earlier versions of this PR caused large performance regressions, and also regressed memory usage. The latest version of this PR still causes some performance regressions, but they are all 1-2%, which is much reduced. I think partly the remaining regressions ared due to the fact that we're just doing a lot more work than we used to. We're able to infer precise types in many instances where we previously inferred Unknown, and we're able to solve many TypeVars that we previously couldn't.

I've experimented with adding Salsa caching to the methods on `ClassType` for figuring out what a class's tuple spec is. It appeared to speedup `ty_micro[many_string_assignments]` a little, but it had negligible difference on the other benchmarks (and possibly regressed performance slightly on some benchmarks, including colour-science).

---

_Label `ty` added by @AlexWaygood on 2025-07-31 19:05_

---

_Comment by @github-actions[bot] on 2025-07-31 19:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-11 20:53:33.906552447 +0000
+++ new-output.txt	2025-08-11 20:53:33.968552586 +0000
@@ -865,8 +865,6 @@
 tuples_type_compat.py:127:13: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 tuples_type_compat.py:129:13: error[type-assertion-failure] Argument does not have asserted type `tuple[int | str, int]`
 tuples_type_compat.py:130:13: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-tuples_type_compat.py:149:5: error[type-assertion-failure] Argument does not have asserted type `Sequence[int | float | complex | list[int]]`
-tuples_type_compat.py:152:5: error[type-assertion-failure] Argument does not have asserted type `Sequence[int | str]`
 tuples_type_compat.py:153:5: error[type-assertion-failure] Argument does not have asserted type `Sequence[Never]`
 tuples_type_compat.py:157:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[""]]` is not assignable to `tuple[int, str]`
 tuples_type_compat.py:162:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[1], Literal[""]]` is not assignable to `tuple[int, *tuple[str, ...]]`
@@ -886,4 +884,4 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 887 diagnostics
+Found 885 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-31 19:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- parso/python/tokenize.py:230:42: error[invalid-argument-type] Argument is incorrect: Expected `tuple[str]`, found `set[Unknown]`
+ parso/python/tokenize.py:230:42: error[invalid-argument-type] Argument is incorrect: Expected `tuple[str]`, found `set[str]`

Expression (https://github.com/cognitedata/Expression)
- expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], Unknown, /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
+ expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], (Any, /) -> Option[Any], /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], Unknown, /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], (Any, /) -> Result[Any, Any], /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_trio.py:909:47: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- Found 223 diagnostics
+ Found 222 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/middleware/base.py:134:27: warning[possibly-unbound-attribute] Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:151:33: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:154:37: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:165:38: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- starlette/testclient.py:143:40: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/testclient.py:143:60: warning[possibly-unbound-attribute] Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/testclient.py:691:52: error[invalid-argument-type] Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:691:52: error[invalid-argument-type] Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:692:55: error[invalid-argument-type] Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:692:55: error[invalid-argument-type] Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- tests/test_websockets.py:213:5: error[invalid-assignment] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectSendStream[MutableMapping[str, Any]]`
- tests/test_websockets.py:213:18: error[invalid-assignment] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectReceiveStream[MutableMapping[str, Any]]`
- Found 161 diagnostics
+ Found 149 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/predicates.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
+ dedupe/predicates.py:118:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- Found 54 diagnostics
+ Found 56 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:546:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MemoryReceiveChannel[Unknown]`, found `MemorySendChannel[T@as_safe_channel] | MemoryReceiveChannel[T@as_safe_channel]`
- src/trio/_core/_tests/test_guest_mode.py:524:66: error[invalid-argument-type] Argument to function `aio_pingpong` is incorrect: Expected `MemorySendChannel[int]`, found `MemorySendChannel[int] | MemoryReceiveChannel[int]`
- src/trio/_core/_tests/test_guest_mode.py:532:24: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_dtls.py:748:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `Unknown | MemorySendChannel[DTLSChannel] | MemoryReceiveChannel[DTLSChannel]` is possibly unbound
- src/trio/_dtls.py:789:29: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `Unknown | MemorySendChannel[bytes] | MemoryReceiveChannel[bytes]` is possibly unbound
- src/trio/_tests/test_channel.py:30:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:32:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:34:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:37:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:38:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:40:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:42:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:45:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:47:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:52:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:54:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:57:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:59:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:66:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:68:11: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:86:41: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_tests/test_channel.py:108:23: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:132:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:134:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:138:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:140:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:151:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:153:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:168:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:170:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:190:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:192:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:196:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:198:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:209:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:211:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:226:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:228:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:237:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:249:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:255:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:266:19: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:269:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:276:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:287:19: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:290:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:297:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:306:13: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:308:29: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_tests/test_channel.py:324:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:338:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:340:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:341:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:350:13: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:355:28: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:366:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:367:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:368:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:369:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:383:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:385:13: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:392:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:394:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:396:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:398:16: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:400:13: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:401:23: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:407:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:409:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:418:26: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:420:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/type_tests/raisesgroup.py:30:5: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ExceptionGroup[ValueError] | ValueError`
- src/trio/_tests/type_tests/raisesgroup.py:32:9: error[type-assertion-failure] Argument does not have asserted type `ExceptionGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:40:5: error[invalid-assignment] Object of type `BaseExceptionGroup[BaseException]` is not assignable to `BaseExceptionGroup[KeyboardInterrupt]`
- src/trio/_tests/type_tests/raisesgroup.py:154:5: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ExceptionGroup[ExceptionGroup[ValueError]]`
- Found 728 diagnostics
+ Found 653 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ tests/test_runserver_watch.py:108:5: error[invalid-assignment] Object of type `set[tuple[MagicMock, str]]` is not assignable to `set[tuple[WebSocketResponse, str]]`
- Found 123 diagnostics
+ Found 124 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/transforms/transforms.py:1779:16: error[unsupported-operator] Operator `<=` is not supported for types `tuple[float, float]` and `Literal[0]`, in comparing `(Unknown & Number) | (tuple[float, float] & Number)` with `Literal[0]`
+ torchvision/transforms/transforms.py:1779:16: error[unsupported-operator] Operator `<=` is not supported for types `tuple[float, float]` and `int`, in comparing `(Unknown & Number) | (tuple[float, float] & Number)` with `Literal[0]`

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/filepost.py:72:9: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
- src/urllib3/filepost.py:72:16: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
- src/urllib3/filepost.py:79:13: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
- src/urllib3/filepost.py:79:20: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
- Found 396 diagnostics
+ Found 392 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32comext/shell/demos/create_link.py:65:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, Unknown, /) -> Unknown`, found `None`
+ com/win32comext/shell/demos/create_link.py:65:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(str, Literal["SetPath", "SetArguments", "SetDescription", "SetWorkingDirectory"], /) -> Unknown`, found `None`

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:666:16: error[invalid-return-type] Return type does not match returned value: expected `list[L@_parse_children_arg]`, found `list[L@_parse_children_arg | list[L@_parse_children_arg]]`
- src/bokeh/util/serialization.py:146:47: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`

altair (https://github.com/vega/altair)
+ altair/utils/core.py:219:1: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["field", "aggregate", "type", "timeUnit"]]`
- Found 1300 diagnostics
+ Found 1301 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/cluster/_hdbscan/hdbscan.py:131:8: error[unsupported-operator] Operator `>` is not supported for types `tuple[int, @Todo(Support for `typing.TypeAlias`)]` and `Literal[1]`
+ sklearn/cluster/_hdbscan/hdbscan.py:131:8: error[unsupported-operator] Operator `>` is not supported for types `tuple[int, @Todo(Support for `typing.TypeAlias`)]` and `int`, in comparing `tuple[int, @Todo(Support for `typing.TypeAlias`)]` with `Literal[1]`

zulip (https://github.com/zulip/zulip)
- zerver/lib/timestamp.py:21:42: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`
- zerver/lib/timestamp.py:26:42: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`
- Found 3729 diagnostics
+ Found 3727 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/expressions/slice.py:10:13: error[unsupported-operator] Operator `<` is not supported for types `tuple[Unknown, Unknown, Unknown]` and `Literal[0]`, in comparing `(Unknown & ~slice[Any, Any, Any] & ~tuple[Unknown, ...] & ~list[Unknown] & ~Tuple) | (tuple[Unknown, Unknown, Unknown] & ~tuple[Unknown, ...])` with `Literal[0]`
+ sympy/matrices/expressions/slice.py:10:13: error[unsupported-operator] Operator `<` is not supported for types `tuple[Unknown, Unknown, Unknown]` and `int`, in comparing `(Unknown & ~slice[Any, Any, Any] & ~tuple[Unknown, ...] & ~list[Unknown] & ~Tuple) | (tuple[Unknown, Unknown, Unknown] & ~tuple[Unknown, ...])` with `Literal[0]`
+ sympy/matrices/matrixbase.py:574:23: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Self@hstack, Self@hstack, /) -> Self@hstack`, found `def row_join(self, other: Self@row_join) -> Self@row_join`
+ sympy/matrices/matrixbase.py:929:23: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(Self@vstack, Self@vstack, /) -> Self@vstack`, found `def col_join(self, other: Self@col_join, /) -> Self@col_join`
+ sympy/tensor/array/expressions/from_array_to_matrix.py:149:5: error[invalid-assignment] Object of type `list[Basic]` is not assignable to `list[Basic | None]`
- Found 12941 diagnostics
+ Found 12944 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/integrate/tests/test_quadrature.py:124:31: error[invalid-argument-type] Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, _StartT_co@slice, _StartT_co@slice | _StopT_co@slice]`
+ scipy/integrate/tests/test_quadrature.py:145:31: error[invalid-argument-type] Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, _StartT_co@slice, _StartT_co@slice | _StopT_co@slice]`
- Found 6390 diagnostics
+ Found 6392 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-31 19:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-tuples?runnerMode=Instrumentation)

### Merging #19669 will **not alter performance**

<sub>Comparing <code>alex/remove-tuples</code> (6797e9f) with <code>main</code> (2abd683)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-07-31 19:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-tuples?runnerMode=WallTime)

### Merging #19669 will **not alter performance**

<sub>Comparing <code>alex/remove-tuples</code> (6797e9f) with <code>main</code> (2abd683)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @MichaReiser on 2025-07-31 20:07_

What might give us more insight is if you could run ty with `TY_MEMORY_REPORT=full` on `colour-science`. Is there a significant increase of any salsa ingredient count (query or struct). If so, which one? 

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-04 17:10_

---

_Comment by @github-actions[bot] on 2025-08-04 17:21_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unbound-attribute` | 0 | 73 | 0 |
| `invalid-argument-type` | 4 | 8 | 4 |
| `invalid-assignment` | 3 | 5 | 0 |
| `not-iterable` | 0 | 5 | 0 |
| `invalid-return-type` | 3 | 0 | 0 |
| `parameter-already-assigned` | 0 | 3 | 0 |
| `unsupported-operator` | 0 | 0 | 3 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **10** | **95** | **7** |

**[Full report with detailed diff](https://alex-remove-tuples.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-04 22:08_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-04 22:09_

---

_Closed by @AlexWaygood on 2025-08-05 11:29_

---

_Reopened by @AlexWaygood on 2025-08-05 11:29_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-05 13:53_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-05 13:53_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-05 14:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-05 14:07_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-05 14:43_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-05 14:44_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-05 16:12_

---

_Comment by @AlexWaygood on 2025-08-05 16:12_

# Ecosystem analysis

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_trio.py:909:47: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- Found 82 diagnostics
+ Found 81 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/middleware/base.py:134:27: warning[possibly-unbound-attribute] Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:151:33: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:154:37: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/middleware/base.py:165:38: error[not-iterable] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` may not be async-iterable
- starlette/testclient.py:143:40: warning[possibly-unbound-attribute] Attribute `receive` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/testclient.py:143:60: warning[possibly-unbound-attribute] Attribute `send` on type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is possibly unbound
- starlette/testclient.py:691:52: error[invalid-argument-type] Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:691:52: error[invalid-argument-type] Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:692:55: error[invalid-argument-type] Argument is incorrect: Expected `ObjectSendStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- starlette/testclient.py:692:55: error[invalid-argument-type] Argument is incorrect: Expected `ObjectReceiveStream[Unknown]`, found `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]`
- tests/test_websockets.py:213:5: error[invalid-assignment] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectSendStream[MutableMapping[str, Any]]`
- tests/test_websockets.py:213:18: error[invalid-assignment] Object of type `MemoryObjectSendStream[Unknown] | MemoryObjectReceiveStream[Unknown]` is not assignable to `ObjectReceiveStream[MutableMapping[str, Any]]`
- Found 161 diagnostics
+ Found 149 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:546:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MemoryReceiveChannel[Unknown]`, found `MemorySendChannel[T@as_safe_channel] | MemoryReceiveChannel[T@as_safe_channel]`
- src/trio/_core/_tests/test_guest_mode.py:524:66: error[invalid-argument-type] Argument to function `aio_pingpong` is incorrect: Expected `MemorySendChannel[int]`, found `MemorySendChannel[int] | MemoryReceiveChannel[int]`
- src/trio/_core/_tests/test_guest_mode.py:532:24: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_dtls.py:748:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `Unknown | MemorySendChannel[DTLSChannel] | MemoryReceiveChannel[DTLSChannel]` is possibly unbound
- src/trio/_dtls.py:789:29: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `Unknown | MemorySendChannel[bytes] | MemoryReceiveChannel[bytes]` is possibly unbound
- src/trio/_tests/test_channel.py:30:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:32:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:34:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:37:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:38:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:40:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:42:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:45:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:47:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:52:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:54:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:57:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:59:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | str | None] | MemoryReceiveChannel[int | str | None]` is possibly unbound
- src/trio/_tests/test_channel.py:66:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:68:11: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:86:41: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_tests/test_channel.py:108:23: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:132:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:134:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:138:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:140:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:151:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:153:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:168:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:170:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:190:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:192:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:196:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:198:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:209:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:211:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:226:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:228:15: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:237:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:249:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:255:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:266:19: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:269:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:276:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:287:19: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:290:22: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:297:15: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[str] | MemoryReceiveChannel[str]` is possibly unbound
- src/trio/_tests/test_channel.py:306:13: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:308:29: error[not-iterable] Object of type `MemorySendChannel[int] | MemoryReceiveChannel[int]` may not be async-iterable
- src/trio/_tests/test_channel.py:324:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:338:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:340:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:341:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:350:13: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:355:28: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[None] | MemoryReceiveChannel[None]` is possibly unbound
- src/trio/_tests/test_channel.py:366:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:367:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:368:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:369:12: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:383:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:385:13: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:392:5: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:394:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:396:28: warning[possibly-unbound-attribute] Attribute `send` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:398:16: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:400:13: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:401:23: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int | None] | MemoryReceiveChannel[int | None]` is possibly unbound
- src/trio/_tests/test_channel.py:407:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:409:9: warning[possibly-unbound-attribute] Attribute `send_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:418:26: warning[possibly-unbound-attribute] Attribute `receive` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
- src/trio/_tests/test_channel.py:420:9: warning[possibly-unbound-attribute] Attribute `receive_nowait` on type `MemorySendChannel[int] | MemoryReceiveChannel[int]` is possibly unbound
```

These diagnostics are false positives going away because we now infer precise types when users unpack instances of [`anyio._core._streams.create_memory_object_stream`](https://github.com/agronholm/anyio/blob/fef093c1e33563493182b95a0c7028b86c61dd90/src/anyio/_core/_streams.py#L16-L18), which is a subclass of a two-element heterogeneous tuple.

---

```diff
trio (https://github.com/python-trio/trio)
- src/trio/_tests/type_tests/raisesgroup.py:30:5: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ExceptionGroup[ValueError] | ValueError`
- src/trio/_tests/type_tests/raisesgroup.py:32:9: error[type-assertion-failure] Argument does not have asserted type `ExceptionGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:40:5: error[invalid-assignment] Object of type `BaseExceptionGroup[BaseException]` is not assignable to `BaseExceptionGroup[KeyboardInterrupt]`
- src/trio/_tests/type_tests/raisesgroup.py:154:5: error[invalid-assignment] Object of type `ExceptionGroup[Exception]` is not assignable to `ExceptionGroup[ExceptionGroup[ValueError]]`
```

These false positives go away because on `main` we are unable to solve [this TypeVar](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/builtins.pyi#L2238) in the call `ExceptionGroup("", (ValueError(),))`, leading us to fall back to the TypeVar's default value ([`Exception`](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/builtins.pyi#L2195) and infer `ExceptionGroup[Exception]` for the result of the constructor call. On this PR branch, we are able to solve the TypeVar to `ValueError`, leading us to infer `ExceptionGroup[ValueError]` as the result of the constructor call.

---

```diff
dedupe (https://github.com/dedupeio/dedupe)
+ dedupe/predicates.py:116:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
+ dedupe/predicates.py:118:20: error[invalid-return-type] Return type does not match returned value: expected `frozenset[Literal["0", "1"]]`, found `frozenset[str]`
- Found 54 diagnostics
+ Found 56 diagnostics

altair (https://github.com/vega/altair)
+ altair/utils/core.py:219:1: error[invalid-assignment] Object of type `frozenset[str]` is not assignable to `frozenset[Literal["field", "aggregate", "type", "timeUnit"]]`
- Found 1304 diagnostics
+ Found 1305 diagnostics
```

These are new false positives due to things like this:

```py
from typing import Literal

X: frozenset[Literal[1, 2, 3]] = frozenset((1, 2, 3))
```

Previously we inferred `frozenset[Unknown]` for the right-hand side of that assignment; now, we are able to solve the TypeVar in `frozenset.__new__`; but due to `Literal` promotion, we infer `frozenset[int]` rather than `frozenset[Literal[1, 2, 3]]`, leading us to complain that this is an invalid assignment. I tried out a change where we avoid doing `Literal` promotions in covariant contexts, but it's tricky to get it right all the time. For example, in the following case we'd want `list[tuple[int, int]]` to be inferred rather than `list[tuple[Literal[1], Literal[2]]]`, because even though the inner variance context (from `tuple`) is covariant, the outer context (from `list`) is invariant:

```py
Y = list(((1, 2),))
```

As such, I've added a test with some TODOs about this issue, but haven't tried to fix these new false positives in this PR.

---

```diff
aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ tests/test_runserver_watch.py:108:5: error[invalid-assignment] Object of type `set[tuple[MagicMock, str]]` is not assignable to `set[tuple[WebSocketResponse, str]]`
- Found 57 diagnostics
+ Found 58 diagnostics
```

This is a new false positive. I think we'd need bidirectional inference to figure this one out. `MagicMock` is assignable to `WebSocketResponse` (because the class inherits from `Any`), and therefore `tuple[MagicMock, str]` is assignable to `tuple[WebSocketResponse, str]`. But `set[tuple[MagicMock, str]]` is not assignable to `set[tuple[WebSocketResponse, str]]`, because of invariance.

There's no way we'd infer the type of the first tuple element as being `WebSocketResponse` without bidirectionally looking at the type annotation the user has given, I don't think: https://github.com/aio-libs/aiohttp-devtools/blob/fa4af72434e016f9fef0d367e94d2a4b5ca93204/tests/test_runserver_watch.py#L108.

---

```diff
urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/filepost.py:72:9: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
- src/urllib3/filepost.py:72:16: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
- src/urllib3/filepost.py:79:13: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | tuple[bytes, int]` is possibly unbound
- src/urllib3/filepost.py:79:20: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str`, found `BytesIO`
- Found 396 diagnostics
+ Found 392 diagnostics
```

These are false positives going away. We now understand that [the `writer` variable here](https://github.com/urllib3/urllib3/blob/6e0e96c76fedec21a7189342f59cd39a1d8e7086/src/urllib3/filepost.py#L72) is an instance of typeshed's [`codecs._StreamWriter`](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/codecs.pyi#L109-L111) protocol. `writer` is defined [here](https://github.com/urllib3/urllib3/blob/6e0e96c76fedec21a7189342f59cd39a1d8e7086/src/urllib3/filepost.py#L11) as the result of a `codecs.lookup("utf-8")[3]` call; `codecs.lookup` is defined [here](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/_codecs.pyi#L74) and we can see that it returns an instance of [`codecs.CodecInfo`](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/codecs.pyi#L125), which is a subclass of a heterogeneous tuple: we now infer a precise type for the `[3]` index into a `CodecInfo` instance, rather than a union of all element types in the tuple subclass (which is what we did previously).

This case _should_ have been fixed by https://github.com/astral-sh/ruff/pull/19493, but wasn't, because of a limitation in our `Protocol` implementation that will be fixed when we are more rigorous in our understanding of equivalence of protocols that have method members. Protocols are relevant here because each element of the tuple superclass of `codecs.CodecInfo` is a protocol type.

---

```diff
bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/util/serialization.py:146:47: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`

zulip (https://github.com/zulip/zulip)
- zerver/lib/timestamp.py:21:42: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`
- zerver/lib/timestamp.py:26:42: error[parameter-already-assigned] Multiple values provided for parameter `tzinfo` of function `__new__`
- Found 7417 diagnostics
+ Found 7415 diagnostics
```

These false positives goes away because we're now able to infer exactly how many arguments are splatted into the `dt.datetime` call [here](https://github.com/bokeh/bokeh/blob/a47e3ac623e2425d08af6aa7bea83585cda338e2/src/bokeh/util/serialization.py#L146), and in similar places. This is because we now infer a precise type from slicing an instance of the stdlib class [`time.struct_time`](https://github.com/python/typeshed/blob/aeb9c4cb393468ea4e1b387993a3af51781ab28e/stdlib/time.pyi#L42), which is a subclass of a heterogeneous tuple.

---

```diff
bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:666:16: error[invalid-return-type] Return type does not match returned value: expected `list[L@_parse_children_arg]`, found `list[L@_parse_children_arg | list[L@_parse_children_arg]]`
```

There's something complicated going on with `TypeVar` solving here; I haven't investigated in-depth but I suspect it falls under the category of https://github.com/astral-sh/ty/issues/623.

---

```diff
sympy (https://github.com/sympy/sympy)
+ sympy/tensor/array/expressions/from_array_to_matrix.py:149:5: error[invalid-assignment] Object of type `list[Basic]` is not assignable to `list[Basic | None]`
```

This is another one that would need bidirectional inference to fix. `Basic | None` would be a perfectly valid solution to the `list` class constructor's TypeVar [here](https://github.com/sympy/sympy/blob/c8310e2b9620b9bf9e24b2262f8a811b01d173a5/sympy/tensor/array/expressions/from_array_to_matrix.py#L149), but there's no way we'd ever arrive at that particular solution without bidirectionally looking at the annotation the user provided.

---

```diff
scipy (https://github.com/scipy/scipy)
+ scipy/integrate/tests/test_quadrature.py:124:31: error[invalid-argument-type] Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, _StartT_co@slice, _StartT_co@slice | _StopT_co@slice]`
+ scipy/integrate/tests/test_quadrature.py:145:31: error[invalid-argument-type] Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, _StartT_co@slice, _StartT_co@slice | _StopT_co@slice]`
- Found 6796 diagnostics
+ Found 6798 diagnostics
```

These look like true positives to me. The `it` variable [here](https://github.com/scipy/scipy/blob/fa4f00d896306fc5dd60ea55a857b16712652c84/scipy/integrate/tests/test_quadrature.py#L121) is inferred as being an instance of [`numpy.nditer`](https://github.com/numpy/numpy/blob/287cf8f7edadc99999da64f8be053f55b9c5d63e/numpy/__init__.pyi#L5059), which therefore means that the `idx` variable [here](https://github.com/scipy/scipy/blob/fa4f00d896306fc5dd60ea55a857b16712652c84/scipy/integrate/tests/test_quadrature.py#L123) is inferred as being type `list[int]` because it's constructed as `list(it.multi_index)` and the `multi_index` property on `numpy.nditer` is [annotated as returning `tuple[int, ...]`](https://github.com/numpy/numpy/blob/287cf8f7edadc99999da64f8be053f55b9c5d63e/numpy/__init__.pyi#L5118-L5119). You can't `.insert()` an instance of `slice` into an object of `list[int]`!

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-05 16:21_

---

_Comment by @AlexWaygood on 2025-08-05 17:18_

> What might give us more insight is if you could run ty with `TY_MEMORY_REPORT=full` on `colour-science`. Is there a significant increase of any salsa ingredient count (query or struct). If so, which one?

<details>
<summary>Results on b324ae1be3183220ee42c01b394e81c91f0012f5</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=4.15MB   fields=8.31MB   count=103863
`Expression`                                       metadata=2.31MB   fields=2.69MB   count=48045
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=0.86MB   fields=0.74MB   count=15324
`IntersectionType`                                 metadata=0.28MB   fields=0.55MB   count=4941
`OverloadLiteral`                                  metadata=0.32MB   fields=0.27MB   count=5673
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=0.29MB   fields=0.25MB   count=5195
`ScopeId`                                          metadata=0.55MB   fields=0.24MB   count=19789
`StringLiteralType`                                metadata=0.81MB   fields=0.23MB   count=14431
`place_by_id::interned_arguments`                  metadata=0.60MB   fields=0.23MB   count=11511
`File`                                             metadata=0.20MB   fields=0.20MB   count=3080
`FunctionType`                                     metadata=0.36MB   fields=0.15MB   count=6456
`TupleType`                                        metadata=0.12MB   fields=0.14MB   count=2200
`Type < 'db >::apply_specialization_::interned_arguments` metadata=0.22MB   fields=0.09MB   count=3866
`FunctionLiteral`                                  metadata=0.32MB   fields=0.09MB   count=5702
`Specialization`                                   metadata=0.16MB   fields=0.09MB   count=2771
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=0.10MB   fields=0.09MB   count=1794
`CallableType`                                     metadata=0.05MB   fields=0.07MB   count=811
`ClassLiteral`                                     metadata=0.08MB   fields=0.07MB   count=1475
`TypeVarInstance`                                  metadata=0.05MB   fields=0.07MB   count=851
`Unpack`                                           metadata=0.06MB   fields=0.07MB   count=1403
`ModuleLiteralType`                                metadata=0.17MB   fields=0.06MB   count=3194
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=0.17MB   fields=0.05MB   count=3111
`GenericAlias`                                     metadata=0.13MB   fields=0.04MB   count=2403
`UnionType`                                        metadata=0.13MB   fields=0.04MB   count=2351
`FileModule`                                       metadata=0.02MB   fields=0.03MB   count=561
`BoundMethodType`                                  metadata=0.06MB   fields=0.03MB   count=1092
`GenericContext`                                   metadata=0.02MB   fields=0.02MB   count=391
`StarImportPlaceholderPredicate`                   metadata=0.03MB   fields=0.02MB   count=1062
`ModuleNameIngredient`                             metadata=0.03MB   fields=0.02MB   count=596
`PropertyInstanceType`                             metadata=0.02MB   fields=0.01MB   count=290
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=206
`ProtocolInterface`                                metadata=0.00MB   fields=0.00MB   count=83
`BareTypeAliasType`                                metadata=0.00MB   fields=0.00MB   count=36
`BoundSuperType`                                   metadata=0.00MB   fields=0.00MB   count=47
`TypeIsType`                                       metadata=0.00MB   fields=0.00MB   count=11
`Program`                                          metadata=0.00MB   fields=0.00MB   count=1
`EnumLiteralType`                                  metadata=0.00MB   fields=0.00MB   count=4
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`TypedDictType`                                    metadata=0.00MB   fields=0.00MB   count=9
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=2
`FieldInstance`                                    metadata=0.00MB   fields=0.00MB   count=2
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=4
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=2
`PatternPredicate`                                 metadata=0.00MB   fields=0.00MB   count=0
`PEP695TypeAliasType`                              metadata=0.00MB   fields=0.00MB   count=0
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=0
`dynamic_resolution_paths::interned_arguments`     metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
`merge_overrides::interned_arguments`              metadata=0.00MB   fields=0.00MB   count=0
=======SALSA QUERIES=======
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.07MB   fields=68.31MB  count=884
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=6.35MB   fields=67.81MB  count=878
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=7.58MB   fields=20.02MB  count=49271
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=3.43MB   fields=19.92MB  count=6691
`use_def_map -> ty_python_semantic::semantic_index::ArcUseDefMap`
    metadata=0.07MB   fields=15.05MB  count=1338
`source_text -> ruff_db::source::SourceText`
    metadata=0.06MB   fields=11.91MB  count=884
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=14.68MB  fields=9.92MB   count=36601
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.09MB   fields=2.81MB   count=1802
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=1.53MB   fields=1.42MB   count=5287
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=0.39MB   fields=1.02MB   count=2473
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=2.06MB   fields=0.49MB   count=15324
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.01MB   fields=0.47MB   count=134
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.16MB   fields=0.41MB   count=683
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=0.78MB   fields=0.37MB   count=11511
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=0.64MB   fields=0.30MB   count=3111
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=0.25MB   fields=0.24MB   count=1306
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=0.98MB   fields=0.17MB   count=5195
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.26MB   fields=0.16MB   count=2186
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=0.68MB   fields=0.14MB   count=8952
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=0.30MB   fields=0.12MB   count=4469
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.09MB   count=735
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.04MB   fields=0.09MB   count=683
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.13MB   fields=0.09MB   count=1085
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.07MB   count=36
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=0.27MB   fields=0.06MB   count=1343
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=0.23MB   fields=0.06MB   count=3866
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=1.06MB   fields=0.04MB   count=1794
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.11MB   fields=0.03MB   count=1466
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=0.11MB   fields=0.03MB   count=307
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.03MB   count=31
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.11MB   fields=0.01MB   count=1466
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.16MB   fields=0.01MB   count=596
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.02MB   fields=0.01MB   count=320
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=0.05MB   fields=0.01MB   count=206
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=0.04MB   fields=0.01MB   count=758
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.05MB   fields=0.01MB   count=683
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.13MB   fields=0.00MB   count=1382
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.01MB   fields=0.00MB   count=110
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.08MB   fields=0.00MB   count=1217
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.01MB   fields=0.00MB   count=42
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 292.33MB
    struct metadata = 12.68MB
    struct fields = 14.96MB
    memo metadata = 43.00MB
    memo fields = 221.69MB
```

</details>

<details>
<summary>Results on this PR (after rebasing on b324ae1be3183220ee42c01b394e81c91f0012f5)</summary>

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=4.15MB   fields=8.30MB   count=103772
`Expression`                                       metadata=2.31MB   fields=2.69MB   count=48026
`IntersectionType`                                 metadata=0.38MB   fields=0.76MB   count=6792
`Type < 'db >::member_lookup_with_policy_::interned_arguments` metadata=0.86MB   fields=0.74MB   count=15371
`OverloadLiteral`                                  metadata=0.32MB   fields=0.27MB   count=5674
`Type < 'db >::class_member_with_policy_::interned_arguments` metadata=0.29MB   fields=0.25MB   count=5267
`ScopeId`                                          metadata=0.55MB   fields=0.24MB   count=19766
`StringLiteralType`                                metadata=0.81MB   fields=0.23MB   count=14432
`place_by_id::interned_arguments`                  metadata=0.60MB   fields=0.23MB   count=11511
`File`                                             metadata=0.20MB   fields=0.20MB   count=3079
`Specialization`                                   metadata=0.33MB   fields=0.19MB   count=5958
`FunctionType`                                     metadata=0.36MB   fields=0.16MB   count=6466
`TupleType`                                        metadata=0.12MB   fields=0.14MB   count=2214
`Type < 'db >::apply_specialization_::interned_arguments` metadata=0.22MB   fields=0.10MB   count=4000
`FunctionLiteral`                                  metadata=0.32MB   fields=0.09MB   count=5703
`GenericAlias`                                     metadata=0.31MB   fields=0.09MB   count=5578
`Type < 'db >::try_call_dunder_get_::interned_arguments` metadata=0.10MB   fields=0.09MB   count=1807
`ClassLiteral < 'db >::try_mro_::interned_arguments` metadata=0.27MB   fields=0.08MB   count=4738
`CallableType`                                     metadata=0.05MB   fields=0.07MB   count=815
`ClassLiteral`                                     metadata=0.08MB   fields=0.07MB   count=1475
`TypeVarInstance`                                  metadata=0.05MB   fields=0.07MB   count=853
`Unpack`                                           metadata=0.06MB   fields=0.07MB   count=1403
`ModuleLiteralType`                                metadata=0.17MB   fields=0.06MB   count=3194
`UnionType`                                        metadata=0.20MB   fields=0.06MB   count=3637
`FileModule`                                       metadata=0.02MB   fields=0.03MB   count=560
`BoundMethodType`                                  metadata=0.06MB   fields=0.03MB   count=1091
`GenericContext`                                   metadata=0.02MB   fields=0.03MB   count=393
`StarImportPlaceholderPredicate`                   metadata=0.03MB   fields=0.02MB   count=1062
`ModuleNameIngredient`                             metadata=0.03MB   fields=0.02MB   count=595
`PropertyInstanceType`                             metadata=0.02MB   fields=0.01MB   count=292
`Type < 'db >::lookup_dunder_new_::interned_arguments` metadata=0.01MB   fields=0.00MB   count=206
`ProtocolInterface`                                metadata=0.00MB   fields=0.00MB   count=83
`BareTypeAliasType`                                metadata=0.00MB   fields=0.00MB   count=36
`BoundSuperType`                                   metadata=0.00MB   fields=0.00MB   count=47
`TypeIsType`                                       metadata=0.00MB   fields=0.00MB   count=11
`Program`                                          metadata=0.00MB   fields=0.00MB   count=1
`EnumLiteralType`                                  metadata=0.00MB   fields=0.00MB   count=4
`Project`                                          metadata=0.00MB   fields=0.00MB   count=1
`TypedDictType`                                    metadata=0.00MB   fields=0.00MB   count=9
`FileRoot`                                         metadata=0.00MB   fields=0.00MB   count=2
`FieldInstance`                                    metadata=0.00MB   fields=0.00MB   count=2
`DeprecatedInstance`                               metadata=0.00MB   fields=0.00MB   count=4
`BytesLiteralType`                                 metadata=0.00MB   fields=0.00MB   count=2
`PEP695TypeAliasType`                              metadata=0.00MB   fields=0.00MB   count=0
`PatternPredicate`                                 metadata=0.00MB   fields=0.00MB   count=0
`NamespacePackage`                                 metadata=0.00MB   fields=0.00MB   count=0
`dynamic_resolution_paths::interned_arguments`     metadata=0.00MB   fields=0.00MB   count=1
`module_type_symbols::interned_arguments`          metadata=0.00MB   fields=0.00MB   count=1
`merge_overrides::interned_arguments`              metadata=0.00MB   fields=0.00MB   count=0
=======SALSA QUERIES=======
`semantic_index -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=6.34MB   fields=67.76MB  count=877
`parsed_module -> ruff_db::parsed::ParsedModule`
    metadata=0.07MB   fields=66.55MB  count=883
`infer_definition_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=7.73MB   fields=20.02MB  count=49272
`infer_scope_types -> ty_python_semantic::types::infer::ScopeInference`
    metadata=3.59MB   fields=19.92MB  count=6691
`use_def_map -> ty_python_semantic::semantic_index::ArcUseDefMap`
    metadata=0.07MB   fields=15.04MB  count=1335
`source_text -> ruff_db::source::SourceText`
    metadata=0.06MB   fields=11.91MB  count=883
`infer_expression_types -> ty_python_semantic::types::infer::ExpressionInference`
    metadata=15.81MB  fields=9.92MB   count=36601
`place_table -> alloc::sync::Arc<ty_python_semantic::semantic_index::place::PlaceTable>`
    metadata=0.09MB   fields=2.81MB   count=1799
`infer_deferred_types -> ty_python_semantic::types::infer::DefinitionInference`
    metadata=1.59MB   fields=1.42MB   count=5286
`FunctionType < 'db >::signature_ -> ty_python_semantic::types::signatures::CallableSignature`
    metadata=0.39MB   fields=1.02MB   count=2475
`Type < 'db >::member_lookup_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=2.07MB   fields=0.49MB   count=15371
`ClassLiteral < 'db >::try_mro_ -> core::result::Result<ty_python_semantic::types::mro::Mro, ty_python_semantic::types::mro::MroError>`
    metadata=1.05MB   fields=0.48MB   count=4738
`line_index -> ruff_source_file::line_index::LineIndex`
    metadata=0.01MB   fields=0.47MB   count=134
`check_file_impl -> core::result::Result<alloc::boxed::Box<[ruff_db::diagnostic::Diagnostic]>, ruff_db::diagnostic::Diagnostic>`
    metadata=0.16MB   fields=0.41MB   count=683
`place_by_id -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=0.78MB   fields=0.37MB   count=11511
`infer_unpack_types -> ty_python_semantic::types::unpacker::UnpackResult`
    metadata=0.33MB   fields=0.24MB   count=1306
`Type < 'db >::class_member_with_policy_ -> ty_python_semantic::place::PlaceAndQualifiers`
    metadata=0.98MB   fields=0.17MB   count=5267
`all_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.27MB   fields=0.16MB   count=2186
`infer_expression_type -> ty_python_semantic::types::Type`
    metadata=0.68MB   fields=0.14MB   count=8952
`FunctionLiteral < 'db >::overloads_and_implementation_ -> (alloc::boxed::Box<[ty_python_semantic::types::function::OverloadLiteral]>, core::option::Option<ty_python_semantic::types::function::OverloadLiteral>)`
    metadata=0.30MB   fields=0.12MB   count=4470
`imported_modules -> alloc::sync::Arc<std::collections::hash::set::HashSet<ty_python_semantic::module_name::ModuleName, rustc_hash::FxBuildHasher>>`
    metadata=0.04MB   fields=0.09MB   count=735
`suppressions -> ty_python_semantic::suppression::Suppressions`
    metadata=0.04MB   fields=0.09MB   count=683
`all_negative_narrowing_constraints_for_expression -> core::option::Option<std::collections::hash::map::HashMap<ty_python_semantic::semantic_index::place::ScopedPlaceId, ty_python_semantic::types::Type, rustc_hash::FxBuildHasher>>`
    metadata=0.13MB   fields=0.09MB   count=1086
`dunder_all_names -> core::option::Option<std::collections::hash::set::HashSet<ruff_python_ast::name::Name, rustc_hash::FxBuildHasher>>`
    metadata=0.00MB   fields=0.07MB   count=36
`ClassLiteral < 'db >::try_metaclass_ -> core::result::Result<(ty_python_semantic::types::Type, core::option::Option<ty_python_semantic::types::function::DataclassTransformerParams>), ty_python_semantic::types::class::MetaclassError>`
    metadata=0.27MB   fields=0.06MB   count=1347
`Type < 'db >::apply_specialization_ -> ty_python_semantic::types::Type`
    metadata=0.26MB   fields=0.06MB   count=4000
`Type < 'db >::try_call_dunder_get_ -> core::option::Option<(ty_python_semantic::types::Type, ty_python_semantic::types::AttributeKind)>`
    metadata=1.11MB   fields=0.04MB   count=1807
`ClassLiteral < 'db >::explicit_bases_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.11MB   fields=0.03MB   count=1464
`enum_metadata -> core::option::Option<ty_python_semantic::types::enums::EnumMetadata>`
    metadata=0.11MB   fields=0.03MB   count=308
`exported_names -> alloc::boxed::Box<[ruff_python_ast::name::Name]>`
    metadata=0.00MB   fields=0.03MB   count=31
`ClassLiteral < 'db >::pep695_generic_context_ -> core::option::Option<ty_python_semantic::types::generics::GenericContext>`
    metadata=0.11MB   fields=0.01MB   count=1464
`resolve_module_query -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.15MB   fields=0.01MB   count=595
`ClassLiteral < 'db >::decorators_ -> alloc::boxed::Box<[ty_python_semantic::types::Type]>`
    metadata=0.02MB   fields=0.01MB   count=318
`Type < 'db >::lookup_dunder_new_ -> core::option::Option<ty_python_semantic::place::PlaceAndQualifiers>`
    metadata=0.05MB   fields=0.01MB   count=206
`global_scope -> ty_python_semantic::semantic_index::scope::ScopeId`
    metadata=0.04MB   fields=0.01MB   count=757
`file_settings -> ty_project::metadata::settings::FileSettings`
    metadata=0.05MB   fields=0.01MB   count=683
`ClassLiteral < 'db >::is_typed_dict_ -> bool`
    metadata=0.13MB   fields=0.00MB   count=1382
`file_to_module -> core::option::Option<ty_python_semantic::module_resolver::module::Module>`
    metadata=0.01MB   fields=0.00MB   count=110
`ClassLiteral < 'db >::inheritance_cycle_ -> core::option::Option<ty_python_semantic::types::class::InheritanceCycle>`
    metadata=0.08MB   fields=0.00MB   count=1218
`module_type_symbols -> smallvec::SmallVec<[ruff_python_ast::name::Name; 8]>`
    metadata=0.00MB   fields=0.00MB   count=1
`cached_protocol_interface -> ty_python_semantic::types::protocol_class::ProtocolInterface`
    metadata=0.01MB   fields=0.00MB   count=42
`dynamic_resolution_paths -> alloc::vec::Vec<ty_python_semantic::module_resolver::path::SearchPath>`
    metadata=0.00MB   fields=0.00MB   count=1
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 293.82MB
    struct metadata = 13.31MB
    struct fields = 15.37MB
    memo metadata = 45.09MB
    memo fields = 220.04MB
```

</details>

Both reports obtained by running `TY_MEMORY_REPORT=full uv run cargo run --release --manifest-path ../ruff/Cargo.toml -p ty check` from a local clone of `colour`.

---

## Big changes between the two memory reports

- Intersections:
  - On `main` we intern 4941 `IntersectionType` instances when analysing `colour`
    - metadata: 0.28MB
    - fields: 0.55MB
  - On this PR we intern 6792 `IntersectionType` instances
    - metadata: 0.38MB
    - fields: 0.76MB
- `Specialization`:
  - On `main` we intern 2771 `Specialization` instances
    - metadata: 0.16MB
    - fields: 0.09MB
  - On this PR we intern 5958 `Specialization` instances
    - metadata: 0.33MB
    - fields: 0.19MB
- `GenericAlias`:
  - On `main` we intern 2403 `GenericAlias` instances
    - metadata: 0.13MB
    - fields: 0.04MB
  - On this PR we intern 5578 `GenericAlias` instances
    - metadata: 0.31MB
    - fields: 0.09MB
- `ClassLiteral < 'db >::try_mro_::interned_arguments`
  - `main`: 3111 instances interned
    - metadata: 0.17MB
    - fields: 0.05MB
  - this PR: 4738 instances interned
    - metadata: 0.27MB
    - fields: 0.08MB 
- `UnionType`
  - `main`: 2351 instances interned
    - metadata: 0.13MB
    - fields: 0.04MB
  - this PR: 3637 instances interned
    - metadata: 0.20MB
    - fields: 0.06MB

---

_@AlexWaygood reviewed on 2025-08-06 22:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:656 on 2025-08-06 22:27_

I honestly have no idea if this is a correct change or not. No tests fail if I remove this block, and I've struggled to find a behaviour difference from having this block or not having it.

This comment would suggest that it's incorrect to have this block here:

https://github.com/astral-sh/ruff/blob/ef1802b94f3bf7e7afcba2dfb9bd8896e73485c8/crates/ty_python_semantic/src/types/class.rs#L250-L258

But if I remove this, then all the `find_legacy_typevars` methods in `tuple.rs` are flagged as unused. Is it really okay to just delete them as part of this PR...?

Paging @dcreager for help!

---

_Marked ready for review by @AlexWaygood on 2025-08-06 22:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-06 22:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-06 22:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-06 22:28_

---

_Renamed from "[ty] [WIP] Remove `Type::Tuple`" to "[ty] Remove `Type::Tuple`" by @AlexWaygood on 2025-08-06 22:30_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:656 on 2025-08-06 22:57_

The tuple part of a specialization should™ be entirely redundant with the `types` part, since we are careful to always make sure that `types` is the union of all of the elements of the tuple. So it should be safe to remove this block. (It should also be safe to keep it, since `find_legacy_typevars` is building up a set, so it doesn't break anything to add the tuple elements twice).)

It should also be safe to remove the methods from the various tuple classes — those were there for completeness, since we'd need to recurse into them for the `Type::Tuple(tuple)` variant. But if that's now replaced with `NominalInstance` of a `tuple` subclass, then I think it's correct that they're now unused. (There are no other places I'm aware of where a `Type` can contain `Tuple<Type<'db>>` inside of it, but if there were, then we'd still need the `find_legacy_typevars` methods in `tuple.rs`.)

---

_@dcreager reviewed on 2025-08-06 22:58_

---

_@AlexWaygood reviewed on 2025-08-07 08:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:656 on 2025-08-07 08:59_

Okaaay, I removed the call and the methods from the various types in `tuple.rs`. It seems to have had ~no ecosystem impact... the only new diagnostics I can see in the primer report are a few strange ones on pycryptodome that... I can't reproduce locally.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1149 on 2025-08-07 10:26_

here I ran into lifetime issues similar to https://github.com/astral-sh/ruff/pull/19687 (but unlike with that PR, it didn't really seem to me to make sense to split this change into its own PR -- it doesn't have any obvious benefits to callsites to fix the lifetimes bug here, except in the context of this PR)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:7955 on 2025-08-07 10:34_

I did a refactoring of `infer_binary_type_comparison` here similar to the one I did to `infer_subscript_expression_types` in https://github.com/astral-sh/ruff/pull/19658. It noticeably helps performance on this PR branch, but I doubt it would if we did the refactor as a standalone PR to `main`, and it makes the method more complicated. So unlike #19658, I don't think this refactor makes much sense as a standalone change, unfortunately.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4683 on 2025-08-07 10:34_

this is a micro-optimisation that appears to help performance a bit on this branch, but doesn't seem to have a noticeable impact for me locally if I apply the change straight to `main`

---

_@AlexWaygood reviewed on 2025-08-07 10:34_

---

_Comment by @AlexWaygood on 2025-08-07 11:56_

I've extracted the new tests out into a standalone PR to make this one easier to review: #19803. This PR is now based on top of that one.

---

_@AlexWaygood reviewed on 2025-08-07 12:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:154 on 2025-08-07 12:06_

this has been moved to `instance.rs`

---

_Comment by @carljm on 2025-08-07 12:54_

This is pretty cool!

It looks like the performance regression is in large part due to the fact that tuple types are common, and our representation for a simple (non-subclass) tuple type now becomes significantly heavier: rather than just a tuple-spec, it now includes a GenericAlias and a Specialization as well. And that Specialization includes an eagerly-created union of all the tuple element types, to fill the `types` attribute of `Specialization`, whereas before that "homogenized union" was created only on-demand, when necessary (and it's not necessary for many uses of tuple types). So that means we now also create a lot more union types.

This latter part will also be somewhat in conflict with support for recursive type aliases involving tuples (see https://github.com/astral-sh/ruff/pull/19805), where it becomes important that we _don't_ eagerly create a union of all tuple element types (because adding a type alias to a union unpacks the type alias). There are probably some ways around this (e.g. add support for creating a union with a type alias without unpacking it).

But it does seem like there are multiple reasons why it may be valuable to avoid eagerly creating this homogenized union. It seems possible to do this by making `Specialization` an enum, so that when we create a tuple `Specialization` we can by default store only the tuple spec, and leave the homogenized union to be created lazily on-demand. (I think I saw you mentioned something like this above as a possibility.)

In general, I think the focus for improving perf of this PR will be to see how much we can "slim down" our representation of a tuple type to avoid doing unnecessary work and storing/interning unnecessary extra data. I think this will be worth doing even if it reintroduces somewhat more special handling of tuple types internally. I think that's not necessarily in conflict with the goals of this PR, if that special handling is done internally to the NominalInstance/ClassType/GenericAlias infrastructure, so it doesn't leak out into all the type-semantics, like the current `Type::Tuple` does.

---

_Comment by @AlexWaygood on 2025-08-07 13:01_

> But it does seem like there are multiple reasons why it may be valuable to avoid eagerly creating this homogenized union. It seems possible to do this by making `Specialization` an enum, so that when we create a tuple `Specialization` we can by default store only the tuple spec, and leave the homogenized union to be created lazily on-demand. (I think I saw you mentioned something like this above as a possibility.)
> 
> In general, I think the focus for improving perf of this PR will be to see how much we can "slim down" our representation of a tuple type to avoid doing unnecessary work and storing/interning unnecessary extra data. I think this will be worth doing even if it reintroduces somewhat more special handling of tuple types internally. I think that's not necessarily in conflict with the goals of this PR, if that special handling is done internally to the NominalInstance/ClassType/GenericAlias infrastructure, so it doesn't leak out into all the type-semantics, like the current `Type::Tuple` does.

Yup, agreed. I started experimenting with something like this, but then got nervous that it would make the diff on this PR much bigger (and it's already a big diff!). But given the potential conflict with #19805, I'll resume experimenting!

---

_Comment by @carljm on 2025-08-07 13:18_

It might be worth considering whether this approach could be applied at a much higher level; that is, make `NominalInstanceType` an enum with a special case for non-subclass tuples that only stores the tuple spec, and lazily creates the full `ClassType` on demand (probably behind a Salsa query). I suspect that if that's doable, it might improve the perf here a lot.

I don't think the potential conflict with #19805 necessarily needs to drive what gets done in this PR, but I do think there's enough likelihood that the perf impact here is due mostly to this PR making our internal representation for a common type less efficient (as opposed to _just_ being "we understand more types now", though it's quite possible there's some of that too) that it's worth trying some of these things to bring down the perf impact in this PR, rather than landing it as-is and doing those as follow-up. (But I'm also not closed to the latter option if you reach a point where you feel strongly that's the best way to go.)

---

_Comment by @AlexWaygood on 2025-08-07 13:27_

> as opposed to _just_ being "we understand more types now"

Sorry if this sounded hand-wavy in my PR description! That wasn't my intent. I fully agree with you -- from the analysis I did in https://github.com/astral-sh/ruff/pull/19669#issuecomment-3155972695 of Salsa structs being created during a run on colour-science, it definitely looks like we're creating many more `Union`s, `GenericAlias`es and `Specialization`s than we used to.

I think it's pretty likely that us understanding more types is _part_ of the cause for the perf regression, but I definitely didn't mean to say that it was the only cause, or that it wouldn't be possible to find ways to mitigate the perf regression.

---

_Comment by @carljm on 2025-08-07 13:31_

No, I didn't mean to imply that at all! I think what you wrote about perf in the PR summary is reasonable and covers both aspects.

---

_Comment by @AlexWaygood on 2025-08-07 21:12_

> It might be worth considering whether this approach could be applied at a much higher level; that is, make `NominalInstanceType` an enum with a special case for non-subclass tuples that only stores the tuple spec, and lazily creates the full `ClassType` on demand (probably behind a Salsa query). I suspect that if that's doable, it might improve the perf here a lot.

I tried this out in #19811 (stacked on top of this one), and it does indeed significantly help with perf, without sacrificing much of the code-architecture improvements this PR makes! There does seem to be some primer diff on that PR, which I'll look through tomorrow — though I'm not totally sure if we get reliable primer diffs on stacked PRs? I've had trouble reproducing some of the diffs locally for some other stacked PRs I've done recently

---

_Closed by @AlexWaygood on 2025-08-08 12:05_

---

_Reopened by @AlexWaygood on 2025-08-08 12:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:1106 on 2025-08-08 16:47_

Does it help perf if this is a salsa query?

---

_@MichaReiser reviewed on 2025-08-08 16:50_

There's nothing standing out to me why this would regress performance so much other than that walking MROs might be expensive but the MRO should be cached already. 

Have you tried capturing a perf profile locally and comparing it between main and this branch (it might be fruitless but just curious). I can try as well if that would be helpful

---

_@AlexWaygood reviewed on 2025-08-08 16:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1106 on 2025-08-08 16:51_

no, I tried it, it doesn't :-)

See the last paragraph in my PR description

---

_Comment by @AlexWaygood on 2025-08-08 16:59_

> There's nothing standing out to me why this would regress performance so much other than that walking MROs might be expensive but the MRO should be cached already.
> 
> Have you tried capturing a perf profile locally and comparing it between main and this branch (it might be fruitless but just curious). I can try as well if that would be helpful

I believe #19811 (which is stacked on top of this PR) buys back almost all the performance and memory regressions from this change, so I think @carljm's analysis in https://github.com/astral-sh/ruff/pull/19669#issuecomment-3164071301 was correct: this PR just meant that we were eagerly constructing a lot more unions and `Specialization`s than we were previously; it's much better for performance to lazily figure out the union of all the element types in the union if and when we actually _need_ to figure out the `Sequence` supertype of the tuple.

---

_@AlexWaygood reviewed on 2025-08-08 17:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1 on 2025-08-08 17:00_

Several methods in this module are unused in this PR branch, but we start using them again in #19811, which is stacked on top of this branch

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:245 on 2025-08-09 15:52_

It seems unfortunate to me that we add this `known_class` field to `GenericContext`. It's a bit of extra memory, of course, but more that it seems like an encapsulation/layering violation for `GenericContext` to have to know what kind of class it is a context for.

And it doesn't seem necessary to me. The only two call sites for `default_specialization` are both in `ClassLiteral`, which already knows its own known-class, so it seems to me this logic could pretty easily be moved up a layer into `ClassLiteral` by adding an extra parameter to `default_specialization` instead?

---

_Comment by @AlexWaygood on 2025-08-11 11:00_

> I believe #19811 (which is stacked on top of this PR) buys back almost all the performance and memory regressions from this change, so I think @carljm's analysis in [#19669 (comment)](https://github.com/astral-sh/ruff/pull/19669#issuecomment-3164071301) was correct: this PR just meant that we were eagerly constructing a lot more unions and `Specialization`s than we were previously; it's much better for performance to lazily figure out the union of all the element types in the union if and when we actually _need_ to figure out the `Sequence` supertype of the tuple.

I've merged the changes from #19811 into this one. This PR now passes the codspeed CI check: there are still regressions on many benchmarks, but they are nearly all 1-2% regressions (pydantic and sympy show regressions of 3%).

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-11 11:00_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-11 11:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:230 on 2025-08-11 11:02_

This `.expect()` call means we now panic if we're given a custom typeshed without a `tuple` class. I don't _love_ this but I also think it's _okay_ -- there are already classes where we panic if they're not available in typeshed, such as `object` and `types.ModuleType`. `tuple` is similarly special; I think it's okay if we effectively mandate that custom typesheds have to have a `tuple` class in them.

Doing the refactor in the third commit of this PR (which provided a big performance boost) without the `.expect()` call here would be quite a bit harder, I think.

---

_@AlexWaygood reviewed on 2025-08-11 11:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:404 on 2025-08-11 12:56_

this is now moved to be a method on `NominalInstanceType`, so that we avoid materializing the `class` for an instance type where it's unnecessary

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4597 on 2025-08-11 13:03_

also moved to be a method on `NominalInstanceType`, so that we can avoid materializing `ClassType`s where it's unnecessary

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:176 on 2025-08-11 13:03_

no semantic change here, I just micro-optimised this a bit to reduce the number of `.class(db).is_known(db)` calls,

---

_@AlexWaygood reviewed on 2025-08-11 13:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:404 on 2025-08-11 19:57_

It does seem like there are still >1 remaining call sites where you already have a `class` and could use this method rather than the raw `is_known` check? That is, there's no efficiency reason we can't have both this method and the one on `NominalInstanceType`? Marginally more concise and expressive for a few call sites, not a big deal either way.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/type_ordering.rs`:134 on 2025-08-11 20:01_

It seems like this might still cause us to materialize classes for tuples in many cases where we don't really need to? We could pretty easily do something a bit more involved here to avoid that...

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:322 on 2025-08-11 20:14_

I think we should keep the comment here that was previously on the `Type::Tuple` branch of `Type::is_singleton`, and is removed in this diff (that although the empty tuple is in practice a singleton on CPython, this isn't a language guarantee, isn't true for all Python implementations, and shouldn't be relied on.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:183 on 2025-08-11 20:16_

If we also kept `ClassType::is_object`, this could just call it

---

_@carljm approved on 2025-08-11 20:19_

This is awesome!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/type_ordering.rs`:134 on 2025-08-11 20:45_

Yeah... I wasn't sure about how far to go with that kind of optimisation generally. It felt like it made sense for methods already on `NominalInstanceType`, and it felt like it made sense to move commonly used methods like `is_object()` to `NominalInstanceType` (especially since for that one in particular, it also arguably improves ergonomics to move the method onto `NominalInstanceType`). But the premise underlying this PR is that we should basically be treating `tuple`-instance types the same as other `NominalInstance` types in 95% of cases -- adding new methods to `NominalInstanceType` that branch on the inner enum, purely for the purpose of optimisation, feels like it works against that principle to a certain extent?

TL;DR I don't plan to do this in this PR, but also wouldn't object to it being done if you feel like tackling it!

---

_@AlexWaygood reviewed on 2025-08-11 20:45_

---

_@AlexWaygood reviewed on 2025-08-11 20:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:404 on 2025-08-11 20:46_

Fair enough, I can add it back!

---

_@AlexWaygood reviewed on 2025-08-11 20:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:245 on 2025-08-11 20:52_

Ahh, that's much better -- thank you!

---

_Comment by @AlexWaygood on 2025-08-11 21:03_

Final Codspeed report shows 1% _speedups_ on micro and macrobenchmarks! 😃 https://codspeed.io/astral-sh/ruff/branches/alex%2Fremove-tuples?runnerMode=Instrumentation

---

_Merged by @AlexWaygood on 2025-08-11 21:03_

---

_Closed by @AlexWaygood on 2025-08-11 21:03_

---

_Branch deleted on 2025-08-11 21:03_

---
