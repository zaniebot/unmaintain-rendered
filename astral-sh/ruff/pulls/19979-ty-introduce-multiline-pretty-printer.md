```yaml
number: 19979
title: "[ty] introduce multiline pretty printer"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/func-fmt
created_at: 2025-08-18T21:54:06Z
updated_at: 2025-08-19T17:32:08Z
url: https://github.com/astral-sh/ruff/pull/19979
synced_at: 2026-01-12T15:56:51Z
```

# [ty] introduce multiline pretty printer

---

_@Gankra_

Requires some iteration, but this includes the most tedious part -- threading a new concept of DisplaySettings through every type display impl. Currently it only holds a boolean for multiline, but in the future it could also take other things like "render to markdown" or "here's your base indent if you make a newline".

For types which have exposed display functions I've left the old signature as a compatibility polyfill to avoid having to audit everywhere that prints types right off the bat (notably I originally tried doing multiline functions unconditionally and a ton of things churned that clearly weren't ready for multi-line (diagnostics).

The only real use of this API in this PR is to multiline render function types in hovers, which is the highest impact (see snapshot changes).

Fixes https://github.com/astral-sh/ty/issues/1000

---

_Review requested from @carljm by @Gankra on 2025-08-18 21:54_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-18 21:54_

---

_Review requested from @sharkdp by @Gankra on 2025-08-18 21:54_

---

_Review requested from @dcreager by @Gankra on 2025-08-18 21:54_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-18 21:54_

---

_Label `ty` added by @Gankra on 2025-08-18 21:54_

---

_Label `server` added by @Gankra on 2025-08-18 21:54_

---

_Comment by @github-actions[bot] on 2025-08-18 21:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-18 21:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @Gankra on 2025-08-18 23:22_

I've also added multiline rendering for overloads using a similar scheme to pyright (just render each signature on its own line, don't mention overloads at all).

---

_Comment by @Gankra on 2025-08-18 23:26_

pydantic.Field now renders as:

<details>
<summary>click here for long signature...</summary>

```python
(
    default: EllipsisType,
    *,
    alias: str | None = PydanticUndefinedType,
    alias_priority: int | None = PydanticUndefinedType,
    validation_alias: str | AliasPath | AliasChoices | None = PydanticUndefinedType,
    serialization_alias: str | None = PydanticUndefinedType,
    title: str | None = PydanticUndefinedType,
    field_title_generator: ((str, FieldInfo, /) -> str) | None = PydanticUndefinedType,
    description: str | None = PydanticUndefinedType,
    examples: list[Any] | None = PydanticUndefinedType,
    exclude: bool | None = PydanticUndefinedType,
    discriminator: str | Unknown | None = PydanticUndefinedType,
    deprecated: @Todo(Support for `types.UnionType` instances in type expressions) | str | bool | None = PydanticUndefinedType,
    json_schema_extra: @Todo(Support for `typing.TypeAlias`) | ((@Todo(Support for `typing.TypeAlias`), /) -> None) | None = PydanticUndefinedType,
    frozen: bool | None = PydanticUndefinedType,
    validate_default: bool | None = PydanticUndefinedType,
    repr: bool = PydanticUndefinedType,
    init: bool | None = PydanticUndefinedType,
    init_var: bool | None = PydanticUndefinedType,
    kw_only: bool | None = PydanticUndefinedType,
    pattern: str | Pattern[str] | None = PydanticUndefinedType,
    strict: bool | None = PydanticUndefinedType,
    coerce_numbers_to_str: bool | None = PydanticUndefinedType,
    gt: SupportsGt | None = PydanticUndefinedType,
    ge: SupportsGe | None = PydanticUndefinedType,
    lt: SupportsLt | None = PydanticUndefinedType,
    le: SupportsLe | None = PydanticUndefinedType,
    multiple_of: int | float | None = PydanticUndefinedType,
    allow_inf_nan: bool | None = PydanticUndefinedType,
    max_digits: int | None = PydanticUndefinedType,
    decimal_places: int | None = PydanticUndefinedType,
    min_length: int | None = PydanticUndefinedType,
    max_length: int | None = PydanticUndefinedType,
    union_mode: Literal["smart", "left_to_right"] = PydanticUndefinedType,
    fail_fast: bool | None = PydanticUndefinedType,
    **extra: @Todo(`Unpack[]` special form)
) -> Any
(
    default: _T@Field,
    *,
    alias: str | None = PydanticUndefinedType,
    alias_priority: int | None = PydanticUndefinedType,
    validation_alias: str | AliasPath | AliasChoices | None = PydanticUndefinedType,
    serialization_alias: str | None = PydanticUndefinedType,
    title: str | None = PydanticUndefinedType,
    field_title_generator: ((str, FieldInfo, /) -> str) | None = PydanticUndefinedType,
    description: str | None = PydanticUndefinedType,
    examples: list[Any] | None = PydanticUndefinedType,
    exclude: bool | None = PydanticUndefinedType,
    discriminator: str | Unknown | None = PydanticUndefinedType,
    deprecated: @Todo(Support for `types.UnionType` instances in type expressions) | str | bool | None = PydanticUndefinedType,
    json_schema_extra: @Todo(Support for `typing.TypeAlias`) | ((@Todo(Support for `typing.TypeAlias`), /) -> None) | None = PydanticUndefinedType,
    frozen: bool | None = PydanticUndefinedType,
    validate_default: bool | None = PydanticUndefinedType,
    repr: bool = PydanticUndefinedType,
    init: bool | None = PydanticUndefinedType,
    init_var: bool | None = PydanticUndefinedType,
    kw_only: bool | None = PydanticUndefinedType,
    pattern: str | Pattern[str] | None = PydanticUndefinedType,
    strict: bool | None = PydanticUndefinedType,
    coerce_numbers_to_str: bool | None = PydanticUndefinedType,
    gt: SupportsGt | None = PydanticUndefinedType,
    ge: SupportsGe | None = PydanticUndefinedType,
    lt: SupportsLt | None = PydanticUndefinedType,
    le: SupportsLe | None = PydanticUndefinedType,
    multiple_of: int | float | None = PydanticUndefinedType,
    allow_inf_nan: bool | None = PydanticUndefinedType,
    max_digits: int | None = PydanticUndefinedType,
    decimal_places: int | None = PydanticUndefinedType,
    min_length: int | None = PydanticUndefinedType,
    max_length: int | None = PydanticUndefinedType,
    union_mode: Literal["smart", "left_to_right"] = PydanticUndefinedType,
    fail_fast: bool | None = PydanticUndefinedType,
    **extra: @Todo(`Unpack[]` special form)
) -> _T@Field
(
    *,
    default_factory: (() -> _T@Field) | ((dict[str, Any], /) -> _T@Field),
    alias: str | None = PydanticUndefinedType,
    alias_priority: int | None = PydanticUndefinedType,
    validation_alias: str | AliasPath | AliasChoices | None = PydanticUndefinedType,
    serialization_alias: str | None = PydanticUndefinedType,
    title: str | None = PydanticUndefinedType,
    field_title_generator: ((str, FieldInfo, /) -> str) | None = PydanticUndefinedType,
    description: str | None = PydanticUndefinedType,
    examples: list[Any] | None = PydanticUndefinedType,
    exclude: bool | None = PydanticUndefinedType,
    discriminator: str | Unknown | None = PydanticUndefinedType,
    deprecated: @Todo(Support for `types.UnionType` instances in type expressions) | str | bool | None = PydanticUndefinedType,
    json_schema_extra: @Todo(Support for `typing.TypeAlias`) | ((@Todo(Support for `typing.TypeAlias`), /) -> None) | None = PydanticUndefinedType,
    frozen: bool | None = PydanticUndefinedType,
    validate_default: bool | None = PydanticUndefinedType,
    repr: bool = PydanticUndefinedType,
    init: bool | None = PydanticUndefinedType,
    init_var: bool | None = PydanticUndefinedType,
    kw_only: bool | None = PydanticUndefinedType,
    pattern: str | Pattern[str] | None = PydanticUndefinedType,
    strict: bool | None = PydanticUndefinedType,
    coerce_numbers_to_str: bool | None = PydanticUndefinedType,
    gt: SupportsGt | None = PydanticUndefinedType,
    ge: SupportsGe | None = PydanticUndefinedType,
    lt: SupportsLt | None = PydanticUndefinedType,
    le: SupportsLe | None = PydanticUndefinedType,
    multiple_of: int | float | None = PydanticUndefinedType,
    allow_inf_nan: bool | None = PydanticUndefinedType,
    max_digits: int | None = PydanticUndefinedType,
    decimal_places: int | None = PydanticUndefinedType,
    min_length: int | None = PydanticUndefinedType,
    max_length: int | None = PydanticUndefinedType,
    union_mode: Literal["smart", "left_to_right"] = PydanticUndefinedType,
    fail_fast: bool | None = PydanticUndefinedType,
    **extra: @Todo(`Unpack[]` special form)
) -> _T@Field
(
    *,
    alias: str | None = PydanticUndefinedType,
    alias_priority: int | None = PydanticUndefinedType,
    validation_alias: str | AliasPath | AliasChoices | None = PydanticUndefinedType,
    serialization_alias: str | None = PydanticUndefinedType,
    title: str | None = PydanticUndefinedType,
    field_title_generator: ((str, FieldInfo, /) -> str) | None = PydanticUndefinedType,
    description: str | None = PydanticUndefinedType,
    examples: list[Any] | None = PydanticUndefinedType,
    exclude: bool | None = PydanticUndefinedType,
    discriminator: str | Unknown | None = PydanticUndefinedType,
    deprecated: @Todo(Support for `types.UnionType` instances in type expressions) | str | bool | None = PydanticUndefinedType,
    json_schema_extra: @Todo(Support for `typing.TypeAlias`) | ((@Todo(Support for `typing.TypeAlias`), /) -> None) | None = PydanticUndefinedType,
    frozen: bool | None = PydanticUndefinedType,
    validate_default: bool | None = PydanticUndefinedType,
    repr: bool = PydanticUndefinedType,
    init: bool | None = PydanticUndefinedType,
    init_var: bool | None = PydanticUndefinedType,
    kw_only: bool | None = PydanticUndefinedType,
    pattern: str | Pattern[str] | None = PydanticUndefinedType,
    strict: bool | None = PydanticUndefinedType,
    coerce_numbers_to_str: bool | None = PydanticUndefinedType,
    gt: SupportsGt | None = PydanticUndefinedType,
    ge: SupportsGe | None = PydanticUndefinedType,
    lt: SupportsLt | None = PydanticUndefinedType,
    le: SupportsLe | None = PydanticUndefinedType,
    multiple_of: int | float | None = PydanticUndefinedType,
    allow_inf_nan: bool | None = PydanticUndefinedType,
    max_digits: int | None = PydanticUndefinedType,
    decimal_places: int | None = PydanticUndefinedType,
    min_length: int | None = PydanticUndefinedType,
    max_length: int | None = PydanticUndefinedType,
    union_mode: Literal["smart", "left_to_right"] = PydanticUndefinedType,
    fail_fast: bool | None = PydanticUndefinedType,
    **extra: @Todo(`Unpack[]` special form)
) -> Any
```
```text
!!! abstract "Usage Documentation"
    [Fields](../concepts/fields.md)

Create a field for objects that can be configured.

Used to provide extra information about a field, either for the model schema or complex validation. Some arguments
apply only to number fields (`int`, `float`, `Decimal`) and some apply only to `str`.

Note:
    - Any `_Unset` objects will be replaced by the corresponding value defined in the `_DefaultValues` dictionary. If a key for the `_Unset` object is not found in the `_DefaultValues` dictionary, it will default to `None`

Args:
    default: Default value if the field is not set.
    default_factory: A callable to generate the default value. The callable can either take 0 arguments
        (in which case it is called as is) or a single argument containing the already validated data.
    alias: The name to use for the attribute when validating or serializing by alias.
        This is often used for things like converting between snake and camel case.
    alias_priority: Priority of the alias. This affects whether an alias generator is used.
    validation_alias: Like `alias`, but only affects validation, not serialization.
    serialization_alias: Like `alias`, but only affects serialization, not validation.
    title: Human-readable title.
    field_title_generator: A callable that takes a field name and returns title for it.
    description: Human-readable description.
    examples: Example values for this field.
    exclude: Whether to exclude the field from the model serialization.
    discriminator: Field name or Discriminator for discriminating the type in a tagged union.
    deprecated: A deprecation message, an instance of `warnings.deprecated` or the `typing_extensions.deprecated` backport,
        or a boolean. If `True`, a default deprecation message will be emitted when accessing the field.
    json_schema_extra: A dict or callable to provide extra JSON schema properties.
    frozen: Whether the field is frozen. If true, attempts to change the value on an instance will raise an error.
    validate_default: If `True`, apply validation to the default value every time you create an instance.
        Otherwise, for performance reasons, the default value of the field is trusted and not validated.
    repr: A boolean indicating whether to include the field in the `__repr__` output.
    init: Whether the field should be included in the constructor of the dataclass.
        (Only applies to dataclasses.)
    init_var: Whether the field should _only_ be included in the constructor of the dataclass.
        (Only applies to dataclasses.)
    kw_only: Whether the field should be a keyword-only argument in the constructor of the dataclass.
        (Only applies to dataclasses.)
    coerce_numbers_to_str: Whether to enable coercion of any `Number` type to `str` (not applicable in `strict` mode).
    strict: If `True`, strict validation is applied to the field.
        See [Strict Mode](../concepts/strict_mode.md) for details.
    gt: Greater than. If set, value must be greater than this. Only applicable to numbers.
    ge: Greater than or equal. If set, value must be greater than or equal to this. Only applicable to numbers.
    lt: Less than. If set, value must be less than this. Only applicable to numbers.
    le: Less than or equal. If set, value must be less than or equal to this. Only applicable to numbers.
    multiple_of: Value must be a multiple of this. Only applicable to numbers.
    min_length: Minimum length for iterables.
    max_length: Maximum length for iterables.
    pattern: Pattern for strings (a regular expression).
    allow_inf_nan: Allow `inf`, `-inf`, `nan`. Only applicable to float and [`Decimal`][decimal.Decimal] numbers.
    max_digits: Maximum number of allow digits for strings.
    decimal_places: Maximum number of decimal places allowed for numbers.
    union_mode: The strategy to apply when validating a union. Can be `smart` (the default), or `left_to_right`.
        See [Union Mode](../concepts/unions.md#union-modes) for details.
    fail_fast: If `True`, validation will stop on the first error. If `False`, all validation errors will be collected.
        This option can be applied only to iterable types (list, tuple, set, and frozenset).
    extra: (Deprecated) Extra fields that will be included in the JSON schema.

        !!! warning Deprecated
            The `extra` kwargs is deprecated. Use `json_schema_extra` instead.

Returns:
    A new [`FieldInfo`][pydantic.fields.FieldInfo]. The return annotation is `Any` so `Field` can be used on
        type-annotated fields without causing a type error.
```

</details>

Which is about on-par with pyright (except for all the TODOs)

---

_Comment by @Gankra on 2025-08-18 23:33_

I've also now heavily restricted the places where multiline printing recursively propagates -- in particular it seems pylance basically only ever multi-line prints a function type if it's the top-level type. As such most types now apply `self.settings.singleline()` to supress multiline printing.

---

_Review comment by @carljm on `crates/ty_ide/src/hover.rs`:210 on 2025-08-19 00:56_

I feel like this is a case where keeping it on one line would be preferable.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:765 on 2025-08-19 01:01_

I guess this `self.parameters.len() > 1` heuristic could probably use some improvement. Ideally it would be based on "how many characters is the rendered display"? That might not be too hard to do, if we approach it as building up a vector of segments, summing their length, and then at the end joining them with the right separator?

---

_@carljm reviewed on 2025-08-19 01:02_

Awesome!

---

_Comment by @Gankra on 2025-08-19 01:45_

<img width="250" height="224" alt="Screenshot 2025-08-18 at 9 44 11 PM" src="https://github.com/user-attachments/assets/b1a1b2f8-2368-41cf-a9e7-99bded11a8b3" />

Note that the `parameters.len() > 1` heuristic is based on my observation of pylance, which indeed seems to have this simplistic approach (however it also fills in a few more details that make it feel less "empty").

---

_Comment by @carljm on 2025-08-19 01:47_

Ah ok! Well that suggests that it's fine to go with this heuristic for now. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:779 on 2025-08-19 06:09_

Does the indent work as expected for nested callable types (where a parameter type is a callable type itself that needs to break over multiple lines) 

---

_@MichaReiser approved on 2025-08-19 06:14_

I'm fine with this simple heuristic for now. I think it would be good to add some tests that demonstrate how the rendering looks when nesting multiple complex types. 

This code looks very similar to what `ruff_formatter` supports and we could use it to e.g. avoid breaking a simple `f(a, b)` signature if it fits into some line length that we pick. 

What I don't know is if the formatter is fast enough for the case where we emit many diagnostics, but it probably is. 

Here's some docs on the formatter supported IR elements: https://github.com/astral-sh/ruff/blob/7dfde3b929c70b5f5fb9933ef09b8005717a8d85/crates/ruff_formatter/src/builders.rs#L1-L0

---

_@Gankra reviewed on 2025-08-19 12:20_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:779 on 2025-08-19 12:20_

This is avoided by the gratuitous uses of `settings.singleline()` preventing nested types from being line-wrapped almost anywhere.

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:779 on 2025-08-19 12:21_

(I'll add tests to show that)


---

_@Gankra reviewed on 2025-08-19 12:21_

---

_Comment by @MichaReiser on 2025-08-19 17:08_

Just in case it isn't clear from my previous comment. I don't think we need to explore using ruff's formatter now. But we may want to if we want to do more fancy rendering

---

_Merged by @Gankra on 2025-08-19 17:31_

---

_Closed by @Gankra on 2025-08-19 17:31_

---

_Branch deleted on 2025-08-19 17:31_

---
