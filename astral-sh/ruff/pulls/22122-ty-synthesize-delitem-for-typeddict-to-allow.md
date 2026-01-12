```yaml
number: 22122
title: "[ty] Synthesize `__delitem__` for TypedDict to allow deleting non-required keys"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/del-typed
created_at: 2025-12-21T01:07:33Z
updated_at: 2025-12-24T03:39:56Z
url: https://github.com/astral-sh/ruff/pull/22122
synced_at: 2026-01-12T15:57:42Z
```

# [ty] Synthesize `__delitem__` for TypedDict to allow deleting non-required keys

---

_@charliermarsh_

## Summary

TypedDict now synthesizes a proper `__delitem__` method that...

- ...allows deletion of `NotRequired` keys and keys in `total=False` TypedDicts.
- ...rejects deletion of required keys (synthesizes `__delitem__(k: Never)`).


---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-24 03:36:33.579011815 +0000
+++ new-output.txt	2025-12-24 03:36:35.732022492 +0000
@@ -968,8 +968,8 @@
 typeddicts_extra_items.py:29:23: error[invalid-key] Unknown key "novel_adaptation" for TypedDict `Movie`: Unknown key "novel_adaptation"
 typeddicts_extra_items.py:39:54: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `InheritedMovie`: value of type `None`
 typeddicts_extra_items.py:43:5: error[invalid-key] Unknown key "other_extra_key" for TypedDict `InheritedMovie`: Unknown key "other_extra_key"
-typeddicts_extra_items.py:128:9: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieEI.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieEI`
-typeddicts_extra_items.py:129:9: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieEI.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["year"]` on object of type `MovieEI`
+typeddicts_extra_items.py:128:9: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieEI`
+typeddicts_extra_items.py:129:9: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["year"]` on object of type `MovieEI`
 typeddicts_extra_items.py:254:63: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
 typeddicts_extra_items.py:255:63: error[invalid-key] Unknown key "description" for TypedDict `MovieExtraStr`: Unknown key "description"
 typeddicts_extra_items.py:266:64: error[invalid-key] Unknown key "year" for TypedDict `MovieExtraInt`: Unknown key "year"
@@ -991,7 +991,7 @@
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
 typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
 typeddicts_extra_items.py:342:27: error[invalid-key] TypedDict `IntDictWithNum` can only be subscripted with a string literal key, got key of type `str`.
-typeddicts_extra_items.py:343:9: error[invalid-argument-type] Method `__delitem__` of type `bound method IntDictWithNum.__delitem__(k: Never) -> None` cannot be called with key of type `str` on object of type `IntDictWithNum`
+typeddicts_extra_items.py:343:9: error[invalid-argument-type] Method `__delitem__` of type `(key: Literal["num"], /) -> None` cannot be called with key of type `str` on object of type `IntDictWithNum`
 typeddicts_extra_items.py:352:25: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`
@@ -1002,9 +1002,8 @@
 typeddicts_operations.py:32:36: error[invalid-key] Unknown key "other" for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
-typeddicts_operations.py:49:5: error[invalid-argument-type] Method `__delitem__` of type `bound method Movie.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `Movie`
+typeddicts_operations.py:49:5: error[invalid-argument-type] Method `__delitem__` of type `(key: Never, /) -> None` cannot be called with key of type `Literal["name"]` on object of type `Movie`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
-typeddicts_operations.py:64:5: error[invalid-argument-type] Method `__delitem__` of type `bound method MovieOptional.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["name"]` on object of type `MovieOptional`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
@@ -1037,4 +1036,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1039 diagnostics
+Found 1038 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-21 01:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kopf (https://github.com/nolar/kopf)
- kopf/_cogs/configs/conventions.py:281:17: error[invalid-argument-type] Method `__delitem__` of type `bound method MetaEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["annotations"]` on object of type `MetaEssence`
- kopf/_cogs/configs/conventions.py:283:17: error[invalid-argument-type] Method `__delitem__` of type `bound method MetaEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["labels"]` on object of type `MetaEssence`
- kopf/_cogs/configs/conventions.py:285:17: error[invalid-argument-type] Method `__delitem__` of type `bound method BodyEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["metadata"]` on object of type `BodyEssence`
- kopf/_cogs/configs/conventions.py:287:17: error[invalid-argument-type] Method `__delitem__` of type `bound method BodyEssence.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["status"]` on object of type `BodyEssence`
- Found 267 diagnostics
+ Found 263 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/rust.py:183:13: error[invalid-argument-type] Method `__delitem__` of type `bound method ExecutableKeywordArguments.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["rust_crate_type"]` on object of type `ExecutableKeywordArguments`
- Found 1948 diagnostics
+ Found 1947 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

zulip (https://github.com/zulip/zulip)
- zerver/lib/message.py:244:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["prev_content"]` on object of type `FormattedEditHistoryEvent`
- zerver/lib/message.py:245:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["prev_rendered_content"]` on object of type `FormattedEditHistoryEvent`
- zerver/lib/message.py:246:21: error[invalid-argument-type] Method `__delitem__` of type `bound method FormattedEditHistoryEvent.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["content_html_diff"]` on object of type `FormattedEditHistoryEvent`
- zerver/lib/users.py:637:13: error[invalid-argument-type] Method `__delitem__` of type `bound method APIUserDict.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["timezone"]` on object of type `APIUserDict`
- Found 3663 diagnostics
+ Found 3659 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2095 diagnostics
+ Found 2097 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/deconz/light.py:319:17: error[invalid-argument-type] Method `__delitem__` of type `bound method SetStateAttributes.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["on"]` on object of type `SetStateAttributes`
- homeassistant/components/deconz/light.py:339:17: error[invalid-argument-type] Method `__delitem__` of type `bound method SetStateAttributes.__delitem__(k: Never) -> None` cannot be called with key of type `Literal["on"]` on object of type `SetStateAttributes`
- Found 14414 diagnostics
+ Found 14412 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-21 01:16_

I believe the conformance tests are right except the `Ok` here:

```python
class MovieEI(TypedDict, extra_items=int):
    name: str

def del_items(movie: MovieEI) -> None:
    del movie["name"]  # E: The value type of 'name' is 'Required[str]'
    del movie["year"]  # OK: The value type of 'year' is 'NotRequired[int]'
```

But I believe we don't support `extra_items` yet.

---

_Comment by @charliermarsh on 2025-12-21 01:17_

(Oh, `extra_items` is Python 3.15 anyway.)

---

_Marked ready for review by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-21 01:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-21 01:35_

---

_Label `ty` added by @charliermarsh on 2025-12-21 01:35_

---

_Comment by @AlexWaygood on 2025-12-21 19:42_

> (Oh, `extra_items` is Python 3.15 anyway.)

you can use it on earlier Python versions if you use `typing_extensions.TypedDict`. But yeah, we haven't implemented it yet, so no need to worry about it in this PR :-)

---

_Comment by @AlexWaygood on 2025-12-21 19:44_

I would be tempted to land this as a standalone change before landing https://github.com/astral-sh/ruff/pull/22121, to make the ecosystem report on that PR easier to analyze. (IIUC, that PR adds a bunch of `TypedDict`-related false positives that this PR then fixes?)

I think the tests you've currently added possibly won't be much use if you land this as a standalone change before https://github.com/astral-sh/ruff/pull/22121, but you can write tests like this instead:

```py
from typing import TypedDict

class Foo(TypedDict):
    x: int

reveal_type(Foo.__delitem__)
```

---

_Comment by @charliermarsh on 2025-12-21 19:50_

For me personally, the current stack feels like the right order -- that PR adds support for `__delitem__`, and then this PR improves that support. (And it's not really that many false positives -- it should be exactly those diagnostics removed here?)


---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/del.md`:265 on 2025-12-24 01:56_

I think it's cool that we do this by synthesizing overloads, but what does this error end up looking like? Can we snapshot-diagnostics here?

---

_@carljm approved on 2025-12-24 01:57_

---

_@charliermarsh reviewed on 2025-12-24 03:35_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/del.md`:265 on 2025-12-24 03:35_

I added a dedicated diagnostic for it (with snapshots) in https://github.com/astral-sh/ruff/pull/22123.

---

_Merged by @charliermarsh on 2025-12-24 03:39_

---

_Closed by @charliermarsh on 2025-12-24 03:39_

---

_Branch deleted on 2025-12-24 03:39_

---
