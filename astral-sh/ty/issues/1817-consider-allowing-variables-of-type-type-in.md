```yaml
number: 1817
title: "Consider allowing variables of type `type[...]` in explicit specializations"
type: issue
state: closed
author: carljm
labels:
  - needs-decision
  - generics
assignees: []
created_at: 2025-12-09T01:49:38Z
updated_at: 2025-12-17T03:49:13Z
url: https://github.com/astral-sh/ty/issues/1817
synced_at: 2026-01-12T15:54:25Z
```

# Consider allowing variables of type `type[...]` in explicit specializations

---

_@carljm_

Here is the original use case presented at https://discord.com/channels/1039017663004942429/1279201882337705994/1446534091901108355

```py
# INEResponse is a generic BaseModel that accepts a BaseDataPoint type, thus changing what kind of response it attempts to validate

class _INEResponse[TData: BaseDataPoint](BaseModel):
   ...

# "Indicator" is the metadata of the dataset being retrieved from the API, including its ID

class Indicator[TRow: _RowType](BaseModel):
    model: type[BaseDataPoint[TRow]]

# _get_indicator should accept an Indicator with an expected row model to validate the response
# and then create a parametrized _INEResponse[expected_response_type] so that it can call .model_validate on the response

async def _get_indicator[TRow: _RowType](
    indicator: Indicator[TRow],
) -> _INEResponse[BaseDataPoint[TRow]]:

    ... # Response is retrieved from an API call

    # we emit `[invalid-type-form] Variable of type `type[...]` is not allowed in a type expression`
    return _INEResponse[indicator.model].model_validate(response)
```

Simplified version that still demonstrates the core issue:

```py
class Base:
    pass

class Response[TData: Base]:
   ...

class Indicator:
    model: type[Base]

def get_indicator(indicator: Indicator) -> Response[Base]:
    # we emit `[invalid-type-form] Variable of type `type[Base]` is not allowed in a type expression`
    return Response[indicator.model]()
```

https://play.ty.dev/4373a5c6-734d-463a-b744-3bf0955ffe35

---

_Added to milestone `Stable` by @carljm on 2025-12-09 01:49_

---

_Label `needs-decision` added by @carljm on 2025-12-09 01:49_

---

_Label `generics` added by @carljm on 2025-12-09 01:49_

---

_Comment by @carljm on 2025-12-09 01:54_

Pyright and pyrefly both allow this; mypy does not. (And mypy's error message is even stranger; it claims that `indicator.model` is not defined.)

It's a little strange to allow this, in that it makes explicit specializations kind of a hybrid type-expression/value-expression. `indicator.model` is not allowed as an annotation by either pyright or pyrefly.

I think it's also unsound to allow it if `Response` is invariant in `TData`. Since `type[Base]` includes subclasses of `Base`, this means we could be specializing `Response` to a specialization that is not actually a subtype of `Response[Base]` at all.

---

_Removed from milestone `Stable` by @carljm on 2025-12-09 01:54_

---

_Comment by @AlexWaygood on 2025-12-09 13:24_

To me this just seems like a bug report we should file with pyright and Pyrefly. But I imagine we might do better here due to the fact that no other type checker has the concept of class-literal types as first-class types.

---

_Comment by @carljm on 2025-12-09 18:32_

Discussed this with a pyrefly maintainer, it sounds like we are agreed that this should not be allowed. The typing spec currently says that if `a.b` is a type expression, `a` should be a module or package. https://github.com/python/typing/issues/1884 aims to clarify that it can also be a class, but it cannot be an instance. (And the name must ultimately reference "a valid in-scope class, type alias, or TypeVar", which does not include a variable of type `type[...]`.)

---

_Closed by @carljm on 2025-12-09 18:32_

---

_Comment by @5j9 on 2025-12-16 15:13_

I had a code like this which I believe has the same issue as the one being discussed:
```python
from typing import Any, Literal, reveal_type

from pydantic import BaseModel


class Resp[Data](BaseModel):
    isSuccess: Literal[True]
    message: str
    errorCode: int
    problemDetail: None
    data: Data


class C:
    def _call_api_and_return_model_instance[T: BaseModel](
        self,
        api_call_params: Any,
        expected_response_model: type[T],
    ) -> T:
        text: str = 'API response obtained after processing api_call_params'
        return expected_response_model.model_validate_json(text)

    def get_data[DataModel](
        self,
        api_call_params: Any,
        expected_data_model: type[DataModel],
    ):
        m = self._call_api_and_return_model_instance(
            api_call_params,
            Resp[expected_data_model],
        )
        return m.data


reveal_type(C().get_data(any, str))
```
<s>
As a workaround I introduced `make_resp` as follows:

```python
from typing import Any, Literal, reveal_type

from pydantic import BaseModel


class Resp[Data](BaseModel):
    isSuccess: Literal[True]
    message: str
    errorCode: int
    problemDetail: None
    data: Data


class C:
    def _call_api_and_return_model_instance[T: BaseModel](
        self,
        api_call_params: Any,
        expected_response_model: type[T],
    ) -> T:
        text: str = 'API response obtained after processing api_call_params'
        return expected_response_model.model_validate_json(text)

    def get_data[DataModel](
        self,
        api_call_params: Any,
        expected_data_model: type[DataModel],
    ):
        resp_type = make_resp(expected_data_model)
        m = self._call_api_and_return_model_instance(
            api_call_params,
            resp_type,
        )
        return m.data


def make_resp[T](model: type[T]) -> type[Resp[T]]:
    return Resp[T]


reveal_type(C().get_data(any, str))
```

Now ty is revealing `unknown` instead of `str` at the last line. I don't understand why.</s> Any clues or other workarounds? 

---

_Comment by @carljm on 2025-12-16 16:25_

@5j9 You have no return type annotation on your `get_data` method. Until #128 is closed, we don't do return type inference on un-annotated methods, so if there is no annotated return type, the return type is `Unknown`.

---
