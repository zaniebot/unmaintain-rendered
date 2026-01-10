```yaml
number: 9352
title: "`ANN001` and `ANN201` for override methods?"
type: issue
state: open
author: ReubenFrankel
labels:
  - configuration
assignees: []
created_at: 2024-01-02T03:36:24Z
updated_at: 2024-09-25T13:29:54Z
url: https://github.com/astral-sh/ruff/issues/9352
synced_at: 2026-01-10T11:09:51Z
```

# `ANN001` and `ANN201` for override methods?

---

_Issue opened by @ReubenFrankel on 2024-01-02 03:36_

When overriding a method from a parent class, is it required to define the entire original method signature with its argument and return types? I'm not sure if this is a personal preference thing, but I don't really want to have to redefine or import any non-builtin types if I don't have to (especially for more complex methods) - VS Code IntelliSense seems to pick these up already without any type hints.

Given the following example

`example.py`
```py
"""Example module."""

from singer_sdk.streams.rest import RESTStream
from typing_extensions import override


class MyRESTStream(RESTStream):
    """My RESTStream implementation."""

    @override
    def prepare_request(self, context, next_page_token):
        return super().prepare_request(context, next_page_token)

```

where `RESTStream.prepare_request` is a fully-annotated method...

### Expected
No errors

### Actual
```
example.py:11:9: ANN201 Missing return type annotation for public function `prepare_request`
example.py:11:31: ANN001 Missing type annotation for function argument `context`
example.py:11:40: ANN001 Missing type annotation for function argument `next_page_token`
Found 3 errors.
```

This is the code that fixes the above 3 errors and some others after importing:

`example.py`
```py
"""Example module."""

from __future__ import annotations

from typing import TYPE_CHECKING, Any

from singer_sdk.streams.rest import RESTStream
from typing_extensions import override

if TYPE_CHECKING:
    from requests.models import PreparedRequest


class MyRESTStream(RESTStream):
    """My RESTStream implementation."""

    @override
    def prepare_request(
        self,
        context: dict | None,
        next_page_token: Any | None,
    ) -> PreparedRequest:
        return super().prepare_request(context, next_page_token)

``` 

---

_Label `configuration` added by @charliermarsh on 2024-01-02 18:13_

---

_Comment by @charliermarsh on 2024-01-02 18:14_

I'd be interested to get more opinions on this, I can see both sides.

---

_Comment by @tylerlaprade on 2024-09-24 19:48_

I came searching for this because after adding `ANN201`, I have to add `Response` to every single one of my overridden Django ViewSet methods when the type-checker already knows they always have to return a `Response`.

---

_Comment by @zanieb on 2024-09-24 19:59_

cc @AlexWaygood / @MichaReiser 

Do our plans for red-knot make this clearer? I basically think we should not enforce annotations for overrides with these rules.

---

_Comment by @MichaReiser on 2024-09-25 06:53_

> Do our plans for red-knot make this clearer? I basically think we should not enforce annotations for overrides with these rules.

I don't think the decision here is red knot specific. Red knot will provide better means to reason about overrides but it's a non-goal for the first release to improve any lint rules. 

I do agree that requiring type annotations on overrides is overly strict but not requiring them has its own downsides, unless Ruff has other rules that fill the gaps:

* Error about methods annotated with `@override` that don't override a base method
* Error about methods that have an incompatible signature with an override method
* Error when the function's body isn't compatible with the type's from the base method

I could see us going middle ground here where a method decorated with `override` should either be fully annotated or not annotated at all. 

---

_Comment by @tylerlaprade on 2024-09-25 13:29_

All three of those bullet points will be covered by the type-checker. I wouldn't expect a linter to have an opinion on them.

---
