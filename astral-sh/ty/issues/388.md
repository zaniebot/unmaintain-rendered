```yaml
number: 388
title: "error[invalid-type-form]: Function calls are not allowed in type expressions with Pydantic conlist"
type: issue
state: closed
author: alex-goswag
labels:
  - question
  - diagnostics
assignees: []
created_at: 2025-05-14T15:45:22Z
updated_at: 2025-06-24T18:31:00Z
url: https://github.com/astral-sh/ty/issues/388
synced_at: 2026-01-10T02:07:35Z
```

# error[invalid-type-form]: Function calls are not allowed in type expressions with Pydantic conlist

---

_Issue opened by @alex-goswag on 2025-05-14 15:45_

### Summary

I can use `Field(min_length=1)` as a work around or maybe that is the better way to do it but I guess `conlist` should still be supported by `ty` ?


# Example 

```python
# !pip install pydantic==2.11.4
# !pip install ty==0.0.1a1

from pydantic import BaseModel, Field, conlist


class OrderLine(BaseModel):
    product_id: int
    quantity: int


class Order(BaseModel):
    order_lines: conlist(item_type=OrderLine, min_length=1)
    field_order_lines: list[OrderLine] = Field(
        min_length=1,
    )


if __name__ == "__main__":
    order = Order(
        order_lines=[OrderLine(product_id=1, quantity=10)],
        field_order_lines=[OrderLine(product_id=1, quantity=10)],
    )
    print(order)
```

# Ty

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-type-form]: Function calls are not allowed in type expressions
  --> py.py:13:18
   |
12 | class Order(BaseModel):
13 |     order_lines: conlist(item_type=OrderLine, min_length=1)
   |                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
14 |     field_order_lines: list[OrderLine] = Field(
15 |         min_length=1,
   |
info: rule `invalid-type-form` is enabled by default

Found 1 diagnostic

```

# Mypy 

`pip install mypy==1.15.0`

```
py.py:13: error: Invalid type comment or annotation  [valid-type]
py.py:13: note: Suggestion: use conlist[...] instead of conlist(...)
Found 1 error in 1 file (checked 1 source file)
```



### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Comment by @sharkdp on 2025-05-14 17:50_

Thank you very much for reporting this.

Call expressions are indeed [not valid in type annotations](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions), so mypy and ty are correct here. As @AlexWaygood suggested, we could probably add a hint to the `invalid-type-form` diagnostic to help users.

Apart from that, it's not completely clear how we could improve the behavior here?

---

_Label `question` added by @sharkdp on 2025-05-14 17:51_

---

_Label `diagnostics` added by @sharkdp on 2025-05-14 17:51_

---

_Comment by @carljm on 2025-05-14 17:58_

Is the use of `conlist` as a function in type annotations something that is recommended and supported by pydantic? This seems like a problem that should be addressed in pydantic, because it's very clear in the typing specification that calls in annotations are not supported, and other type checkers also prohibit it.

Happy to continue the discussion here and reopen if a concrete improvement suggestion arises from discussion, but in order to keep the open issues actionable, I'm going to close this as "not planned" for now.

---

_Closed by @carljm on 2025-05-14 17:58_

---

_Comment by @sharkdp on 2025-05-14 18:59_

> As [@AlexWaygood](https://github.com/AlexWaygood) suggested, we could probably add a hint to the `invalid-type-form` diagnostic to help users.

That has been added in https://github.com/astral-sh/ruff/pull/18104.

---

_Comment by @jc-louis on 2025-05-15 08:18_

Another case which is [recommended by pydantic](https://docs.pydantic.dev/latest/concepts/models/#dynamic-model-creation) 

```py
from pydantic import create_model, BaseModel


DynamicFoobarModel = create_model('DynamicFoobarModel', foo=str, bar=(int, 123))

class StaticFoobarModel(BaseModel):
    foo: str
    bar: int = 123

# DynamicFoobarModel and StaticFoobarModel are equivalent
# https://docs.pydantic.dev/latest/concepts/models/#dynamic-model-creation

type mytypestatic = list[StaticFoobarModel] # OK
type mytypedynamic = list[DynamicFoobarModel] # Variable of type `type[BaseModel]` is not allowed in a type expression


class MyNestedDynamic(BaseModel):
    inner: DynamicFoobarModel # Variable of type `type[BaseModel]` is not allowed in a type expression

class MyNestedStatic(BaseModel):
    inner: StaticFoobarModel

# Both work fine
print(MyNestedDynamic.model_json_schema())
print(MyNestedStatic.model_json_schema())
```

ty output (which is right according to [spec](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions))
```
error[invalid-type-form] Variable of type `type[BaseModel]` is not allowed in a type expression
error[invalid-type-form] Variable of type `type[BaseModel]` is not allowed in a type expression
```

---

_Comment by @MichaReiser on 2025-05-15 08:23_

For others interested. [Here's](https://docs.pydantic.dev/1.10/usage/types/#constrained-types) where this is recommended in the pydantic docs.

---

_Comment by @MichaReiser on 2025-05-15 08:26_

It seems the `con` functions are discouraged, see https://github.com/pydantic/pydantic/issues/9011#issuecomment-1999970835



---

_Comment by @OliverFarren on 2025-06-24 18:30_

I found this a _little_ tricky to piece together myself for handling constrained lists. 

[This](https://github.com/pydantic/pydantic/issues/9011#issuecomment-1999970835) did not work for me. 

This worked for me and mypy

``` python
from typing import Annotated
from pydantic import BaseModel, conlist


TEXT_EMBEDDING_ADA_002_DIMENSIONALITY = 1536

class Model(BaseModel):
    embeddings: Annotated[
        list[float] | None,
        conlist(
            float,
            min_length=TEXT_EMBEDDING_ADA_002_DIMENSIONALITY,
            max_length=TEXT_EMBEDDING_ADA_002_DIMENSIONALITY,
        ),
    ] = None
```

---
