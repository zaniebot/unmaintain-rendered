```yaml
number: 22104
title: "[ty] narrow tagged unions of `TypedDict`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeddict_union_narrowing
created_at: 2025-12-20T03:07:11Z
updated_at: 2025-12-23T19:38:56Z
url: https://github.com/astral-sh/ruff/pull/22104
synced_at: 2026-01-12T15:57:41Z
```

# [ty] narrow tagged unions of `TypedDict`

---

_@oconnor663_

Identify and narrow cases like this:

```py
class Foo(TypedDict):
    tag: Literal["foo"]

class Bar(TypedDict):
    tag: Literal["bar"]

def _(union: Foo | Bar):
    if union["tag"] == "foo":
        reveal_type(union)  # Foo
```

Fixes part of https://github.com/astral-sh/ty/issues/1479.

---

_Review requested from @carljm by @oconnor663 on 2025-12-20 03:07_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-20 03:07_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-20 03:07_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-20 03:07_

---

_Label `ty` added by @oconnor663 on 2025-12-20 03:07_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-12-20 03:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:177:50: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:179:30: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:183:50: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:185:33: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `TypedDictSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DecimalSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `StringSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `BytesSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DateSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `TimeSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DatetimeSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `TimedeltaSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `LiteralSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `MissingSentinelSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `EnumSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `IsInstanceSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `IsSubclassSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `CallableSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ListSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `TupleSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `SetSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `FrozenSetSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `GeneratorSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DictSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `AfterValidatorFunctionSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `BeforeValidatorFunctionSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `WrapValidatorFunctionSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `FloatSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `NullableSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ChainSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `LaxOrStrictSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `JsonOrPythonSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `InvalidSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ModelFieldsSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ModelSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DataclassArgsSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DataclassSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ArgumentsSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ArgumentsV3Schema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `CallSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `CustomErrorSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `JsonSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `UrlSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `MultiHostUrlSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DefinitionsSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `DefinitionReferenceSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `UuidSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `ComplexSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `IntSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `BoolSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `AnySchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `NoneSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:196:80: error[invalid-key] Unknown key "choices" for TypedDict `WithDefaultSchema`: Unknown key "choices"
- pydantic/_internal/_discriminated_union.py:197:40: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements]`, found `list[@Todo | InvalidSchema | AnySchema | ... omitted 51 union elements]`
- pydantic/_internal/_discriminated_union.py:223:13: error[invalid-argument-type] Argument to function `tagged_union_schema` is incorrect: Expected `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 4 union elements`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 6 union elements`
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DataclassSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `FloatSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DecimalSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `StringSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `BytesSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DateSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `TimeSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DatetimeSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `TimedeltaSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `LiteralSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `MissingSentinelSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `EnumSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `IsInstanceSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `IsSubclassSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `CallableSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ListSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `TupleSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `SetSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `FrozenSetSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `GeneratorSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `AfterValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `BeforeValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `WrapValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `IntSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `WithDefaultSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `NullableSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `UnionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `TaggedUnionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ChainSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `LaxOrStrictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `JsonOrPythonSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `TypedDictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ModelFieldsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ModelSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DataclassArgsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `InvalidSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ArgumentsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ArgumentsV3Schema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `CallSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `CustomErrorSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `JsonSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `UrlSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `MultiHostUrlSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `DefinitionsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `UuidSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `ComplexSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `AnySchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `NoneSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `BoolSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:238:23: error[invalid-key] Unknown key "schema_ref" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `IsSubclassSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `FloatSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DecimalSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `StringSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `BytesSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DateSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `TimeSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DatetimeSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `TimedeltaSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `LiteralSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `MissingSentinelSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `EnumSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `IsInstanceSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `InvalidSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `CallableSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ListSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `TupleSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `SetSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `FrozenSetSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `GeneratorSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `AfterValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `BeforeValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `WrapValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `IntSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `WithDefaultSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `NullableSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `UnionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `TaggedUnionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ChainSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `LaxOrStrictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `JsonOrPythonSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `TypedDictSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ModelFieldsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ModelSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DataclassArgsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DataclassSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ArgumentsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ArgumentsV3Schema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `CallSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `CustomErrorSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `JsonSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `UrlSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `MultiHostUrlSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `DefinitionsSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `UuidSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `ComplexSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `BoolSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `AnySchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `NoneSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:239:59: error[invalid-key] Unknown key "schema_ref" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema_ref"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:244:40: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`

... (truncated 3110 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @carljm on 2025-12-20 03:12_

omg look at those pydantic diagnostics disappear

---

_Comment by @charliermarsh on 2025-12-20 03:13_

Wow!

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:13_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 0 | 3,124 | 0 |
| `invalid-assignment` | 0 | 74 | 5 |
| `invalid-argument-type` | 2 | 19 | 19 |
| `invalid-return-type` | 2 | 0 | 13 |
| `possibly-missing-attribute` | 0 | 2 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **4** | **3,220** | **37** |

**[Full report with detailed diff](https://typeddict-union-narrowing.ecosystem-663.pages.dev/diff)** ([timing results](https://typeddict-union-narrowing.ecosystem-663.pages.dev/timing))


**[Full report with detailed diff](https://8163c9ad.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://8163c9ad.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 03:25_


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

_Comment by @codspeed-hq[bot] on 2025-12-20 03:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeddict_union_narrowing?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22104 will **not alter performance**

<sub>Comparing <code>typeddict_union_narrowing</code> (05184f3) with <code>main</code> (4c175fa)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/typeddict_union_narrowing?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@oconnor663 reviewed on 2025-12-20 03:29_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:918 on 2025-12-20 03:29_

I ended up not using synthesized `TypedDict`s at all in this first iteration. Interested in getting feedback on that point. If we wanted to use them, it would be here.

---

_@AlexWaygood reviewed on 2025-12-20 13:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/Cargo.toml`:36 on 2025-12-20 13:08_

nit: I think you could just use `itertools::Either`, which would mean we wouldn't need the additional dependency

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:887 on 2025-12-20 14:05_

```suggestion
        if matches!(&**ops, [ast::CmpOp::Eq])
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:897 on 2025-12-20 14:06_

```suggestion
            && let Type::StringLiteral(key_literal) = inference
                .expression_type(&*subscript.slice)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:911 on 2025-12-20 14:13_

another important case to handle here is enum-literal types.

However, there's some additional complications with enum literals, so I think it might be good to postpone enums to a followup. Namely, an enum literal can't be used as a "tag" if the enum class has a custom `__eq__` method. That's true if the enum class inherits from `IntEnum` or `StrEnum` (we handle this incorrectly in [some other situations](https://github.com/astral-sh/ty/issues/1454) right now).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:918 on 2025-12-20 14:20_

Using a synthesized supertype might be more efficient for something like this, where multiple `TypedDict`s in the union have the same tag:

```py
from typing import TypedDict, Literal

class A(TypedDict):
    tag: Literal["tag1"]
    x: int

class B(TypedDict):
    tag: Literal["tag1"]
    x: str

class C(TypedDict):
    tag: Literal["tag1"]
    x: bytes

class D(TypedDict):
    tag: Literal["tag2"]

def f(td: A | B | C | D):
    if td["tag"] == "tag2":
        reveal_type(td)
    else:
        reveal_type(td)
```

You'd only have to create an intersection with one negative element rather than one with three negative elements. But I think it would be fine to land your current implementation and then experiment with whether this is in fact more efficient as a followup. The point about readability seems reasonable!

---

_@AlexWaygood reviewed on 2025-12-20 14:21_

Nice! It would be great to rebase on `main` and see what the codspeed report is now https://github.com/astral-sh/ruff/pull/22102 has landed

---

_Comment by @oconnor663 on 2025-12-22 17:04_

Unfortunately merging `main` takes this from a 0.87x slowdown to a 0.60x slowdown. Looking into it...

---

_Comment by @oconnor663 on 2025-12-23 04:23_

04ee4dcbd8e34c5e5c35e601897242934726e5cf is a substantial refactoring. After staring at some profiles and adding logs and stuff, I'm pretty sure the problem is that I'm taking these unions of ~50 TypedDicts and intersecting them with ~49 negated TypedDicts in a lot of places, and that's slow. The new approach is to synthesize a TypedDict with a single field in both cases (the `==` case and the `!=` case). To avoid incorrectly intersecting out non-`TypedDict` types in unions, I always intersect with a _negated_ `SynthesizedTypedDict`, and the `==` case ends up as a double negative. Will look at what CodSpeed says later tonight or in the morning.

---

_@oconnor663 reviewed on 2025-12-23 04:41_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2109 on 2025-12-23 04:41_

The display here changed from `~Bar` to `~<TypedDict with items 'tag'>`. Probably that's fine?

---

_Closed by @AlexWaygood on 2025-12-23 11:37_

---

_Reopened by @AlexWaygood on 2025-12-23 11:37_

---

_@AlexWaygood reviewed on 2025-12-23 12:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2109 on 2025-12-23 12:34_

I think that's fine. I think the real fix to get rid of this is to implement disjointness between `dict` and `TypedDict` types... which theoretically isn't too hard, but I'm a _bit_ worried it might lead to false positives due to missing pieces in our TypedDict/bidirectional-inference logic elsewhere. Best done as a standalone change, anyway.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2124 on 2025-12-23 12:46_

I guess that means that it _would_ be sound to narrow using `not in` in some situations...? (But obviously not something for this PR, it can wait for a followup)

```py
from typing import TypedDict

class Foo(TypedDict):
    x: int

class Bar(TypedDict):
    y: int

def f(td: Foo | Bar):
    if "x" not in td:
        reveal_type(td)  # we can safely narrow to `Bar` here,
                         # -- the type cannot be `Foo`, since `x` is a required
                         # key in `Foo`
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:913 on 2025-12-23 15:50_

```suggestion
            let constrain_with_equality = is_positive == (ops[0] == ast::CmpOp);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:946 on 2025-12-23 15:50_

```suggestion
                // To avoid excluding non-`TypedDict` types, our constraints are always
                // expressed as a negative intersection (i.e. "you're *not* this kind of
                // `TypedDict`"). If `constrain_with_equality` is true, the whole constraint
                // is going to be a double negative, i.e. "you're *not* a `TypedDict`
                // *without* this literal
                // field". As the first step of building that, we negate the right hand side.
                let field_type = rhs_type.negate_if(self.db, constrain_with_equality);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:960 on 2025-12-23 16:17_

```suggestion
                let intersection = Type::TypedDict(synthesized_typeddict).negate(self.db);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1299 on 2025-12-23 16:18_

```suggestion
                    Type::Intersection(intersection) => {
                        intersection.positive(db).iter().any(Type::is_typed_dict)
                    }
```

---

_@AlexWaygood reviewed on 2025-12-23 16:19_

Mostly some small simplifications:

---

_@AlexWaygood reviewed on 2025-12-23 17:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:907 on 2025-12-23 17:32_

is there any reason not to include `BytesLiteral` types here (and in `all_matching_typeddict_fields_have_literal_types` lower down)? I guess it's unlikely that someone would actually use a `BytesLiteral` tag, but if someone were to, they should behave just the same way as string-literal and int-literal tags, so I don't see any disadvantage to including them here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1341 on 2025-12-23 17:38_

```suggestion
        // no matching field to check if the `.get()` call returns `None`
        typeddict.items(db).get(field_name).is_none_or(|field| {
            matches!(
                field.declared_ty,
                Type::StringLiteral(_) | Type::IntLiteral(_),
            )
        })
```

---

_@AlexWaygood reviewed on 2025-12-23 17:38_

---

_@AlexWaygood reviewed on 2025-12-23 17:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:954 on 2025-12-23 17:40_

```suggestion
                let schema = TypedDictSchema::from_iter([(field_name, field)]);
```

---

_@AlexWaygood approved on 2025-12-23 17:42_

Awesome work. Thank you!!

---

_@oconnor663 reviewed on 2025-12-23 18:18_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:946 on 2025-12-23 18:18_

Oh that's so nice.

---

_@oconnor663 reviewed on 2025-12-23 18:19_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:913 on 2025-12-23 18:19_

0dcc064056

---

_@oconnor663 reviewed on 2025-12-23 18:21_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:1299 on 2025-12-23 18:21_

Glorious. So many helper functions I haven't spotted.

---

_@oconnor663 reviewed on 2025-12-23 18:31_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:907 on 2025-12-23 18:31_

404dfd152e

---

_@oconnor663 reviewed on 2025-12-23 18:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/narrow.rs`:1341 on 2025-12-23 18:33_

bdf958a547

---

_@oconnor663 reviewed on 2025-12-23 18:40_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2124 on 2025-12-23 18:40_

Added a TODO: e326492b80091866dba90654e588f1376521e479

---

_Merged by @oconnor663 on 2025-12-23 19:30_

---

_Closed by @oconnor663 on 2025-12-23 19:30_

---

_Branch deleted on 2025-12-23 19:30_

---

_Comment by @carljm on 2025-12-23 19:37_

Awesome!

---
