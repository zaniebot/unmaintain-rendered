```yaml
number: 980
title: "ty wrongly respects `dataclasses.field` as a field specifier for a custom `dataclass_transform`"
type: issue
state: closed
author: carljm
labels:
  - dataclasses
assignees: []
created_at: 2025-08-13T23:15:34Z
updated_at: 2025-08-20T18:33:24Z
url: https://github.com/astral-sh/ty/issues/980
synced_at: 2026-01-12T15:54:24Z
```

# ty wrongly respects `dataclasses.field` as a field specifier for a custom `dataclass_transform`

---

_@carljm_

In this example code:

```py
from typing import dataclass_transform
from dataclasses import field, dataclass

@dataclass_transform()
def create_model(*, init: bool = True):
    def deco[T](cls: T) -> T:
        return cls
    return deco

@create_model()
class A:
    name: str = field(init=False)
   
reveal_type(A(name="foo").name) 

@dataclass
class B:
    name: str = field(init=False)

reveal_type(B(name="foo").name)
```

We have a custom dataclass transform, and one class created using it, and another class created using regular `dataclass` decorator. Both classes use `dataclasses.field` to specify a field.

The [typing spec](https://typing.python.org/en/latest/spec/dataclasses.html#dataclass-transform-parameters) says this about the `field_specifiers` argument to `dataclass_transform`:

> If not specified, field_specifiers will default to an empty tuple (no field specifiers supported). The standard dataclass behavior supports only one type of field specifier called Field plus a helper function (field) that instantiates this class, so if we were describing the stdlib dataclass behavior, we would provide the tuple argument (dataclasses.Field, dataclasses.field)

This pretty clearly means that `dataclasses.Field` and `dataclasses.field` should _not_ be accepted by default as field specifiers for a custom dataclass transform that doesn't list them explicitly in `field_specifiers`.

Mypy/pyright/pyrefly all implement this, with the result that in the above code, the call `A(name="foo")` is not an error, since `field` is not a field specifier for `create_model`, therefore `init=False` is ignored there. Only `B(name="foo")` is an error.

In ty, we currently wrongly respect `dataclasses.field` as a field specifier for `create_model`.

---

_Label `dataclasses` added by @carljm on 2025-08-13 23:15_

---

_Comment by @leandrobbraga on 2025-08-15 00:19_

Indeed `mypy` check this code as valid and `ty` does not, but I'm getting a runtime error trying to run the code you provided, which matches `ty` error:

`t.py`

```
from dataclasses import field
from typing import dataclass_transform


@dataclass_transform()
def create_model(*, init: bool = True):
    def deco[T](cls: T) -> T:
        return cls

    return deco


@create_model()
class A:
    name: str = field(init=False)


print(A(name="foo").name)
```

```console
uv run teste.py
Traceback (most recent call last):
    A(name="foo").name
    ~^^^^^^^^^^^^
TypeError: A() takes no arguments
> uv run python --version
Python 3.13.5
```

Am I missing something?

---

_Comment by @carljm on 2025-08-15 00:39_

That's because `create_model` in the example claims it is a `dataclass_transform` but doesn't actually transform anything. The example assumes that a real implementation of `create_model` would actually synthesize an `__init__` method for the given class, like dataclasses does. In other words, this example shows a buggy/wrong implementation of `create_model`. That's fine for the example, because type checkers don't verify the internal implementation of a `dataclass_transform`, they just trust it, and this example is just meant to test the type checker behavior. But it does mean you can't usefully check this example against its runtime behavior.

Ty is wrongly erroring because it is respecting the `field(init=False)` (even though it shouldn't, since `create_model` doesn't have a `field_specifiers`). The runtime error is for a totally different reason, because no `__init__` method is created at all by this buggy `create_model` implementation.

If a real implementation of `create_model` would actually respect options passed to `dataclasses.field`, then it should list `dataclasses.field` in its `field_specifiers`.

---

_Comment by @leandrobbraga on 2025-08-15 02:55_

I was giving a shot at it and would like an advise to how to implement it.

For now I'm doing the following:

First, in [bind.rs](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/call/bind.rs#L897-L924) I check weather the `field_specifier` is some, if it is I add a `field_specifier` bit in the `DataclassTransformerParams` bitflags. The idea is to identify whenever this value is the default empty tuple value or there are some value here, I'm not parsing the exact values for now, I only care if it's the empty tuple.

```rust
Some(KnownFunction::DataclassTransform) => {
    if let [
        eq_default,
        order_default,
        kw_only_default,
        frozen_default,
        field_specifiers,
        _kwargs,
    ] = overload.parameter_types()
    {
        let mut params = DataclassTransformerParams::empty();

        if to_bool(eq_default, true) {
            params |= DataclassTransformerParams::EQ_DEFAULT;
        }
        if to_bool(order_default, false) {
            params |= DataclassTransformerParams::ORDER_DEFAULT;
        }
        if to_bool(kw_only_default, false) {
            params |= DataclassTransformerParams::KW_ONLY_DEFAULT;
        }
        if to_bool(frozen_default, false) {
            params |= DataclassTransformerParams::FROZEN_DEFAULT;
        }

        if let Some(field_specifiers_type) = field_specifiers {
            // For now, we'll do a simple check: if field_specifiers is not
            // None/empty, we assume it might contain dataclasses.field
            // TODO: Implement proper parsing to check for
            //   dataclasses.field/Field specifically
            if !field_specifiers_type.is_none(db) {
                params |= DataclassTransformerParams::FIELD_SPECIFIERS;
            }
            // If field_specifiers is None or empty tuple, don't set the
            // flag (default behavior)
        }

        overload.set_return_type(Type::DataclassTransformer(params));
    }
}
```

Now we need to somehow ignore the `Field` in [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/class.rs#L2345-L2349).
```rust
if let Some(Type::KnownInstance(KnownInstanceType::Field(field))) = default_ty {
    init = field.init(db);
    kw_only = field.kw_only(db);
}
```

The only way that I found to communicate the flag `DataclassTransformerParams::FIELD_SPECIFIERS` flag was to create a new flag in the [DataclassParams](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L477-L522) to convey this meaning. But this is very unsatisfying, since `@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False, match_args=True, kw_only=False, slots=False, weakref_slot=False)` does not contain such flag.
```rust
bitflags! {
    /// Used for the return type of `dataclass(…)` calls. Keeps track of the arguments
    /// that were passed in. For the precise meaning of the fields, see [1].
    ///
    /// [1]: https://docs.python.org/3/library/dataclasses.html
    #[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
    pub struct DataclassParams: u16 {
        const INIT = 0b0000_0000_0001;
        const REPR = 0b0000_0000_0010;
        const EQ = 0b0000_0000_0100;
        const ORDER = 0b0000_0000_1000;
        const UNSAFE_HASH = 0b0000_0001_0000;
        const FROZEN = 0b0000_0010_0000;
        const MATCH_ARGS = 0b0000_0100_0000;
        const KW_ONLY = 0b0000_1000_0000;
        const SLOTS = 0b0001_0000_0000;
        const WEAKREF_SLOT = 0b0010_0000_0000;
    }
}
```

Do you have any idea on how to implement this?

---

_Comment by @carljm on 2025-08-15 19:19_

I think it is fine to have `DataclassParams` also track some things from `DataclassTransformerParams` which are not actually overridable in the individual decorator-factory call. (We should have comments to clarify which is which, though.) The alternative would be to have the `DataclassDecorator` type actually track the origin `DataClassTransformerParams`, but that doesn't seem necessarily better?

@sharkdp wrote our dataclasses implementation, and he is returning from vacation next week, so if you don't reach a satisfactory approach by then, he may have thoughts as well.

---

_Comment by @leandrobbraga on 2025-08-15 20:47_

Ok, can you assign this to me? I'm out until Wednesday, but I already have a working version in my computer. 

---

_Assigned to @leandrobbraga by @carljm on 2025-08-15 20:48_

---

_Comment by @sharkdp on 2025-08-19 08:21_

Adding additional flags to `DataclassParams` seems fine to me, but if I understand correctly, this would just be a temporary solution to fix this bug? Eventually, we'd have to keep track of the static list of classes that describe fields, right? And once we do, is there still a need for that flag?

---

_Comment by @carljm on 2025-08-19 16:09_

My interpretation is that in the future the field will change from a boolean flag to a list of classes/functions, but I think the underlying issue will remain the same either way: sometimes handling of a specific field in a dataclass needs access to information defined in the original dataclass-transform definition, and we currently don't thread that through. The options for threading it through will either be to copy some fields from `DataclassTransformerParams` to `DataclassParams` at each usage of that particular dataclass-transform, or to otherwise thread through the `DataclassTransformerParams` itself.

---

_Closed by @carljm on 2025-08-20 18:33_

---
