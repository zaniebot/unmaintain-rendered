```yaml
number: 1068
title: support field_specifiers in dataclass_transform
type: issue
state: closed
author: carljm
labels:
  - feature
  - dataclasses
  - typing semantics
assignees: []
created_at: 2025-08-20T21:25:18Z
updated_at: 2025-10-16T18:49:13Z
url: https://github.com/astral-sh/ty/issues/1068
synced_at: 2026-01-10T02:06:24Z
```

# support field_specifiers in dataclass_transform

---

_Issue opened by @carljm on 2025-08-20 21:25_

This is important, because it is used by two of the most popular clients of `dataclass_transform`, attrs and pydantic.

---

_Added to milestone `Beta` by @carljm on 2025-08-20 21:25_

---

_Label `feature` added by @carljm on 2025-08-20 21:25_

---

_Label `typing semantics` added by @carljm on 2025-08-20 21:25_

---

_Comment by @carljm on 2025-08-22 13:49_

@PrettyWood is [planning to look into this](https://github.com/astral-sh/ruff/pull/19825#issuecomment-3211169196)

---

_Label `dataclasses` added by @AlexWaygood on 2025-08-22 13:51_

---

_Assigned to @carljm by @carljm on 2025-08-22 14:07_

---

_Comment by @carljm on 2025-08-22 14:07_

For some reason GitHub won't let me actually assign the issue to @PrettyWood, so I'm assigning it to myself, just to mark that it isn't unowned

---

_Comment by @AlexWaygood on 2025-08-22 14:09_

> For some reason GitHub won't let me actually assign the issue to [@PrettyWood](https://github.com/PrettyWood), so I'm assigning it to myself, just to mark that it isn't unowned

for people outside the organisation, they need to have commented on the issue before GitHub will let you assign them to the issue

---

_Comment by @carljm on 2025-08-22 14:20_

> for people outside the organisation, they need to have commented on the issue before GitHub will let you assign them to the issue

I feel like every month or so I learn this again and then forget it again

---

_Comment by @PrettyWood on 2025-08-22 14:35_

I'm actually working on it directly in https://github.com/astral-sh/ruff/pull/19825.

It's trickier than I thought because when I first started, I thought `field_specifiers `was the only missing piece and I went with
- parse `@dataclass_transform(field_specifiers=(Field, ...))
- store the field specifiers
- detect calls to those field specifiers
- apply dataclass-like validation (via FieldInstance...)

The assumption was that `@dataclass_transform` itself was already fully supported but it's not. I'm working on making it understood for `attrs` and `pydantic`
I'll keep you posted

---

_Assigned to @PrettyWood by @carljm on 2025-08-22 14:56_

---

_Unassigned @carljm by @carljm on 2025-08-22 14:56_

---

_Comment by @carljm on 2025-08-22 15:08_

> The assumption was that `@dataclass_transform` itself was already fully supported but it's not. I'm working on making it understood for `attrs` and `pydantic`

Interesting, I thought it was supported enough for your plan in the above bullet points to be sufficient. What else is missing? It seems like we support it enough for `kw_only_default` to be respected.

---

_Comment by @PrettyWood on 2025-08-23 08:18_

@carljm metaclasses decorated with `@dataclass_transform(kw_only_default=True)` did not forward the information to the applied classes, which is what `pydantic` uses
But it's ok it's now fixed

<details>
  <summary>Here is a summary of current behaviour for dataclass_transform, attrs and pydantic</summary>

```py
import attrs
import pydantic
from typing import dataclass_transform, reveal_type
from dataclasses import dataclass


@dataclass(kw_only=True)
class DataclassUser:
    name: str
    age: int


reveal_type(DataclassUser.__init__)
# # `(self: DataclassUser, *, name: str, age: int) -> None` 🟢 


@dataclass_transform(kw_only_default=True)
def model(cls):
    return cls


@model
class CustomUser:
    name: str
    age: int


reveal_type(CustomUser.__init__)
# `(self: CustomUser, *, name: str, age: int) -> None` 🟢


@dataclass_transform(kw_only_default=True)
class ModelMeta(type):
    pass


class CustomModel(metaclass=ModelMeta):
    def __init__(self, **data):
        for k, v in data.items():
            setattr(self, k, v)


class CustomMetaclassUser(CustomModel):
    name: str
    age: int


reveal_type(CustomMetaclassUser.__init__)
# `(self: CustomMetaclassUser, name: str, age: int) -> None` 🟠 (`kw_only_default` not recognised)

@attrs.define(kw_only=True)
class AttrsUser:
    name: str
    age: int


reveal_type(AttrsUser.__init__)
# `(self: AttrsUser, name: str, age: int) -> None` 🟠
# (`kw_only_default` not recognised but expected because stub file doesn't add `kw_only_default` in `@dataclass_transform`)


class PydanticUser(pydantic.BaseModel):
    name: str
    age: int


reveal_type(PydanticUser.__init__)
# `(self: PydanticUser, __pydantic_extra__: @Todo(unknown type subscript) | None = Any, __pydantic_fields_set__: set[str] = Any, __pydantic_private__: @Todo(unknown type subscript) | None = Any, name: str, age: int) -> None` 🔴
# `kw_only_default` not recognised because Pydantic uses metaclass
# extra fields in signature that will be removed when `field_specifiers` are implemented)
```
</details>

---

_Comment by @PrettyWood on 2025-08-24 17:45_

@carljm @sharkdp (pinging you because you worked on https://github.com/astral-sh/ruff/pull/17445 🙏 )
Before opening a PR I would like to get your guidance

To support what I just wrote above (pydantic-like usage with metaclass) I simply changed

```diff
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2022,7 +2022,10 @@ impl<'db> ClassLiteral<'db> {
         specialization: Option<Specialization<'db>>,
         name: &str,
     ) -> Option<Type<'db>> {
-        let dataclass_params = self.dataclass_params(db);
+        let dataclass_params = self.dataclass_params(db).or(self
+            .try_metaclass(db)
+            .ok()
+            .and_then(|(_, transformer_params)| transformer_params.map(Into::into)));
         let has_dataclass_param =
             |param| dataclass_params.is_some_and(|params| params.contains(param));
```

It works and 

```py
@dataclass_transform(kw_only_default=True)
def my_dataclass[T](cls: type[T]) -> type[T]:
    return cls

@my_dataclass
class M2:
    x: int

reveal_type(M2.__init__)
```

now gives `(self: M2, *, x: int) -> None` (when it used to be without `*` so no kw-only)

But I don't really like the fact that this code

```rs
            if let Type::FunctionLiteral(f) = decorator_ty {
                // We do not yet detect or flag `@dataclass_transform` applied to more than one
                // overload, or an overload and the implementation both. Nevertheless, this is not
                // allowed. We do not try to treat the offenders intelligently -- just use the
                // params of the last seen usage of `@dataclass_transform`
                let params = f
                    .iter_overloads_and_implementation(self.db())
                    .find_map(|overload| overload.dataclass_transformer_params(self.db()));
                if let Some(params) = params {
                    dataclass_params = Some(params.into());
                    continue;
                }
            }

            if let Type::DataclassTransformer(params) = decorator_ty {
                dataclass_transformer_params = Some(params);
                continue;
            }
```

is only called for function literals.

Also in my code the actual `field_specifiers` are stored separately in `FieldSpecifiers` 

```rs
#[salsa::interned(debug, heap_size=ruff_memory_usage::heap_size)]
pub struct FieldSpecifiers<'db> {
    #[returns(ref)]
    pub specifiers: Vec<Type<'db>>,
}
```

They are linked via `OverloadLiteral::field_specifiers` to keep this struct `Copy`-able. Why does it need to be `Copy`? Only `Clone` wouldn't be enough?

What I would love is

- parse `@dataclass_transform(...)`
- create a unified `DataclassParams` that works the same way for both `@dataclass` and `@dataclass_transform`
  We would have `DataclassParams` with field specifiers = `(dataclasses.field, dataclasses.Field)` by default. If it comes from `dataclass_transform` we set them to `()` if no field specifiers is set, else the set fields specifiers.
- unified logic in `own_fields()`

Thank you for your help

---

_Comment by @sharkdp on 2025-08-26 11:22_

> To support what I just wrote above (pydantic-like usage with metaclass) I simply changed […] It works and […] now gives (self: M2, *, x: int) -> None (when it used to be without * so no kw-only)

👍 

> But I don't really like the fact that this code […] is only called for function literals.

Can you go into this in more detail? What kind of code sample do you have in mind that wouldn't be supported?

> Also in my code the actual `field_specifiers` are stored separately in `FieldSpecifiers` […] They are linked via `OverloadLiteral::field_specifiers` to keep this struct `Copy`-able. Why does it need to be `Copy`? Only `Clone` wouldn't be enough?

Do you mean: "why does `OverloadLiteral` need to be `Copy`"? If so, `OverloadLiteral` is used inside `FunctionLiteral`. `FunctionLiteral` is used inside `FunctionType`. And `FunctionType` is used in the `Type::FunctionLiteral` variant, where `Type` is currently `Copy`. I think the motivation for that is mostly just "for convenience". We want it to be a small type that's cheap to clone anyway, so making it `Copy` too allows for much easier handling of that type (e.g. no explicit `.clone()` calls everywhere). It also allows us to use `Type` directly in arguments and return types of salsa queries, which do not allow for references. Maybe there is also an even stricter requirement for it to be `Copy`, but I can't remember one right now.

> What I would love is
> 
>     * parse `@dataclass_transform(...)`

What do you mean by parse? Why is returning a `Type::DataclassTransformer(…)` variant from calls to `dataclass_transform` not good enough?



> create a unified DataclassParams that works the same way for both @dataclass and @dataclass_transform
We would have DataclassParams with field specifiers = (dataclasses.field, dataclasses.Field) by default. If it comes from dataclass_transform we set them to () if no field specifiers is set, else the set fields specifiers.

That seems reasonable

> unified logic in `own_fields()`

I'm not sure what that means.

---

_Comment by @carljm on 2025-09-09 17:42_

https://github.com/astral-sh/ty/issues/1159 needs this, specifically support for the `alias` parameter.

---

_Comment by @Gerharddc on 2025-10-03 09:13_

It's a bit unclear if this is related to the issue discussed here, but when I use https://github.com/openapi-generators/openapi-python-client to generate an openapi cient, it generates a class similar to:

```
...
from attrs import define, evolve, field

@define
class Client:
    ...
    raise_on_unexpected_status: bool = field(default=False, kw_only=True)
    _base_url: str = field(alias="base_url")
    ...
    _httpx_args: dict[str, Any] = field(factory=dict, kw_only=True, alias="httpx_args")
    ...
```

When I then try to use it as follows:

`client = Client(base_url="http://10.1.3.3", raise_on_unexpected_status=True, httpx_args={ "transport": RetryTransport() })`

I get `unknown-argument` warnings for `base_url` and `httpx_args` which are using "alias" but there is no warning for `raise_on_unexpected_status` which does not use it.

---

_Comment by @sharkdp on 2025-10-06 11:20_

This is related, yes. In addition, that will require us to support the `alias` field of field specifiers.

---

_Unassigned @PrettyWood by @sharkdp on 2025-10-09 10:33_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-09 10:33_

---

_Closed by @sharkdp on 2025-10-16 18:49_

---
