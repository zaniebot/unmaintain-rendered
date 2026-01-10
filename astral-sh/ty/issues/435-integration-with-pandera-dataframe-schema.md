```yaml
number: 435
title: Integration with pandera dataframe schema validation
type: issue
state: closed
author: josh-gree
labels:
  - question
assignees: []
created_at: 2025-05-17T17:14:21Z
updated_at: 2025-05-21T11:12:48Z
url: https://github.com/astral-sh/ty/issues/435
synced_at: 2026-01-10T02:34:10Z
```

# Integration with pandera dataframe schema validation

---

_Issue opened by @josh-gree on 2025-05-17 17:14_

### Summary

I assume that this is probably just due to runtime checking hitting ty checking and the two not being possible to have at the same time!? But would be great if ty could understand such patterns?

```py
import pandas as pd
import pandera.pandas as pa
from pandera.typing import Series, DataFrame


class MySchema(pa.DataFrameModel):
    id: Series[str] = pa.Field(nullable=False)
    value: Series[int] = pa.Field(nullable=True)

    class Config:
        coerce = True


@pa.check_types
def get_data() -> DataFrame[MySchema]:
    data = [
        {"id": "a", "value": 1},
        {"id": "b", "value": 2},
    ]
    df = pd.DataFrame(data)
    # Return directly without validation
    return df

    # Return with explicit validation
    # return MySchema.validate(df)


# Usage
result = get_data()
print(result)
```


With output from ty of;

```
error[invalid-return-type]: Return type does not match returned value
  --> repro.py:22:12
   |
20 |     df = pd.DataFrame(data)
21 |     # Return directly without validation
22 |     return df
   |            ^^ expected `DataFrame[MySchema]`, found `DataFrame`
23 |
24 |     # Return with explicit validation
   |
  ::: repro.py:15:19
   |
14 | @pa.check_types
15 | def get_data() -> DataFrame[MySchema]:
   |                   ------------------- Expected `DataFrame[MySchema]` because of return type
16 |     data = [
17 |         {"id": "a", "value": 1},
   |
info: rule `invalid-return-type` is enabled by default
```

### Version

(3.10.14) ➜  ty-repro git:(main) ✗ uvx ty --version     
ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Comment by @sharkdp on 2025-05-17 19:11_

Thank you for reporting this

> and the two not being possible to have at the same time

I'm not sure I understand. If you `return MySchema.validate(df)` instead of `return df`, then ty reports no error? Isn't that what you want? Runtime validation and no type-check error?

---

_Comment by @josh-gree on 2025-05-18 16:32_

The `@pa.check_types`  should be enough - the point is to return a bare df and then the return is validated to be the given schema by the decorator - so doing `return MySchema.validate(df)` is redundant? If this function doesn't raise an exception then df is `MySchema` - but thats at runtime and thats kinda what I meant about "the two not being possible to have at the same time" - ty would need to understand the semantics of the pandera lib to know that this function will only ever return a `MySchema` if it returns a `DataFrame`?

---

_Comment by @sharkdp on 2025-05-20 09:40_

Thank you for the clarification. So if I understand correctly, the type checker is supposed to accept a returned value of `pandas.DataFrame` in a function that is annotated as returning a `pandera.typing.DataFrame[MySchema]`.

The latter is defined [here](https://github.com/unionai-oss/pandera/blob/88bb60909d1cd7c940bd389107f1cc85b048a506/pandera/typing/pandas.py#L155C1-L155C58):
```py
class DataFrame(DataFrameBase, pd.DataFrame, Generic[T]):
```

We can see that `pandera.typing.DataFrame` a *sub*class of `pandas.DataFrame`, so I'm not sure how this is supposed to work? Do other type checkers support this? If so, do you know if they have specialized code / plugins to make this possible? Or am I missing something?

---

_Comment by @MichaReiser on 2025-05-20 09:43_

@sharkdp I think the idea is that the `pa.check_types` decorator performs the `validate` call before returning the data frame. 

---

_Comment by @sharkdp on 2025-05-20 12:41_

> [@sharkdp](https://github.com/sharkdp) I think the idea is that the `pa.check_types` decorator performs the `validate` call before returning the data frame.

I understand how this works at runtime, yes. But that doesn't change the fact that the decorated function (`get_data`) has a seemingly wrong return type? The decorator can change the return type, sure. But the return type annotation must still be valid for `get_data` alone, decorated or not.

---

_Comment by @carljm on 2025-05-20 14:22_

I agree with @sharkdp. The way Python static type annotations are intended to work with decorators is that `get_data` needs to be annotated to return whatever type its body returns (`pd.DataFrame` in this case?), and the `pa.check_types` decorator would need to take `MySchema` as an argument, and be a generic function that takes in one `Callable` type and returns a `Callable` type which returns the pandera `DataFrame[MySchema]` type.

So I don't see any possibility that we would support this pattern as currently written; the type annotations are just incorrect, according to the static typing specification.

---

_Comment by @josh-gree on 2025-05-21 10:43_

This conclusion makes sense to me - the typing is "wrong" but it needs to be wrong for the decorator to do what you want it to do - so as I thought this is probably just a situation where I would need to ignore the type error 

---

_Label `question` added by @MichaReiser on 2025-05-21 11:12_

---

_Closed by @MichaReiser on 2025-05-21 11:12_

---
