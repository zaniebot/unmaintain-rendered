```yaml
number: 20368
title: "[ty] Improve assignability/subtyping between two protocol types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - great writeup
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/subtyping-between-protocols
created_at: 2025-09-12T19:30:33Z
updated_at: 2025-10-08T18:47:54Z
url: https://github.com/astral-sh/ruff/pull/20368
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Improve assignability/subtyping between two protocol types

---

_@AlexWaygood_

## Summary

We currently have reasonably good assignability and subtyping checks between nominal types and protocol types if the protocol has a method member or an attribute member. For example, these assertions all pass:

```py
from ty_extensions import static_assert, is_subtype_of
from typing import Iterable, Iterator, Callable

class IsIterable:
    def __iter__(self) -> Iterator[str]: ...

class NotIterable:
    __iter__ = None

class AlsoNotIterable:
    def __init__(self):
        self.__init__: Callable[[], Iterator[int]] = lambda: iter(range(42))

static_assert(is_subtype_of(IsIterable, Iterable[str]))
static_assert(not is_subtype_of(NotIterable, Iterable[str]))
static_assert(not is_subtype_of(AlsoNotIterable, Iterator[int]))
```

However, we do not currently have a good implementation of subtyping and assignability _between_ protocol types: we currently think that `Iterator[str]` is a subtype of `Iterator[int]`, and vice versa, for example, because both protocols have `__iter__` and `__next__` method members, and we do not currently compare the types of these members when doing subtyping/assignability checks _between_ protocols (we only check for the presence of those members).

This PR fixes that.

Fixes https://github.com/astral-sh/ty/issues/306
Fixes https://github.com/astral-sh/ty/issues/733
Fixes https://github.com/astral-sh/ty/issues/1089

## Implementation details

The change that I _wanted_ to make in this PR is the change to `protocol_class.rs`. Unfortunately, I quickly ran into stack overflows after making that change, however, due to large cycles that were spread across `is_subtype_of` and `is_disjoint_from`. Looking at backtraces, the root of the problem was clear: `has_relation_to` for intersections calls `is_disjoint_from`:

https://github.com/astral-sh/ruff/blob/3771f1567c778776777679674b8cffdc90aa51aa/crates/ty_python_semantic/src/types.rs#L1653-L1665

...and `is_disjoint_from` for intersections calls `has_relation_to`...

https://github.com/astral-sh/ruff/blob/3771f1567c778776777679674b8cffdc90aa51aa/crates/ty_python_semantic/src/types.rs#L2196-L2209

...meaning that existing mdtests ended up running into an infinite loop when checking whether intersections of protocols were subtypes of other types. This infinite loop could not be caught by our existing cycle-detector infrastructure, because a fresh cycle detector would be created in the call to `has_relation_to` call from `is_disjoint_from_impl`, and this cycle detector would not be passed into the `is_disjoint_from` call inside `has_relation_to`.

I therefore had to make a much more invasive change here than I wanted to, in order to fix these stack overflows. Rather than accepting only one cycle detector as an input parameter, `has_relation_to_impl` must now accept two cycle detectors: one `HasRelationToVisitor` and one `IsDisjointFromVisitor`. This allows `has_relation_to` to call `is_disjoint_from_impl` rather than `is_disjoint_from`, meaning that the same cycle detector can be passed between `has_relation_to` calls and `is_disjoint_from` calls. The cycle can therefore be detected and stopped in its tracks before a stack overflow occurs.

Similar changes also needed to be made to `is_disjoint_from_impl`: it now accepts a `HasRelationToVisitor` as well as an `IsDisjointFromVisitor`.

## Performance

There are some regressions of up to 5% or so on some benchmarks. I think this is just because we're doing more work now, but I'm happy to hear suggestions if anybody has any ideas for how to optimise this.

## Conformances suite impact

See https://github.com/astral-sh/ruff/pull/20368#issuecomment-3286597297

## Ecosystem impact

See https://github.com/astral-sh/ruff/pull/20368#issuecomment-3368395654

## Test Plan

Mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-09-12 19:30_

---

_Comment by @github-actions[bot] on 2025-09-12 19:32_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-08 18:35:15.522415266 +0000
+++ new-output.txt	2025-10-08 18:35:18.851436775 +0000
@@ -104,6 +104,7 @@
 callables_annotation.py:57:18: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+callables_annotation.py:157:5: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
 callables_kwargs.py:24:5: error[type-assertion-failure] Argument does not have asserted type `int`
 callables_kwargs.py:32:9: error[type-assertion-failure] Argument does not have asserted type `str`
 callables_kwargs.py:35:5: error[type-assertion-failure] Argument does not have asserted type `str`
@@ -130,6 +131,35 @@
 callables_protocol.py:311:1: error[invalid-assignment] Object of type `def cb14_no_default(*, path: str) -> str` is not assignable to `Proto14_Default`
 callables_subtyping.py:26:5: error[invalid-assignment] Object of type `(int, /) -> int` is not assignable to `(int | float, /) -> int | float`
 callables_subtyping.py:29:5: error[invalid-assignment] Object of type `(int | float, /) -> int | float` is not assignable to `(int, /) -> int`
+callables_subtyping.py:51:5: error[invalid-assignment] Object of type `PosOnly2` is not assignable to `Standard2`
+callables_subtyping.py:52:5: error[invalid-assignment] Object of type `KwOnly2` is not assignable to `Standard2`
+callables_subtyping.py:55:5: error[invalid-assignment] Object of type `KwOnly2` is not assignable to `PosOnly2`
+callables_subtyping.py:58:5: error[invalid-assignment] Object of type `PosOnly2` is not assignable to `KwOnly2`
+callables_subtyping.py:82:5: error[invalid-assignment] Object of type `NoArgs3` is not assignable to `IntArgs3`
+callables_subtyping.py:85:5: error[invalid-assignment] Object of type `NoArgs3` is not assignable to `FloatArgs3`
+callables_subtyping.py:86:5: error[invalid-assignment] Object of type `IntArgs3` is not assignable to `FloatArgs3`
+callables_subtyping.py:116:5: error[invalid-assignment] Object of type `IntArgs4` is not assignable to `PosOnly4`
+callables_subtyping.py:119:5: error[invalid-assignment] Object of type `StrArgs4` is not assignable to `IntStrArgs4`
+callables_subtyping.py:120:5: error[invalid-assignment] Object of type `IntArgs4` is not assignable to `IntStrArgs4`
+callables_subtyping.py:122:5: error[invalid-assignment] Object of type `IntArgs4` is not assignable to `StrArgs4`
+callables_subtyping.py:124:5: error[invalid-assignment] Object of type `StrArgs4` is not assignable to `IntArgs4`
+callables_subtyping.py:126:5: error[invalid-assignment] Object of type `StrArgs4` is not assignable to `Standard4`
+callables_subtyping.py:151:5: error[invalid-assignment] Object of type `NoKwargs5` is not assignable to `IntKwargs5`
+callables_subtyping.py:154:5: error[invalid-assignment] Object of type `NoKwargs5` is not assignable to `FloatKwargs5`
+callables_subtyping.py:155:5: error[invalid-assignment] Object of type `IntKwargs5` is not assignable to `FloatKwargs5`
+callables_subtyping.py:187:5: error[invalid-assignment] Object of type `IntKwargs6` is not assignable to `KwOnly6`
+callables_subtyping.py:190:5: error[invalid-assignment] Object of type `StrKwargs6` is not assignable to `IntStrKwargs6`
+callables_subtyping.py:191:5: error[invalid-assignment] Object of type `IntKwargs6` is not assignable to `IntStrKwargs6`
+callables_subtyping.py:193:5: error[invalid-assignment] Object of type `IntKwargs6` is not assignable to `StrKwargs6`
+callables_subtyping.py:195:5: error[invalid-assignment] Object of type `StrKwargs6` is not assignable to `IntKwargs6`
+callables_subtyping.py:196:5: error[invalid-assignment] Object of type `IntStrKwargs6` is not assignable to `Standard6`
+callables_subtyping.py:197:5: error[invalid-assignment] Object of type `StrKwargs6` is not assignable to `Standard6`
+callables_subtyping.py:236:5: error[invalid-assignment] Object of type `NoDefaultArg8` is not assignable to `DefaultArg8`
+callables_subtyping.py:237:5: error[invalid-assignment] Object of type `NoX8` is not assignable to `DefaultArg8`
+callables_subtyping.py:240:5: error[invalid-assignment] Object of type `NoX8` is not assignable to `NoDefaultArg8`
+callables_subtyping.py:243:5: error[invalid-assignment] Object of type `NoDefaultArg8` is not assignable to `NoX8`
+callables_subtyping.py:273:5: error[invalid-assignment] Object of type `Overloaded9` is not assignable to `FloatArg9`
+callables_subtyping.py:297:5: error[invalid-assignment] Object of type `StrArg10` is not assignable to `Overloaded10`
 classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
 classes_classvar.py:39:14: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 classes_classvar.py:40:14: error[unresolved-reference] Name `var` used when not defined
@@ -669,6 +699,7 @@
 narrowing_typeguard.py:89:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeguard.py:93:9: error[type-assertion-failure] Argument does not have asserted type `B`
 narrowing_typeis.py:21:9: error[type-assertion-failure] Argument does not have asserted type `tuple[str, ...]`
+narrowing_typeis.py:35:9: error[invalid-assignment] Object of type `object` is not assignable to `int`
 narrowing_typeis.py:38:9: error[type-assertion-failure] Argument does not have asserted type `int`
 narrowing_typeis.py:72:9: error[type-assertion-failure] Argument does not have asserted type `int`
 narrowing_typeis.py:76:9: error[type-assertion-failure] Argument does not have asserted type `int`
@@ -713,6 +744,7 @@
 overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
+protocols_definition.py:30:11: error[invalid-argument-type] Argument to function `close_all` is incorrect: Expected `Iterable[SupportsClose]`, found `list[Unknown | int]`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:1: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
 protocols_definition.py:116:1: error[invalid-assignment] Object of type `Concrete2_Bad3` is not assignable to `Template2`
@@ -726,6 +758,10 @@
 protocols_definition.py:288:1: error[invalid-assignment] Object of type `Concrete5_Bad4` is not assignable to `Template5`
 protocols_definition.py:289:1: error[invalid-assignment] Object of type `Concrete5_Bad5` is not assignable to `Template5`
 protocols_generic.py:40:1: error[invalid-assignment] Object of type `Concrete1` is not assignable to `Proto1[int, str]`
+protocols_generic.py:56:5: error[invalid-assignment] Object of type `Box[int | float]` is not assignable to `Box[int]`
+protocols_generic.py:66:5: error[invalid-assignment] Object of type `Sender[int]` is not assignable to `Sender[int | float]`
+protocols_generic.py:74:5: error[invalid-assignment] Object of type `AttrProto[int]` is not assignable to `AttrProto[int | float]`
+protocols_generic.py:75:5: error[invalid-assignment] Object of type `AttrProto[int | float]` is not assignable to `AttrProto[int]`
 protocols_merging.py:52:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable1`
 protocols_merging.py:53:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable2`
 protocols_merging.py:54:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable3`
@@ -742,6 +778,10 @@
 protocols_subtyping.py:16:6: error[call-non-callable] Cannot instantiate class `Proto1`: This call will raise `TypeError` at runtime
 protocols_subtyping.py:38:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Concrete2`
 protocols_subtyping.py:55:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Proto3`
+protocols_subtyping.py:79:5: error[invalid-assignment] Object of type `Proto5[int]` is not assignable to `Proto4[int, int | float]`
+protocols_subtyping.py:80:5: error[invalid-assignment] Object of type `Proto4[int, int]` is not assignable to `Proto5[int | float]`
+protocols_subtyping.py:102:5: error[invalid-assignment] Object of type `Proto6[int | float, int | float]` is not assignable to `Proto7[int, int | float]`
+protocols_subtyping.py:103:5: error[invalid-assignment] Object of type `Proto6[int | float, int | float]` is not assignable to `Proto7[int | float, object]`
 protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 qualifiers_annotated.py:36:42: error[invalid-syntax] named expression cannot be used within a type annotation
 qualifiers_annotated.py:40:28: error[invalid-syntax] await expression cannot be used within a type annotation
@@ -858,5 +898,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 859 diagnostics
+Found 899 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-12 19:36_

---

_Comment by @AlexWaygood on 2025-09-12 19:37_

> ```diff
> +callables_annotation.py:157:5: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
> ```

According to the conformance test, we're not meant to emit an error on this one ([here](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/callables_annotation.py#L157)). All other new diagnostics on the conformance suite look correct.

Edit: this is a pre-existing bug in `Callable` assignability. I opened a separate issue for this (https://github.com/astral-sh/ty/issues/1257)

---

_Comment by @github-actions[bot] on 2025-09-12 19:40_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 141 | 27 | 60 |
| `unused-ignore-comment` | 10 | 51 | 0 |
| `invalid-return-type` | 4 | 44 | 8 |
| `possibly-missing-attribute` | 36 | 2 | 3 |
| `invalid-assignment` | 19 | 10 | 8 |
| `no-matching-overload` | 35 | 1 | 0 |
| `unresolved-attribute` | 11 | 15 | 0 |
| `unsupported-operator` | 6 | 7 | 0 |
| `redundant-cast` | 0 | 10 | 0 |
| `type-assertion-failure` | 5 | 3 | 0 |
| `index-out-of-bounds` | 3 | 3 | 0 |
| `not-iterable` | 5 | 1 | 0 |
| `call-non-callable` | 3 | 1 | 0 |
| `possibly-missing-implicit-call` | 3 | 0 | 0 |
| **Total** | **281** | **175** | **79** |

**[Full report with detailed diff](https://alex-subtyping-between-proto.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-subtyping-between-proto.ecosystem-663.pages.dev/timing))


---

_Comment by @github-actions[bot] on 2025-09-15 16:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_validators.py:1339:27: error[invalid-argument-type] Argument to function `not_` is incorrect: Expected `type[Exception] | Iterable[type[Exception]]`, found `tuple[<class 'str'>, <class 'int'>]`
- Found 556 diagnostics
+ Found 557 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_contextmanagers.py:72:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co@__enter__`, found `object`
- src/anyio/_core/_contextmanagers.py:165:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co@__aenter__`, found `object`
- src/anyio/_core/_sockets.py:974:38: error[invalid-argument-type] Argument is incorrect: Expected `str | IPv4Address | IPv6Address`, found `str | IPv4Address | IPv6Address | int`
- src/anyio/_core/_sockets.py:974:38: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `str | IPv4Address | IPv6Address | int`
+ src/anyio/_core/_sockets.py:974:38: error[invalid-argument-type] Argument is incorrect: Expected `str | IPv4Address | IPv6Address`, found `object`
+ src/anyio/_core/_sockets.py:974:38: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `object`
+ src/anyio/_core/_sockets.py:976:39: error[invalid-argument-type] Argument is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `(str & ~ByteStreamConnectable) | (bytes & ~ByteStreamConnectable) | (tuple[str | IPv4Address | IPv6Address, int] & PathLike[object] & ~ByteStreamConnectable) | (PathLike[str] & ~ByteStreamConnectable)`
- Found 225 diagnostics
+ Found 224 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/renderers/html.py:104:25: error[invalid-argument-type] Argument to function `open` is incorrect: Expected `str`, found `Literal[b""]`
- Found 42 diagnostics
+ Found 41 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/core.py:86:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None]`, found `TextIO`
+ isort/core.py:126:40: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[None]`, found `TextIO`
- Found 34 diagnostics
+ Found 36 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/models/direct_url.py:192:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ src/pip/_internal/locations/_distutils.py:92:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- src/pip/_internal/models/link.py:182:11: error[unsupported-operator] Operator `+` is unsupported between objects of type `str` and `Literal[b""]`
- src/pip/_internal/models/link.py:459:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/pip/_internal/utils/misc.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, tuple[str, tuple[str | None, str | None]]]`, found `tuple[Literal[b""], tuple[str, tuple[str | None, str | None]]]`
+ src/pip/_internal/utils/subprocess.py:29:33: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[str | HiddenText]`, found `(HiddenText & Top[list[Unknown]]) | list[str | HiddenText]`
- src/pip/_internal/vcs/git.py:512:19: error[unsupported-operator] Operator `+` is unsupported between objects of type `str` and `Literal[b""]`
- src/pip/_internal/vcs/versioncontrol.py:400:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- src/pip/_vendor/pkg_resources/__init__.py:2849:21: error[unresolved-attribute] Type `~None` has no attribute `strip`
+ src/pip/_vendor/pkg_resources/__init__.py:2840:13: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str | None, Iterable[str]]]`
- src/pip/_vendor/urllib3/util/url.py:373:26: warning[possibly-missing-attribute] Attribute `groups` on type `Match[str] | None` may be missing
- Found 465 diagnostics
+ Found 460 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/cmd/ci.py:716:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ lib/spack/spack/cmd/commands.py:571:24: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `object`
- lib/spack/spack/llnl/url.py:130:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str]`, found `tuple[Literal[b""], str]`
- lib/spack/spack/oci/oci.py:48:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- lib/spack/spack/oci/opener.py:288:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b""]`
+ lib/spack/spack/solver/requirements.py:203:49: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `@Todo | list[Unknown]`
- lib/spack/spack/vendor/jinja2/async_utils.py:65:27: warning[redundant-cast] Value is already of type `AsyncIterable[V@auto_aiter]`
+ lib/spack/spack/vendor/jinja2/filters.py:219:9: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str, Any]]`
+ lib/spack/spack/vendor/jinja2/loaders.py:187:28: error[no-matching-overload] No overload of function `fspath` matches arguments
- lib/spack/spack/vendor/jinja2/loaders.py:606:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 7523 diagnostics
+ Found 7521 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/async_utils.py:91:16: error[call-non-callable] Object of type `object` is not callable
- src/jinja2/filters.py:142:17: warning[redundant-cast] Value is already of type `HasHTML`
+ src/jinja2/filters.py:169:9: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str, Any]]`
- src/jinja2/filters.py:1050:17: warning[redundant-cast] Value is already of type `HasHTML`
+ src/jinja2/loaders.py:190:28: error[no-matching-overload] No overload of function `fspath` matches arguments
+ src/jinja2/loaders.py:639:25: error[no-matching-overload] No overload of function `fspath` matches arguments
- Found 184 diagnostics
+ Found 186 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/asyncio/server.py:934:37: warning[redundant-cast] Value is already of type `Iterable[tuple[str, str]]`
- src/websockets/legacy/auth.py:163:37: warning[redundant-cast] Value is already of type `Iterable[tuple[str, str]]`
- src/websockets/sync/server.py:706:37: warning[redundant-cast] Value is already of type `Iterable[tuple[str, str]]`
- Found 41 diagnostics
+ Found 38 diagnostics

twine (https://github.com/pypa/twine)
+ twine/repository.py:78:43: error[no-matching-overload] No overload of bound method `join` matches arguments
- twine/utils.py:198:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- twine/utils.py:199:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 11 diagnostics
+ Found 10 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/error/graphql_error.py:144:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_directives.py:74:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_directives.py:160:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_schema.py:384:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_validation.py:435:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/utilities/test_ast_to_dict.py:32:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 424 diagnostics
+ Found 418 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/file_storage.py:126:19: error[no-matching-overload] No overload of function `fspath` matches arguments
+ src/werkzeug/datastructures/headers.py:518:51: error[invalid-argument-type] Argument to bound method `getlist` is incorrect: Expected `str`, found `object`
- src/werkzeug/datastructures/headers.py:518:51: error[invalid-argument-type] Argument to bound method `getlist` is incorrect: Expected `Never`, found `str`
+ src/werkzeug/datastructures/headers.py:518:51: error[invalid-argument-type] Argument to bound method `getlist` is incorrect: Expected `Never`, found `object`
+ src/werkzeug/middleware/shared_data.py:121:13: error[invalid-assignment] Object of type `ItemsView[object, object]` is not assignable to `Mapping[str, str | tuple[str, str]] | Iterable[tuple[str, str | tuple[str, str]]]`
- src/werkzeug/routing/map.py:768:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/werkzeug/test.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `bytes`
- src/werkzeug/urls.py:110:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/werkzeug/urls.py:165:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/werkzeug/utils.py:419:24: warning[redundant-cast] Value is already of type `PathLike[str] | str`
+ src/werkzeug/wsgi.py:244:13: error[invalid-assignment] Object of type `list[Unknown | (() -> None) | (Iterable[() -> None] & (() -> object))]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
+ src/werkzeug/wsgi.py:249:13: warning[possibly-missing-attribute] Attribute `insert` on type `list[Unknown] | None | (() -> None) | Iterable[() -> None]` may be missing
+ tests/test_routing.py:615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Rule]`, found `list[Unknown | Submount | EndpointPrefix | Subdomain]`
+ tests/test_send_file.py:131:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:223:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:227:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `dict[Unknown | str, Unknown | str]`
+ tests/test_test.py:702:16: error[no-matching-overload] No overload of bound method `join` matches arguments
+ tests/test_test.py:728:12: error[no-matching-overload] No overload of bound method `join` matches arguments
+ tests/test_wrappers.py:295:12: error[no-matching-overload] No overload of bound method `join` matches arguments
+ tests/test_wsgi.py:261:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:265:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:270:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:274:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:280:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:288:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:297:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:305:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[bytes]`, found `Iterable[str] | Iterable[bytes]`
+ tests/test_wsgi.py:338:12: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 366 diagnostics
+ Found 382 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/downloadermiddlewares/httpproxy.py:57:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[bytes | None, str]`, found `tuple[Unknown | None, Literal[b""]]`
+ scrapy/http/headers.py:37:9: error[invalid-assignment] Object of type `ItemsView[object, object] | (Iterable[tuple[AnyStr@update, Any]] & ~Top[Mapping[Unknown, object]])` is not assignable to `Mapping[AnyStr@update, Any] | Iterable[tuple[AnyStr@update, Any]]`
+ scrapy/http/headers.py:39:21: error[not-iterable] Object of type `AnyStr@update` may not be iterable
+ scrapy/utils/datatypes.py:90:9: error[invalid-assignment] Object of type `ItemsView[object, object] | (Iterable[tuple[AnyStr@update, Any]] & ~Top[Mapping[Unknown, object]])` is not assignable to `Mapping[AnyStr@update, Any] | Iterable[tuple[AnyStr@update, Any]]`
+ scrapy/utils/datatypes.py:91:66: error[not-iterable] Object of type `AnyStr@update` may not be iterable
- scrapy/utils/url.py:195:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ tests/test_contracts.py:571:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPreProcess]`
+ tests/test_contracts.py:585:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[type[Contract]]`, found `list[Unknown | CustomFailContractPostProcess]`
- Found 1063 diagnostics
+ Found 1067 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_capture.py:1663:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 491 diagnostics
+ Found 490 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/bundle.py:300:13: error[unresolved-attribute] Type `set[Unknown]` has no attribute `append`
- dulwich/client.py:1885:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- dulwich/client.py:2143:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- dulwich/client.py:2997:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ dulwich/porcelain.py:924:13: error[invalid-assignment] Object of type `list[Unknown | (str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | ... omitted 3 union elements]` is not assignable to `Sequence[str | bytes | PathLike[str]] | bytes | PathLike[str] | None`
+ dulwich/porcelain.py:925:18: error[not-iterable] Object of type `Sequence[str | bytes | PathLike[str]] | (list[Unknown | str] & ~PathLike[object]) | bytes | PathLike[str] | None` may not be iterable
+ dulwich/worktree.py:283:13: error[invalid-assignment] Object of type `list[Unknown | str | bytes | (Iterable[str | bytes | PathLike[str]] & PathLike[object]) | PathLike[str]]` is not assignable to `Iterable[str | bytes | PathLike[str]] | bytes | PathLike[str]`
+ dulwich/worktree.py:284:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Iterable[str | bytes | PathLike[str]] | bytes | PathLike[str]`
- Found 184 diagnostics
+ Found 186 diagnostics

starlette (https://github.com/encode/starlette)
- tests/test_formparsers.py:297:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field0"\r\n\r\nvalue0\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="file.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field1"\r\n\r\nvalue1\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:297:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field0"\r\n\r\nvalue0\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="file.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field1"\r\n\r\nvalue1\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
- tests/test_formparsers.py:372:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:372:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
- tests/test_formparsers.py:396:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe7\x94\xbb\xe5\x83\x8f.jpg"\r\nContent-Type: image/jpeg\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:396:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe7\x94\xbb\xe5\x83\x8f.jpg"\r\nContent-Type: image/jpeg\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
- tests/test_formparsers.py:420:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--20b303e711c4ab8c443184ac833ab00f\r\nContent-Disposition: form-data; name="value"\r\n\r\nTransf\xc3\xa9rer\r\n--20b303e711c4ab8c443184ac833ab00f--\r\n']`
+ tests/test_formparsers.py:420:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'--20b303e711c4ab8c443184ac833ab00f\r\nContent-Disposition: form-data; name="value"\r\n\r\nTransf\xc3\xa9rer\r\n--20b303e711c4ab8c443184ac833ab00f--\r\n']`
- tests/test_formparsers.py:494:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'Content-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n']`
+ tests/test_formparsers.py:494:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'Content-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n']`
- tests/test_formparsers.py:522:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; ="field0"\r\n\r\nvalue0\r\n']`
+ tests/test_formparsers.py:522:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str] | bytes] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; ="field0"\r\n\r\nvalue0\r\n']`
+ tests/test_formparsers.py:554:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:581:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:610:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:638:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:668:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:698:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_formparsers.py:715:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 137 diagnostics
+ Found 144 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/local_run.py:983:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ paasta_tools/contrib/check_orphans.py:152:26: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ paasta_tools/utils.py:608:17: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ paasta_tools/utils.py:616:35: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown]]`
+ paasta_tools/utils.py:629:16: error[invalid-return-type] Return type does not match returned value: expected `Iterable[DockerParameter]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
- paasta_tools/utils.py:2939:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, str]`, found `tuple[int | @Todo | None, LiteralString]`
+ paasta_tools/utils.py:2939:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[int, str]`, found `tuple[int | @Todo | None, str]`
- paasta_tools/utils.py:4181:19: warning[redundant-cast] Value is already of type `list[str]`
- Found 912 diagnostics
+ Found 914 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/dicts.py:241:25: warning[possibly-missing-attribute] Attribute `obj` on type `(_T@walk & Unknown & ~None) | (Iterable[_T@walk] & Unknown) | (_T@walk & _dummy) | (Iterable[_T@walk] & _dummy)` may be missing
+ kopf/_cogs/structs/dicts.py:241:25: warning[possibly-missing-attribute] Attribute `obj` on type `(_T@walk & Unknown & ~None) | (Iterable[_T@walk | Iterable[_T@walk]] & Unknown) | (_T@walk & _dummy) | (Iterable[_T@walk | Iterable[_T@walk]] & _dummy)` may be missing
- kopf/_kits/webhooks.py:272:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 43 diagnostics
+ Found 42 diagnostics

ignite (https://github.com/pytorch/ignite)
- ignite/engine/engine.py:953:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/handlers/time_profilers.py:99:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/ignite/metrics/test_accuracy.py:445:22: warning[possibly-missing-attribute] Attribute `item` on type `Unknown | int | float` may be missing
+ tests/ignite/metrics/test_accuracy.py:445:22: warning[possibly-missing-attribute] Attribute `item` on type `Unknown | int | float | str` may be missing
- Found 2167 diagnostics
+ Found 2165 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/__init__.py:124:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 193 diagnostics
+ Found 192 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/MainClass.py:640:27: error[no-matching-overload] No overload of bound method `join` matches arguments
- github/Requester.py:499:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/log.py:131:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | str | None].__setitem__(key: Unknown | str, value: Unknown | str | None, /) -> None) | (bound method dict[Unknown | str, Unknown | str | int].__setitem__(key: Unknown | str, value: Unknown | str | int, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None])` cannot be called with a key of type `Literal["level"]` and a value of type `Unknown | Literal[20]` on object of type `Unknown | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | str | None] | ... omitted 3 union elements`
- Found 321 diagnostics
+ Found 322 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_decorators_v1.py:174:12: error[invalid-return-type] Return type does not match returned value: expected `V2CoreBeforeRootValidator`, found `def _wrapper2(fields_tuple: tuple[Any, ...], _: ValidationInfo[Any | None]) -> tuple[Any, ...]`
- Found 769 diagnostics
+ Found 768 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/httputil.py:799:5: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
+ tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | dict[str, Any] | str`
+ tornado/routing.py:355:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `@Todo | dict[str, Any] | str`
- Found 244 diagnostics
+ Found 245 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/vcs/git/backend.py:578:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ tests/installation/test_installer.py:369:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Any | dict[Unknown | str, Unknown | str | bool | list[Unknown]] | str` may be missing
+ tests/installation/test_installer.py:370:26: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["name"]` on object of type `str`
+ tests/installation/test_installer.py:372:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Any | dict[Unknown | str, Unknown | str | bool | list[Unknown]] | str` may be missing
+ tests/installation/test_installer.py:3280:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Any | dict[Unknown | str, Unknown | str | bool | list[Unknown] | dict[Unknown | str, Unknown | str]] | dict[Unknown | str, Unknown | str | bool | list[Unknown]] | str` may be missing
- tests/integration/test_utils_vcs_git.py:432:43: error[unresolved-attribute] Type `Literal[b""]` has no attribute `encode`
- Found 988 diagnostics
+ Found 990 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/transaction_processor/level_5_actions_utest.py:199:55: error[invalid-argument-type] Argument to function `verify_blocks` is incorrect: Expected `Iterable[bytes]`, found `list[Unknown | str]`
- Found 315 diagnostics
+ Found 316 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/owners.py:242:16: error[invalid-return-type] Return type does not match returned value: expected `None`, found `LiteralString`
- Found 183 diagnostics
+ Found 182 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/pyspark/container.py:589:28: error[not-iterable] Object of type `(Unknown & ~None & ~(() -> object)) | (list[str] & ~(() -> object)) | (((...) -> Unknown) & ~str & ~(() -> object)) | (list[Unknown | str] & ~(() -> object))` may not be iterable
- pandera/typing/pandas.py:233:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_model.py:691:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_model.py:697:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pandas/test_model.py:702:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1595 diagnostics
+ Found 1592 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/output/sanitization.py:54:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/schemathesis/transport/prepare.py:63:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- src/schemathesis/transport/prepare.py:90:16: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `Literal[b""]`
- Found 321 diagnostics
+ Found 318 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ src/urllib3/fields.py:281:13: error[invalid-assignment] Object of type `dict_items[object, object]` is not assignable to `Iterable[tuple[str, @Todo | None]]`
+ src/urllib3/util/url.py:423:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_retry.py:250:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:534:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:539:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:544:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:549:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:556:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:561:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:566:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_util.py:571:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 366 diagnostics
+ Found 359 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/lib/interface_ext/_internal.py:132:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
- src/antidote/lib/interface_ext/_internal.py:134:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
- tests/core/test_wiring.py:204:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_wiring.py:317:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 304 diagnostics
+ Found 300 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/jsonld_context.py:229:19: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[MutableMapping[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `Literal["@id"]` on object of type `Top[MutableMapping[Unknown, Unknown]]`
- schema_salad/metaschema.py:384:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- schema_salad/metaschema.py:393:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- schema_salad/python_codegen_support.py:381:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- schema_salad/python_codegen_support.py:390:9: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
- Found 171 diagnostics
+ Found 166 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/tlsconfig.py:615:21: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str | None`
- mitmproxy/http.py:1087:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mitmproxy/net/http/url.py:54:5: error[invalid-assignment] Object of type `ParseResult | ParseResultBytes` is not assignable to `ParseResult`
+ mitmproxy/utils/arg_check.py:143:29: error[no-matching-overload] No overload of bound method `join` matches arguments
- test/mitmproxy/proxy/layers/http/test_http3.py:564:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `stream` of type `((bytes, /) -> Iterable[bytes]) | bool`
+ test/mitmproxy/proxy/layers/http/test_http3.py:564:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `stream` of type `((bytes, /) -> Iterable[bytes] | bytes) | bool`
- test/mitmproxy/test_http.py:149:57: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `Literal[42]`
+ test/mitmproxy/test_http.py:149:57: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `Headers | dict[str | bytes, str | bytes] | Iterable[tuple[bytes, bytes]]`, found `Literal[42]`
- test/mitmproxy/test_http.py:517:27: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `Literal[42]`
+ test/mitmproxy/test_http.py:517:27: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `Headers | Mapping[str, str | bytes] | Iterable[tuple[bytes, bytes]]`, found `Literal[42]`
+ test/mitmproxy/test_http.py:588:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | list[Unknown | bytes]]`
+ test/mitmproxy/test_http.py:821:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[bytes, bytes]]`, found `list[Unknown | tuple[bytes, str]]`

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_server_cursor_base.py:217:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Template` and `(@Todo & Composable) | SQL`
+ psycopg/psycopg/_server_cursor.py:61:31: error[invalid-argument-type] Argument to function `__init__` is incorrect: Expected `RowFactory[tuple[Any, ...]]`, found `(RowFactory[Row@ServerCursor] & ~AlwaysFalsy) | RowFactory[Any]`
+ psycopg/psycopg/_server_cursor_async.py:60:31: error[invalid-argument-type] Argument to function `__init__` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]]`, found `(AsyncRowFactory[Row@AsyncServerCursor] & ~AlwaysFalsy) | AsyncRowFactory[Any]`
+ psycopg/psycopg/_typeinfo.py:102:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RowFactory[tuple[Any, ...]]`, found `def dict_row(cursor: BaseCursor[Any, Any]) -> RowMaker[dict[str, Any]]`
+ psycopg/psycopg/_typeinfo.py:120:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AsyncRowFactory[tuple[Any, ...]]`, found `def dict_row(cursor: BaseCursor[Any, Any]) -> RowMaker[dict[str, Any]]`
- tests/types/test_multirange.py:45:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/types/test_multirange.py:53:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/types/test_multirange.py:56:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/typing_example.py:165:21: error[invalid-argument-type] Argument to bound method `connect` is incorrect: Expected `RowFactory[tuple[Any, ...]] | None`, found `def dict_row(cursor: BaseCursor[Any, Any]) -> RowMaker[dict[str, Any]]`
- Found 685 diagnostics
+ Found 686 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/study/_multi_objective.py:26:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `@Todo | list[int | float] | None`
+ optuna/study/_multi_objective.py:32:50: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `@Todo | list[int | float] | None`
+ optuna/study/study.py:496:13: error[invalid-argument-type] Argument to function `_optimize` is incorrect: Expected `tuple[type[Exception], ...]`, found `tuple[object, ...]`
- Found 565 diagnostics
+ Found 568 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/channel.py:632:31: error[unresolved-attribute] Type `object` has no attribute `id`
- discord/channel.py:639:39: error[unresolved-attribute] Type `object` has no attribute `id`
- discord/channel.py:1276:31: error[unresolved-attribute] Type `object` has no attribute `id`
- discord/channel.py:1283:39: error[unresolved-attribute] Type `object` has no attribute `id`
- discord/threads.py:491:26: error[unresolved-attribute] Type `object` has no attribute `id`
- discord/threads.py:498:39: error[unresolved-attribute] Type `object` has no attribute `id`
+ discord/utils.py:815:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 514 diagnostics
+ Found 509 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/builders/linkcheck.py:758:20: error[invalid-return-type] Return type does not match returned value: expected `str | None`, found `Literal[b""]`
- sphinx/config.py:634:16: error[invalid-return-type] Return type does not match returned value: expected `frozenset[type] | ENUM`, found `(frozenset[object] & ~AlwaysFalsy) | (ENUM & ~AlwaysFalsy)`
+ sphinx/config.py:634:16: error[invalid-return-type] Return type does not match returned value: expected `frozenset[type] | ENUM`, found `(Collection[type] & frozenset[object] & ~AlwaysFalsy) | (ENUM & ~AlwaysFalsy)`
- sphinx/environment/adapters/toctree.py:261:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/environment/adapters/toctree.py:270:24: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Node`
- sphinx/environment/adapters/toctree.py:274:25: error[unresolved-attribute] Type `Node` has no attribute `pop`
- sphinx/ext/autodoc/_directive_options.py:226:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/ext/graphviz.py:268:32: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `str`, found `Literal[b""]`
- sphinx/ext/intersphinx/_load.py:450:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- sphinx/ext/intersphinx/_load.py:471:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- sphinx/util/_uri.py:11:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
+ sphinx/util/template.py:46:13: error[invalid-assignment] Object of type `list[Unknown | str | (Sequence[str | PathLike[str]] & PathLike[object])]` is not assignable to `Sequence[str | PathLike[str]]`
- sphinx/writers/text.py:448:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, list[str]]`, found `tuple[Unknown, list[str] | list[LiteralString]]`
- Found 502 diagnostics
+ Found 493 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/converter/trade_converter_kraken.py:82:39: error[invalid-argument-type] Argument to function `trades_convert_types` is incorrect: Expected `DataFrame`, found `Series[Any]`
- Found 405 diagnostics
+ Found 404 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/config/config_options.py:524:20: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- mkdocs/structure/pages.py:516:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `Literal[b""]`
- Found 208 diagnostics
+ Found 206 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/computation/fit.py:407:9: error[invalid-assignment] Object of type `list[Unknown | Iterable[str | DataArray] | DataArray]` is not assignable to `Iterable[str | DataArray] | DataArray`
+ xarray/core/parallel.py:380:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/structure/combine.py:1048:12: error[invalid-return-type] Return type does not match returned value: expected `Dataset | DataArray`, found `DataTree`
- xarray/structure/merge.py:825:23: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Dataset | Coordinates | None`, found `DataTree`
- xarray/tests/test_dataset.py:1876:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/tests/test_dataset.py:5038:53: error[invalid-argument-type] Argument to bound method `squeeze` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str]`
+ xarray/tests/test_dataset.py:5038:53: error[invalid-argument-type] Argument to bound method `squeeze` is incorrect: Expected `int | Iterable[int] | None`, found `Unknown | list[Unknown | str]`
- xarray/tests/test_merge.py:541:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_merge.py:544:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_merge.py:546:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_merge.py:553:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:553:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:556:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:556:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:558:30: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:558:30: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:559:30: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:559:30: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:562:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:562:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:736:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Argument type `DataTree` does not satisfy upper bound `Dataset` of type variable `Self`
- xarray/tests/test_merge.py:736:13: error[invalid-argument-type] Argument to bound method `identical` is incorrect: Expected `Dataset`, found `DataTree`
- xarray/tests/test_merge.py:881:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1626 diagnostics
+ Found 1611 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_io_windows.py:328:16: error[call-non-callable] Object of type `object` is not callable
+ src/trio/_core/_io_windows.py:373:24: error[call-non-callable] Object of type `object` is not callable
+ src/trio/_highlevel_open_unix_stream.py:63:28: error[no-matching-overload] No overload of function `fspath` matches arguments
- src/trio/_tests/type_tests/task_status.py:19:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 647 diagnostics
+ Found 649 diagnostics

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type, ...]` is not assignable to `list[type] | None`
+ references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type | Unknown, ...]` is not assignable to `list[type] | None`
- torchvision/datasets/_stereo_matching.py:496:110: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Sequence[object]`
+ torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `(Collection[str] & Sequence[object]) | list[Unknown]`
- Found 1481 diagnostics
+ Found 1480 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/backend/ninjabackend.py:1445:13: error[unresolved-attribute] Type `str` has no attribute `write`
+ mesonbuild/backend/ninjabackend.py:1445:13: warning[possibly-missing-attribute] Attribute `write` on type `str | Unknown` may be missing
- mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/traceparser.py:330:58: error[invalid-argument-type] Argument is incorrect: Expected `list[str]`, found `list[LiteralString]`
- mesonbuild/cmake/traceparser.py:591:40: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, list[str]]`, found `tuple[Unknown, list[LiteralString]]`
- mesonbuild/cmake/traceparser.py:597:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[str, list[str]]`, found `tuple[Unknown, list[LiteralString]]`
+ mesonbuild/dependencies/configtool.py:140:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[str | AnsiDecorator]` and `list[Unknown | AnsiDecorator | str | None]`
- mesonbuild/wrap/wrap.py:97:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Literal[b""]`
- mesonbuild/wrap/wrap.py:788:21: error[invalid-assignment] Object of type `Literal[b""]` is not assignable to `str`
+ run_project_tests.py:899:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[tuple[str, str, bool, bool]]` and `list[Unknown | tuple[Unknown, None, bool, bool]]`
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (json_node) -> Unknown]` with length 2
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (node_list) -> Unknown]` with length 2
+ unittests/allplatformstests.py:3861:26: error[index-out-of-bounds] Index 2 is out of bounds for tuple `tuple[str, (kwargs) -> Unknown]` with length 2
- Found 891 diagnostics
+ Found 887 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/helpers_shared.py:341:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[str, Any]`, found `Top[Mapping[Unknown, object]]`
+ pymongo/helpers_shared.py:341:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[str, Any]`, found `Iterable[str] & Top[Mapping[Unknown, object]]`

pyodide (https://github.com/pyodide/pyodide)
+ pyodide-build/pyodide_build/pywasmcross.py:379:27: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 985 diagnostics
+ Found 986 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/schema/name_converter.py:121:44: error[unresolved-attribute] Type `StrawberryType` has no attribute `__strawberry_definition__`
+ strawberry/schema/name_converter.py:121:44: warning[possibly-missing-attribute] Attribute `__strawberry_definition__` on type `StrawberryType | (@Todo & ~Top[LazyType[Unknown, Unknown]])` may be missing

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/stages.py:272:17: error[no-matching-overload] No overload of bound method `join` matches arguments
+ tests/unittests/sources/test_configdrive.py:1003:44: warning[possibly-missing-attribute] Attribute `upper` on type `Unknown | str | None` may be missing
+ tests/unittests/test_ds_identify.py:1002:9: error[no-matching-overload] No overload of bound method `update` matches arguments
+ tests/unittests/test_ds_identify.py:1018:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- tests/unittests/test_ds_identify.py:1037:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]) | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None])` cannot be called with a key of type `Literal["etc/cloud/ds-identify.cfg"]` and a value of type `LiteralString` on object of type `Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str | int]] | dict[Unknown | str, Unknown | str] | list[Unknown | str]`
+ tests/unittests/test_ds_identify.py:1037:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown | dict[Unknown | str, Unknown | str | int], /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | dict[Unknown | str, Unknown | str | int]], /) -> None]) | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None])` cannot be called with a key of type `Literal["etc/cloud/ds-identify.cfg"]` and a value of type `str` on object of type `Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str | int]] | dict[Unknown | str, Unknown | str] | list[Unknown | str]`
- Found 839 diagnostics
+ Found 843 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/core/waitinglist.py:243:41: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- openlibrary/coverstore/utils.py:92:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, dict[str, str]]`, found `tuple[Literal[b""], dict[@Todo, @Todo]]`
+ openlibrary/plugins/worksearch/subjects.py:303:13: error[invalid-argument-type] Argument to function `run_solr_query` is incorrect: Expected `bool | Iterable[str]`, found `(Unknown & ~AlwaysTruthy) | Literal[False] | list[Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | str | int]]`
- Found 849 diagnostics
+ Found 850 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `UUID | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `UUID | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `TaskRun | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `TaskRun | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `Literal["state", "result"]`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `Literal["state", "result"]`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, set[RunInput]] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, set[RunInput]] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:379:47: error[invalid-argument-type] Argument to function `run_task_async` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `UUID | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `UUID | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `TaskRun | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `TaskRun | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `Literal["state", "result"]`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `Literal["state", "result"]`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, set[RunInput]] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, set[RunInput]] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
- src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | None`
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:381:34: error[invalid-argument-type] Argument to function `run_task_sync` is incorrect: Expected `dict[str, Any] | None`, found `Any | UUID | Iterable[PrefectFuture[Any]] | ... omitted 4 union elements`
+ src/prefect/cli/deploy/_core.py:284:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- src/prefect/client/schemas/schedules.py:269:34: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | None`, found `datetime`
+ src/prefect/client/schemas/schedules.py:269:34: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | Iterable[weekday] | None`, found `datetime`
- src/prefect/client/schemas/schedules.py:279:48: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | None`, found `datetime`
+ src/prefect/client/schemas/schedules.py:279:48: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | Iterable[weekday] | None`, found `datetime`
- src/prefect/client/schemas/schedules.py:290:50: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | None`, found `datetime`
+ src/prefect/client/schemas/schedules.py:290:50: error[invalid-argument-type] Argument to bound method `replace` is incorrect: Expected `int | weekday | Iterable[int] | Iterable[weekday] | None`, found `datetime`
+ src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Iterable[PrefectFuture[Unknown]] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1546:48: error[invalid-argument-type] Argument to function `run_generator_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
- src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
+ src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Iterable[PrefectFuture[Any]] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `Literal["state", "result"]`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1548:47: error[invalid-argument-type] Argument to function `run_generator_flow_sync` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1550:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_engine.py:1550:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `FlowRun | None`, found `@Todo | FlowRun | None | ... omitted 3 union elements`
- src/prefect/flow_engine.py:1550:38: error[invalid-argument-type] Argument to function `run_flow_async` is incorrect: Expected `dict[str, Any] | None`, found `@Todo | FlowRun | None | Iterable[PrefectFuture[R@run_flow]]`
+ src/prefect/flow_e...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-09-15 16:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fsubtyping-between-protocols)

### Merging #20368 will **degrade performances by 4.64%**

<sub>Comparing <code>alex/subtyping-between-protocols</code> (6249f6c) with <code>main</code> (b9c84ad)</sub>



### Summary

`❌ 1` regression  
`✅ 50` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fsubtyping-between-protocols)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Instrumentation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fsubtyping-between-protocols?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation) | 182.1 ms | 191 ms | -4.64% |


---

_Comment by @github-actions[bot] on 2025-09-15 16:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-25 14:13_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-25 14:13_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-26 16:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-26 16:06_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-26 17:31_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-26 17:31_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-29 11:36_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-29 11:36_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-03 22:19_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-03 22:19_

---

_Comment by @AlexWaygood on 2025-10-04 16:28_

# Ecosystem analysis

I was expecting this PR to reduce diagnostics across the ecosystem overall, but it seems like on net it leads to more diagnostics, which is unfortunate. I _have_ spent a while now trying to reduce the number of underlying issues here, and the diagnostic diff _is_ much better than it was when I first opened the PR (we've gone from +297,-128 a few weeks ago, to +280,-157 now). PRs such as https://github.com/astral-sh/ruff/pull/20602, https://github.com/astral-sh/ruff/pull/20629, https://github.com/astral-sh/ruff/pull/20603, https://github.com/astral-sh/ruff/pull/20591 and github.com/astral-sh/ruff/pull/20593 have all helped reduce the impact of this PR by fixing underlying issues. It's possible I could carry on with this a little bit, but I think the remaining diagnostics are mostly acceptable and/or hard to fix.

Many new diagnostics on this PR are due to narrowing behaviour that is technically correct, but which I worry may appear quite pedantic to users. For example, an adapted repro from `aiohttp` is:

```py
from typing import Iterable
import socket

def f(x: Iterable[socket.socket] | socket.socket):
    if isinstance(x, Iterable):
        reveal_type(x)  # revealed: Iterable[socket] | (socket & Iterable[object])
        for s in x:
            reveal_type(s)  # revealed: object
            needs_a_socket(s)  # error: "Expected `socket`, found `object`"
    else:
        needs_a_socket(x)

def needs_a_socket(s: socket.socket): ...
```

This is, of course, strictly correct: typeshed (correctly) does not mark `socket.socket` as `@final`, since it can be subclassed at runtime. It's entirely possible that a user _could_ create a subclass of `socket.socket` that was iterable, so inferring `object` for `s` here is entirely correct. Unfortunately it also differs from the behaviour of all currently existing type checkers, and I'm not totally sure how we can tailor our diagnostics so that it's clear to users why we're unexpectedly inferring `object` in many places. Still, this is consistent behaviour with how we do narrowing elsewhere, so it's not really a _bug_ in this PR.

There are lots of diagnostics like this going away, due to https://github.com/astral-sh/ty/issues/733 being fixed by this PR:

```
[error] invalid-return-type - Return type does not match returned value: expected `str`, found `Literal[b""]`
```

And there's also lots of churning regarding diagnostics for `str.join`, due to https://github.com/astral-sh/ty/issues/306 being fixed by this PR.

Not an exhaustive analysis, but here's a spot check of some of the other diagnostics in the ecosystem that stood out to me while I skimmed through:

## bokeh

One of the bokeh hits is:

```
[error] invalid-return-type - [:510:16](https://github.com/bokeh/bokeh/blob/17c46de8f0df63051a08e1c756587702e249ea7f/src/bokeh/model/model.py#L510) - Return type does not match returned value: expected `Iterable[bokeh.model.model.Model]`, found `Iterable[bokeh.model.model.Model]`
```

Debug-printing the types gives:

<details>

```
error[invalid-return-type]: Return type does not match returned value
   --> src/bokeh/model/model.py:510:16
    |
508 |         '''
509 |         from ..core.query import find
510 |         return find(self.references(), selector)
    |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `ProtocolInstance(
    ProtocolInstanceType {
        inner: FromClass(
            Generic(
                GenericAlias {
                    origin: ClassLiteral {
                        name: Name("Iterable"),
                        body_scope: ScopeId {
                            [salsa id]: Id(534a),
                            file: File {
                                path: Vendored(
                                    VendoredPathBuf(
                                        "stdlib/typing.pyi",
                                    ),
                                ),
                                status: Exists,
                                permissions: Some(
                                    292,
                                ),
                                revision: FileRevision(
                                    1689436316,
                                ),
                            },
                            file_scope_id: FileScopeId(
                                106,
                            ),
                        },
                        known: Some(
                            Iterable,
                        ),
                        deprecated: None,
                        dataclass_params: None,
                        dataclass_transformer_params: None,
                    },
                    specialization: Specialization {
                        generic_context: GenericContext {
                            variables_inner: {
                                BoundTypeVarInstance {
                                    typevar: TypeVarInstance {
                                        name: Name("_T_co"),
                                        definition: Some(
                                            Definition {
                                                [salsa id]: Id(6c3d),
                                                file: File {
                                                    path: Vendored(
                                                        VendoredPathBuf(
                                                            "stdlib/typing.pyi",
                                                        ),
                                                    ),
                                                    status: Exists,
                                                    permissions: Some(
                                                        292,
                                                    ),
                                                    revision: FileRevision(
                                                        1689436316,
                                                    ),
                                                },
                                                file_scope: FileScopeId(
                                                    0,
                                                ),
                                                place: Symbol(
                                                    ScopedSymbolId(
                                                        74,
                                                    ),
                                                ),
                                                kind: Assignment(
                                                    AssignmentDefinitionKind {
                                                        target_kind: Single,
                                                        value: AstNodeRef {
                                                            kind: ExprCall,
                                                            range: 21869..21901,
                                                        },
                                                        target: AstNodeRef {
                                                            kind: ExprName,
                                                            range: 21861..21866,
                                                        },
                                                    },
                                                ),
                                                is_reexported: true,
                                            },
                                        ),
                                        _bound_or_constraints: None,
                                        explicit_variance: Some(
                                            Covariant,
                                        ),
                                        _default: None,
                                        kind: Legacy,
                                    },
                                    binding_context: Definition(
                                        Definition {
                                            [salsa id]: Id(6c8f),
                                            file: File {
                                                path: Vendored(
                                                    VendoredPathBuf(
                                                        "stdlib/typing.pyi",
                                                    ),
                                                ),
                                                status: Exists,
                                                permissions: Some(
                                                    292,
                                                ),
                                                revision: FileRevision(
                                                    1689436316,
                                                ),
                                            },
                                            file_scope: FileScopeId(
                                                0,
                                            ),
                                            place: Symbol(
                                                ScopedSymbolId(
                                                    108,
                                                ),
                                            ),
                                            kind: Class(
                                                AstNodeRef {
                                                    kind: StmtClassDef,
                                                    range: 27489..27607,
                                                },
                                            ),
                                            is_reexported: true,
                                        },
                                    ),
                                }: GenericContextTypeVarOptions {
                                    should_promote_literals: false,
                                },
                            },
                        },
                        types: [
                            NominalInstance(
                                NominalInstanceType(
                                    NonTuple(
                                        NonGeneric(
                                            ClassLiteral {
                                                name: Name("Model"),
                                                body_scope: ScopeId {
                                                    [salsa id]: Id(1001),
                                                    file: File {
                                                        path: System(
                                                            "/Users/alexw/dev/bokeh/src/bokeh/model/model.py",
                                                        ),
                                                        status: Exists,
                                                        permissions: Some(
                                                            33188,
                                                        ),
                                                        revision: FileRevision(
                                                            32450546247609911550292892181,
                                                        ),
                                                    },
                                                    file_scope_id: FileScopeId(
                                                        1,
                                                    ),
                                                },
                                                known: None,
                                                deprecated: None,
                                                dataclass_params: None,
                                                dataclass_transformer_params: None,
                                            },
                                        ),
                                    ),
                                ),
                            ),
                        ],
                        materialization_kind: None,
                        tuple_inner: None,
                    },
                },
            ),
        ),
        _phantom: PhantomData<()>,
    },
)`, found `ProtocolInstance(
    ProtocolInstanceType {
        inner: FromClass(
            Generic(
                GenericAlias {
                    origin: ClassLiteral {
                        name: Name("Iterable"),
                        body_scope: ScopeId {
                            [salsa id]: Id(534a),
                            file: File {
                                path: Vendored(
                                    VendoredPathBuf(
                                        "stdlib/typing.pyi",
                                    ),
                                ),
                                status: Exists,
                                permissions: Some(
                                    292,
                                ),
                                revision: FileRevision(
                                    1689436316,
                                ),
                            },
                            file_scope_id: FileScopeId(
                                106,
                            ),
                        },
                        known: Some(
                            Iterable,
                        ),
                        deprecated: None,
                        dataclass_params: None,
                        dataclass_transformer_params: None,
                    },
                    specialization: Specialization {
                        generic_context: GenericContext {
                            variables_inner: {
                                BoundTypeVarInstance {
                                    typevar: TypeVarInstance {
                                        name: Name("_T_co"),
                                        definition: Some(
                                            Definition {
                                                [salsa id]: Id(6c3d),
                                                file: File {
                                                    path: Vendored(
                                                        VendoredPathBuf(
                                                            "stdlib/typing.pyi",
                                                        ),
                                                    ),
                                                    status: Exists,
                                                    permissions: Some(
                                                        292,
                                                    ),
                                                    revision: FileRevision(
                                                        1689436316,
                                                    ),
                                                },
                                                file_scope: FileScopeId(
                                                    0,
                                                ),
                                                place: Symbol(
                                                    ScopedSymbolId(
                                                        74,
                                                    ),
                                                ),
                                                kind: Assignment(
                                                    AssignmentDefinitionKind {
                                                        target_kind: Single,
                                                        value: AstNodeRef {
                                                            kind: ExprCall,
                                                            range: 21869..21901,
                                                        },
                                                        target: AstNodeRef {
                                                            kind: ExprName,
                                                            range: 21861..21866,
                                                        },
                                                    },
                                                ),
                                                is_reexported: true,
                                            },
                                        ),
                                        _bound_or_constraints: None,
                                        explicit_variance: Some(
                                            Covariant,
                                        ),
                                        _default: None,
                                        kind: Legacy,
                                    },
                                    binding_context: Definition(
                                        Definition {
                                            [salsa id]: Id(6c8f),
                                            file: File {
                                                path: Vendored(
                                                    VendoredPathBuf(
                                                        "stdlib/typing.pyi",
                                                    ),
                                                ),
                                                status: Exists,
                                                permissions: Some(
                                                    292,
                                                ),
                                                revision: FileRevision(
                                                    1689436316,
                                                ),
                                            },
                                            file_scope: FileScopeId(
                                                0,
                                            ),
                                            place: Symbol(
                                                ScopedSymbolId(
                                                    108,
                                                ),
                                            ),
                                            kind: Class(
                                                AstNodeRef {
                                                    kind: StmtClassDef,
                                                    range: 27489..27607,
                                                },
                                            ),
                                            is_reexported: true,
                                        },
                                    ),
                                }: GenericContextTypeVarOptions {
                                    should_promote_literals: false,
                                },
                            },
                        },
                        types: [
                            NominalInstance(
                                NominalInstanceType(
                                    NonTuple(
                                        NonGeneric(
                                            ClassLiteral {
                                                name: Name("Model"),
                                                body_scope: ScopeId {
                                                    [salsa id]: Id(c983),
                                                    file: File {
                                                        path: System(
                                                            "/Users/alexw/dev/bokeh/src/bokeh/model/model.pyi",
                                                        ),
                                                        status: Exists,
                                                        permissions: Some(
                                                            33188,
                                                        ),
                                                        revision: FileRevision(
                                                            32450546247609911550292943140,
                                                        ),
                                                    },
                                                    file_scope_id: FileScopeId(
                                                        1,
                                                    ),
                                                },
                                                known: None,
                                                deprecated: None,
                                                dataclass_params: Some(
                                                    DataclassParams(
                                                        REPR | EQ | MATCH_ARGS,
                                                    ),
                                                ),
                                                dataclass_transformer_params: None,
                                            },
                                        ),
                                    ),
                                ),
                            ),
                        ],
                        materialization_kind: None,
                        tuple_inner: None,
                    },
                },
            ),
        ),
        _phantom: PhantomData<()>,
    },
)`
```

</details>

Here's the diff between the two types: https://www.diffchecker.com/ZxRy9rMI/. It appears that one of the `Model` types comes from `bokeh/src/bokeh/model/model.pyi`, and the other `Model` type comes from `bokeh/src/bokeh/model/model.py` (notice the difference in file extension).

I suppose that this is only an issue because the diagnostic is being emitted while checking `model/model.py` itself.

This is quite strange but seems unrelated to this PR, and it was a pre-existing problem. I've opened https://github.com/astral-sh/ty/issues/1306.

## homeassistant

There's lots of new diagnostics like this:

```
[error] invalid-argument-type - :58:9 - Argument to bound method `__init__` is incorrect: Expected `(() -> Awaitable[dict[str, Any]]) | None`, found `def async_update() -> CoroutineType[Any, Any, IssData]`
```

This relates to https://github.com/astral-sh/ty/issues/500 (so I suppose it will be solved by https://github.com/astral-sh/ty/issues/623). A [repro of this false positive](https://play.ty.dev/9b4dee70-b60e-4fc0-8eb4-bd3439de9120) that occurs on `main` and doesn't involve protocols is:

```py
from typing import Any, Callable

class Foo[T = dict[str, Any]]:
    def __init__(self, data: Callable[..., T]): ...

def returns_bool() -> bool:
    return True

# error: [invalid-argument-type] "Argument to bound method `__init__` is incorrect: Expected `(...) -> dict[str, Any]`, found `def returns_bool() -> bool`"
# revealed: `Foo[dict[str, Any]]`
reveal_type(Foo(returns_bool))
```

## ibis

```
ibis/common/egraph.py
[error] unresolved-attribute - :732:44 - Type `list[Rewrite]` has no attribute `matcher`
[error] unresolved-attribute - :733:25 - Type `list[Rewrite]` has no attribute `applier`
```

These are because on `main` we infer this:

```py
from typing import Iterable

def f[T](x: T | Iterable[T]) -> T:
    raise NotImplementedError

reveal_type(f(["foo"]))  # revealed: Unknown
```

whereas with this PR, we infer this:

```py
from typing import Iterable

def f[T](x: T | Iterable[T]) -> T:
    raise NotImplementedError

reveal_type(f(["foo"]))  # revealed: list[Unknown | str]
```

when what is desired is of course:

```py
from typing import Iterable

def f[T](x: T | Iterable[T]) -> T:
    raise NotImplementedError

reveal_type(f(["foo"]))  # revealed: `str`
```

So this should also hopefully be solved by https://github.com/astral-sh/ty/issues/623.

## isort

Here's a minimal repro of what's going on here:

```py
from typing import Iterable, Self

class chain[T]:
    def __new__(cls, *iterables: Iterable[T]) -> Self:
        raise NotImplementedError

def yield_ints() -> Iterable[int]:
    yield 42

# revealed: chain[None]
reveal_type(chain((None,), (None,)))

# revealed: chain[Unknown]
reveal_type(chain(yield_ints(), yield_ints()))

# revealed: chain[Unknown]
# error: [invalid-argument-type] "Argument to function `__new__` is incorrect: Expected `Iterable[None]`, found `Iterable[int]`"
reveal_type(chain(yield_ints(), (None,)))
```

On `main`, the final line reveals `chain[None]` and the error on the final line is not emitted. It's possible that this might be related to https://github.com/astral-sh/ty/issues/1308 ? Not sure.

### meson

```
unittests/allplatformstests.py
[error] index-out-of-bounds - :3861:26 - Index 2 is out of bounds for tuple `tuple[str, (json_node) -> Unknown]` with length 2
[error] index-out-of-bounds - :3861:26 - Index 2 is out of bounds for tuple `tuple[str, (kwargs) -> Unknown]` with length 2
[error] index-out-of-bounds - :3861:26 - Index 2 is out of bounds for tuple `tuple[str, (node_list) -> Unknown]` with length 2
```

These are all https://github.com/astral-sh/ty/issues/1248: it's an unannotated heterogeneous dictionary.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-08 12:59_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-08 12:59_

---

_Marked ready for review by @AlexWaygood on 2025-10-08 13:42_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-08 13:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-08 13:42_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-08 13:42_

---

_Label `great writeup` added by @MichaReiser on 2025-10-08 13:45_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-08 17:31_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-08 17:31_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:550 on 2025-10-08 17:38_

Disjointness is also a relation. If we added TypeRelation::Disjoint and used `has_relation_to_impl` for implementing `is_disjoint_from`, we could reduce this back to a single `HasRelationToVisitor` and eliminate `IsDisjointVisitor` entirely.

In an ideal world I might say we should prefer to do that refactor as a prior PR, and then make this a much smaller PR on top of it. But I won't argue if you prefer to land this PR as-is and consider that as a possible follow-up. Maybe it's not even worth it; there's just a _big_ increase in LoC in this PR from all the `has_relation_to_impl` calls that now have to explode onto seven lines.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:172 on 2025-10-08 17:41_

This seems notable -- can we add a comment explaining the rationale for this?

---

_@carljm approved on 2025-10-08 17:52_

Looks good!

---

_@AlexWaygood reviewed on 2025-10-08 18:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:550 on 2025-10-08 18:18_

> Disjointness is also a relation.

That's true, but in terms of the implementation it's very different from the others (at least currently). I think what you'd be looking at would be greatly expanding the size of the `Type::has_relation_to_impl` function, which is already very complicated, hard to refactor, and hard to understand.

> there's just a _big_ increase in LoC in this PR from all the `has_relation_to_impl` calls that now have to explode onto seven lines.

That's true, but I feel less bad about it given what Doug has in store for us in https://github.com/astral-sh/ruff/pull/20677 ;)

---

_Merged by @AlexWaygood on 2025-10-08 18:37_

---

_Closed by @AlexWaygood on 2025-10-08 18:37_

---

_Branch deleted on 2025-10-08 18:37_

---
