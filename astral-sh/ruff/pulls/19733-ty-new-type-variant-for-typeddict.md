```yaml
number: 19733
title: "[ty] New `Type` variant for `TypedDict`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/typeddict
created_at: 2025-08-04T08:47:25Z
updated_at: 2025-08-05T12:00:47Z
url: https://github.com/astral-sh/ruff/pull/19733
synced_at: 2026-01-10T17:52:17Z
```

# [ty] New `Type` variant for `TypedDict`

---

_Pull request opened by @sharkdp on 2025-08-04 08:47_

## Summary

This PR adds a new `Type::TypedDict` variant. Before this PR, we treated `TypedDict`-based types as dynamic Todo-types, and I originally planned to make this change a no-op. And we do in fact still treat that new variant similar to a dynamic type when it comes to type properties such as assignability and subtyping. But then I somehow tricked myself into implementing some of the things correctly, so here we are. The two main behavioral changes are: (1) we now also detect generic `TypedDict`s, which removes a few false positives in the ecosystem, and (2) we now support *attribute* access (not key-based indexing!) on these types, i.e. we infer proper types for something like `MyTypedDict.__required_keys__`. Nothing exciting yet, but gets the infrastructure into place.

Note that with this PR, the type of (the type) `MyTypedDict` itself is still represented as a `Type::ClassLiteral` or `Type::GenericAlias` (in case `MyTypedDict` is generic). Only inhabitants of `MyTypedDict` (instances of `dict` at runtime) are represented by `Type::TypedDict`. We may want to revisit this decision in the future, if this turns out to be too error-prone. Right now, we need to use `.is_typed_dict(db)` in all the right places to distinguish between actual (generic) classes and `TypedDict`s. But so far, it seemed unnecessary to add additional `Type` variants for these as well.

~~Note: this PR crucially depends on https://github.com/salsa-rs/salsa/pull/954 by @MichaReiser. I'm not sure if we want to merge this before it has landed in salsa.~~ (this is now merged, updated the commit ref to the latest commit on salsa main)

part of https://github.com/astral-sh/ty/issues/154

## Ecosystem impact

The new diagnostics on `cloud-init` look like true positives to me.

## Test Plan

Updated and new Markdown tests

---

_Label `ty` added by @sharkdp on 2025-08-04 08:47_

---

_Comment by @github-actions[bot] on 2025-08-04 08:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree//conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-05 08:54:39.534836544 +0000
+++ new-output.txt	2025-08-05 08:54:39.597836730 +0000
@@ -137,6 +137,7 @@
 callables_annotation.py:57:18: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+callables_annotation.py:114:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
 callables_kwargs.py:24:5: error[type-assertion-failure] Argument does not have asserted type `int`
 callables_kwargs.py:32:9: error[type-assertion-failure] Argument does not have asserted type `str`
 callables_kwargs.py:35:5: error[type-assertion-failure] Argument does not have asserted type `str`
@@ -147,9 +148,12 @@
 callables_kwargs.py:65:5: error[missing-argument] No argument provided for required parameter `v3` of function `func2`
 callables_protocol.py:97:1: error[invalid-assignment] Object of type `def cb4_bad1(x: int) -> None` is not assignable to `Proto4`
 callables_protocol.py:121:1: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
+callables_protocol.py:176:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
 callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 callables_subtyping.py:26:5: error[invalid-assignment] Object of type `(int, /) -> int` is not assignable to `(int | float, /) -> int | float`
 callables_subtyping.py:29:5: error[invalid-assignment] Object of type `(int | float, /) -> int | float` is not assignable to `(int, /) -> int`
+callables_subtyping.py:204:21: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
+classes_classvar.py:35:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
 classes_classvar.py:38:11: error[invalid-type-form] Type qualifier `typing.ClassVar` expected exactly 1 argument, got 2
 classes_classvar.py:39:14: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 classes_classvar.py:40:14: error[unresolved-reference] Name `var` used when not defined
@@ -382,6 +386,7 @@
 generics_defaults.py:55:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults.py:59:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults.py:63:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
+generics_defaults.py:76:23: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
 generics_defaults.py:79:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults.py:80:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
@@ -412,6 +417,7 @@
 generics_paramspec_semantics.py:38:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:53:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
+generics_paramspec_semantics.py:67:9: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
 generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 generics_paramspec_semantics.py:82:5: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
@@ -424,9 +430,12 @@
 generics_paramspec_semantics.py:133:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:138:29: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:143:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
+generics_paramspec_specialization.py:13:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
+generics_paramspec_specialization.py:18:29: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
 generics_paramspec_specialization.py:32:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, bool]`?
 generics_paramspec_specialization.py:40:27: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_paramspec_specialization.py:40:31: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
+generics_paramspec_specialization.py:48:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Generic`
 generics_paramspec_specialization.py:52:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str, bool]`?
 generics_scoping.py:14:1: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
@@ -532,6 +541,7 @@
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo, ...]`, found `Literal[1]`
+generics_typevartuple_basic.py:52:14: error[invalid-argument-type] `TypeVarTuple` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
@@ -784,6 +794,7 @@
 protocols_subtyping.py:16:6: error[call-non-callable] Cannot instantiate class `Proto1`: This call will raise `TypeError` at runtime
 protocols_subtyping.py:38:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Concrete2`
 protocols_subtyping.py:55:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Proto3`
+protocols_variance.py:84:16: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
 protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 qualifiers_annotated.py:43:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 qualifiers_annotated.py:44:17: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -885,4 +896,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 886 diagnostics
+Found 897 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-04 08:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/json_schema.py:1734:9: error[invalid-assignment] Object of type `Any | list[Any]` is not assignable to `ConfigDict`
- Found 767 diagnostics
+ Found 766 diagnostics

yarl (https://github.com/aio-libs/yarl)
- tests/test_url.py:2395:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 49 diagnostics
+ Found 48 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/netinfo.py:596:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ip"]` on object of type `str`
+ cloudinit/netinfo.py:597:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["mask"]` on object of type `str`
+ cloudinit/netinfo.py:598:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | @Todo(Support for `TypedDict`)` is possibly unbound
+ cloudinit/netinfo.py:609:25: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ip"]` on object of type `str`
+ cloudinit/netinfo.py:611:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | @Todo(Support for `TypedDict`)` is possibly unbound
+ cloudinit/sources/DataSourceVMware.py:1017:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ip"]` on object of type `str`
+ cloudinit/sources/DataSourceVMware.py:1027:20: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["ip"]` on object of type `str`
+ cloudinit/sources/DataSourceVMware.py:1155:62: error[invalid-argument-type] Argument to function `convert_to_netifaces_ipv4_format` is incorrect: Expected `dict[Unknown, Unknown]`, found `str`
+ cloudinit/sources/DataSourceVMware.py:1157:62: error[invalid-argument-type] Argument to function `convert_to_netifaces_ipv6_format` is incorrect: Expected `dict[Unknown, Unknown]`, found `str`
- Found 598 diagnostics
+ Found 607 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ src/urllib3/connection.py:992:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 395 diagnostics
+ Found 396 diagnostics

altair (https://github.com/vega/altair)
+ tests/vegalite/v6/test_api.py:410:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/vegalite/v6/test_api.py:430:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/vegalite/v6/test_api.py:465:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/vegalite/v6/test_theme.py:1073:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/vegalite/v6/test_theme.py:1074:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1303 diagnostics
+ Found 1304 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/core/lists/model.py:421:24: warning[possibly-unbound-attribute] Attribute `key` on type `(Thing & ~str & ~dict[Unknown, Unknown]) | (AnnotatedSeed & ~str & ~dict[Unknown, Unknown])` is possibly unbound
- Found 705 diagnostics
+ Found 706 diagnostics

zulip (https://github.com/zulip/zulip)
- corporate/views/support.py:582:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `Self@Model`
- corporate/views/support.py:894:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self@Model`
- corporate/views/support.py:894:65: warning[possibly-unresolved-reference] Name `remote_realm` used when possibly not defined
- corporate/views/support.py:902:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
- corporate/views/support.py:902:66: warning[possibly-unresolved-reference] Name `remote_server` used when possibly not defined
- zerver/lib/message_report.py:35:60: error[invalid-argument-type] Argument to function `silent_mention_syntax_for_user` is incorrect: Expected `UserProfile | UserDisplayRecipient`, found `Unknown | _ST@ForeignKey`
- zerver/lib/reminders.py:35:58: error[invalid-argument-type] Argument to function `silent_mention_syntax_for_user` is incorrect: Expected `UserProfile | UserDisplayRecipient`, found `Unknown | _ST@ForeignKey`
- Found 7424 diagnostics
+ Found 7417 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-04 08:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ftypeddict?runnerMode=Instrumentation)

### Merging #19733 will **not alter performance**

<sub>Comparing <code>david/typeddict</code> (bfa22e9) with <code>main</code> (351121c)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-08-04 08:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 10:50_

---

_Comment by @github-actions[bot] on 2025-08-04 10:59_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 7 | 5 | 0 |
| `unused-ignore-comment` | 4 | 3 | 0 |
| `possibly-unbound-attribute` | 3 | 0 | 0 |
| `possibly-unresolved-reference` | 0 | 2 | 0 |
| `invalid-assignment` | 0 | 1 | 0 |
| **Total** | **14** | **11** | **0** |

**[Full report with detailed diff](https://david-typeddict.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-04 12:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 12:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-04 13:50_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 13:50_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-04 14:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 14:08_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-04 14:35_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 14:35_

---

_Renamed from "[ty] New Type variant for TypedDict" to "[ty] New `Type` variant for `TypedDict`" by @sharkdp on 2025-08-04 18:15_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-04 18:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-04 18:51_

---

_@sharkdp reviewed on 2025-08-04 18:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:64 on 2025-08-04 18:55_

I added this because I noticed that `alice` here is just a `dict[Unknown, Unknown]`. `bob`, however, is a real `Person`.

This doesn't come as a surprise, but shows again that we'll either need bidirectional type inference or a resolution to https://github.com/astral-sh/ty/issues/136 to be able to support `TypedDict`s properly.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:311 on 2025-08-04 18:56_

Basically just moved from above. We don't support the functional syntax at all, so far.

---

_@sharkdp reviewed on 2025-08-04 18:56_

---

_@sharkdp reviewed on 2025-08-04 18:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:156 on 2025-08-04 18:56_

This was moved to the last section of the file (`Person` => `Message`)

---

_Marked ready for review by @sharkdp on 2025-08-04 19:04_

---

_Review requested from @carljm by @sharkdp on 2025-08-04 19:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-04 19:04_

---

_Review requested from @dcreager by @sharkdp on 2025-08-04 19:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:64 on 2025-08-04 20:55_

> `bob`, however, is a real `Person`.

I'm happy for bob!!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:186 on 2025-08-04 20:57_

is it worth also adding some tests that demonstrate that these attributes _cannot_ be accessed from inhabitants of the `TypedDict` type (unlike most `ClassVar`s)?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:221 on 2025-08-04 21:00_

`TypedDict` feels a bit different to the other ones in this branch -- the others are all class objects (or similar) whereas `Type::TypedDict` is more similar to `Type::NominalInstance` or `Type::ProtocolInstance` IIUC, in that it inhabits the type defined by a class rather than actually _being_ the class (or a subclass of the class)

I wonder if calling the variant `TypedDictInhabitant` might make this clearer?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1101 on 2025-08-04 21:02_

We'll need to do more here in due course, I think (but fine to leave this like this for now, of course!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1136 on 2025-08-04 21:07_

is it true that subtyping is always reflexive for a `TypedDict` type? What about gradual `TypedDict` types? I don't think this `TypedDict` is a subtype of itself, because it's a gradual type:

```py
from typing import Any, TypedDict

class Foo(TypedDict):
    x: Any
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1786 on 2025-08-04 21:09_

you could do something like this for now: if `Mapping[str, object]` is disjoint from the other type, any `TypedDict` type will also be disjoint from the other type, since all `TypedDict` types are subtypes of `Mapping[str, object]`

```suggestion
                KnownClass::Mapping.to_specialized_instance(db, [KnownClass::Str.to_instance(db), Type::object(db)]).is_disjoint_from(db, other)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3559 on 2025-08-04 21:13_

this isn't correct: if the `TypedDict` is empty, all its inhabitants will be falsy:

```py
from typing import TypedDict

class Empty(TypedDict): ...

x: Empty = {}
print(bool(x))  # False
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5248 on 2025-08-04 21:14_

style nit:

```suggestion
                    _ if class.is_typed_dict(db) => Type::TypedDict(TypedDictType::new(db, ClassType::NonGeneric(*class))),
                    _ => Type::instance(db, class.default_specialization(db)),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5261 on 2025-08-04 21:15_

```suggestion
            Type::GenericAlias(alias) if alias.is_typed_dict(db) => Ok(Type::TypedDict(TypedDictType::new(
                        db,
                        ClassType::from(*alias),
                    ))),
            Type::GenericAlias(alias) => Ok(Type::instance(db, ClassType::from(*alias))),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5594 on 2025-08-04 21:18_

I'm not sure this is correct, because at runtime `TypedDict` inhabitants are always instances of exactly `dict`. We use `Type::to_meta_type()` to infer the type of things like `type(x)` and `x.__class__` -- if `x` is an inhabitant of a `TypedDict` type, both of those will be `<class 'dict'>` at runtime. Which would imply that this should be

```suggestion
            Type::TypedDict(typed_dict) => KnownClass::Dict.to_class_literal(db),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:979 on 2025-08-04 21:24_

they're always instances of exactly `dict` at runtime, but it may not be worth the complexity trying to model that here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:172 on 2025-08-04 21:25_

(You'll need to change this too if you think my comments above about the meta-type of `TypedDict`s is reasonable)

---

_@AlexWaygood reviewed on 2025-08-04 21:26_

Thank you! `TypedDict` types are odd in a number of ways; there's a few things to think through here --

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_model.rs`:221 on 2025-08-05 03:39_

If we rename to anything, I think `TypedDictInstance` is better than `TypedDictInhabitant` -- but I don't think a rename is necessary.

(I do agree that this should be a `CompletionKind::Struct`, not a `CompletionKind::Class`)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:734 on 2025-08-05 03:43_

We should have a TODO here, because this will need to create a new `TypedDict` type with any gradual item types materialized.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1101 on 2025-08-05 03:43_

Agreed, but I would favor leaving an explicit TODO comment in each particular place where we know the handling will need to be improved.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1136 on 2025-08-05 03:44_

Yes, I think given this method is supposed to be a non-recursive quick check, the right thing here is not a TODO but to simply move this into the `false` branch, and leave it there permanently.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2621 on 2025-08-05 03:48_

Hmm, shouldn't this fallback to attributes of `dict`? (But I must be missing something, since it seems like similar should be true for many of the other types above in this arm.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:474 on 2025-08-05 04:00_

This should have a TODO comment?

---

_@carljm reviewed on 2025-08-05 04:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5594 on 2025-08-05 06:37_

I don't think so. This way, we would lose important information. Sure, inhabitants are instances of `dict` at runtime. But a `Type::TypedDict` also represents a (special sub)set of `dict` instances. And its meta type should be the specific class that describes that subset.

Consider what happens if we access `person["name"]` on `person: Person`. Under the hood, we call `type(person).__getitem__(person, "name")`. If we model `type(person)` as being of type `<class 'dict'>`, we would get a generic answer like `object`. But we want this to be the type of the `name` item on `Person`.

Other type checkers also infer `type[Person]` for `type(Person)` (except for mypy, which infers `Person` as a function).

---

_@sharkdp reviewed on 2025-08-05 06:37_

---

_@sharkdp reviewed on 2025-08-05 06:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2621 on 2025-08-05 06:48_

`find_name_in_mro` only works on class-like types. It is conceptually similar to the following function: 
```py
def find_name_in_mro(cls, name, default):
    "Emulate _PyType_Lookup() in Objects/typeobject.c"
    for base in cls.__mro__:
        if name in vars(base):
            return vars(base)[name]
    return default
```

Accessing members on instance-like types will always go through `class_member`, which calls `to_meta_type` first (`ty.class_member(…) = ty.to_meta_type(…).find_name_in_mro(…)`), similar to how an attribute access of `obj.attr` is always `type(obj).attr` under the hood (except for instance attributes, which are handled elsewhere).

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3559 on 2025-08-05 06:59_

Oh, thank you!

---

_@sharkdp reviewed on 2025-08-05 06:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1786 on 2025-08-05 07:08_

Will keep this comment in mind for the next iteration. Thanks.

---

_@sharkdp reviewed on 2025-08-05 07:08_

---

_@AlexWaygood reviewed on 2025-08-05 07:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5594 on 2025-08-05 07:09_

Hmm, okay, interesting. I think what this maybe implies in that case is that we'll need to treat `type[]` types quite differently for TypedDicts than for other classes: you can only access the synthesised TypedDict methods (and any methods from Mapping) on `type(x)` (where x is a TypedDict inhabitant) — you can't access `__required_keys__` and the other ClassVars on `_TypedDictFallback` from `type(x)`

---

_@sharkdp reviewed on 2025-08-05 07:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3559 on 2025-08-05 07:18_

`{}` is also an inhabitant of `TypedDict`s with `total=False` or a `TypedDict` where all items are `NotRequired`.

Conversely, through subtyping, inhabitants of `Empty` may not be falsy:

```py
class Empty(TypedDict):
    pass

class OneKey(Empty):
    a: int

def accepts_empty(e: Empty):
    reveal_type(bool(e))

accepts_empty(OneKey({"a": 1}))
```

I guess it's best to just return `Ambiguous`/`bool` for now.

---

_@sharkdp reviewed on 2025-08-05 07:35_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:186 on 2025-08-05 07:35_

Yes, thanks. I added a test, but it fails (added a TODO). No other type checker gets this right, so I'm not going to invest time right now.

---

_@sharkdp reviewed on 2025-08-05 08:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5594 on 2025-08-05 08:24_

That's a good observation, yes. I attempted to fix this and found a [pre-existing bug](https://github.com/astral-sh/ty/issues/937) related to annotated assignments without a right hand side (but with type qualifiers) in stub files. As this relates to the `__total__: ClassVar[bool]` annotations in `TypedDictFallback`, I need to fix this first. I'd like to do that in a separate PR as it might have a wider-spread impact.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/truthiness.md`:240 on 2025-08-05 08:34_

But note that if something like https://peps.python.org/pep-0728 was accepted and the TypedDict was marked as `closed=True`, we _would_ be able to infer `Literal[False]` here 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3555 on 2025-08-05 08:35_

Could add a TODO here too saying that we could infer more precise truthiness in some specific situations?

---

_@AlexWaygood reviewed on 2025-08-05 08:35_

---

_@AlexWaygood approved on 2025-08-05 08:37_

LGTM if we're happy with the salsa bump!

---

_Comment by @sharkdp on 2025-08-05 08:53_

> LGTM if we're happy with the salsa bump!

@MichaReiser's PR has been merged and I updated the commit to the latest from salsa main.

---

_Merged by @sharkdp on 2025-08-05 09:19_

---

_Closed by @sharkdp on 2025-08-05 09:19_

---

_Branch deleted on 2025-08-05 09:19_

---

_@sharkdp reviewed on 2025-08-05 10:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5594 on 2025-08-05 10:46_

See bugfix [here](https://github.com/astral-sh/ruff/pull/19756) and follow-up [here](https://github.com/astral-sh/ruff/pull/19758).

---

_@AlexWaygood reviewed on 2025-08-05 11:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:234 on 2025-08-05 11:06_

It looks like you maybe deleted the `### Unions` header above accidentally here

```suggestion
### Unions

For unions, `ide_support::all_members` only returns members that are available on all elements of
```

---

_@sharkdp reviewed on 2025-08-05 12:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`:234 on 2025-08-05 12:00_

Ah, yeah. I noticed that to independently and fixed it in https://github.com/astral-sh/ruff/pull/19758

---
