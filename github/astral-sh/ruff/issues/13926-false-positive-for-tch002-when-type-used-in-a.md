---
number: 13926
title: False positive for TCH002 when type used in a method
type: issue
state: open
author: last-partizan
labels: []
assignees: []
created_at: 2024-10-25T12:31:59Z
updated_at: 2025-07-15T12:14:27Z
url: https://github.com/astral-sh/ruff/issues/13926
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for TCH002 when type used in a method

---

_Issue opened by @last-partizan on 2024-10-25 12:31_

I'm using DRF and drf-spectacular, and it uses runtime type introspection for some cases (like `serializers.SerializerMethodField()`. And i have no way to tell `ruff` to not move those types into TYPE_CHECKING block.

I tried `runtime-evaluated-base-classes`, but looks like it only works for class fields, not class methods.

```
> ruff --version
ruff 0.7.1
```

```toml
[tool.ruff]
target-version = "py39"

[tool.ruff.lint]
select = ["TCH"]

[tool.ruff.lint.flake8-type-checking]
runtime-evaluated-base-classes = [
	"rest_framework.serializers.Serializer",
	"rest_framework.serializers.ModelSerializer",
]
```

```py
from __future__ import annotations

from rest_framework import serializers

from app.models import Book


class BookSerializer(serializers.Serializer):
    name = serializers.SerializerMethodField()

    def get_name(self, instance: Book) -> str:
        return instance.name
```

```sh
> ruff check --select TCH project/serializers.py
project/serializers.py:5:24: TCH002 Move third-party import `app.models.Book` into a type-checking block
  |
3 | from rest_framework import serializers
4 |
5 | from app.models import Book
  |                        ^^^^ TCH002
  |
  = help: Move into type-checking block
```

Workaround for this is probably using `runtime-evaluated-base-classes` and adding `instance: MyClass` for affected serializers. But would be nice to have proper solution.

---

_Comment by @MichaReiser on 2024-10-26 08:23_

I'm not familiar with any of the frameworks that you mentioned in the summary and not knowing `Book`s definition makes it a bit hard to follow the example. Could you explain me where the code depends on runtime type annotations and how Book'` defined (is it just a django model or is there more?)

---

_Comment by @last-partizan on 2024-10-26 16:11_

Yes, `Book` is just a Django model. It's not really important, can be anything.

`djangorestframework` is used to define API/serializers, and `drf_spectacular` is used to build `schema.yml` from it.

And for `name` field, it uses `get_type_hints` from `typing` module, to know what it returns.

```python
# Partial traceback
    return append_meta(self._map_response_type_hint(method), meta)
# ...   .../site-packages/drf_spectacular/openapi.py:1119: in _map_response_type_hint
    hint = get_override(method, 'field') or get_type_hints(method).get('return')
```

I can make a full repo with reproduction, but it's actually boils down to this:

```python
from __future__ import annotations
from typing import TYPE_CHECKING, get_type_hints

if TYPE_CHECKING:
    # We're just using this as example, instead of 'from app.models import Book'
    from collections.abc import MutableMapping


class Serializer: ...


class SerializerMethodField: ...


class BookSerializer(Serializer):
    name = SerializerMethodField()

    def get_name(self, instance: MutableMapping) -> str: ...


# drf_spectacular calls get_type_hints on a method to get return type
get_type_hints(BookSerializer.get_name)
```

I hope that's enough, but if not, i can make full repo with example.

---

_Comment by @Avasam on 2024-10-27 09:14_

@MichaReiser `drf-spectacular` inspects runtime annotations to produce an OpenAPI spec schema (an API specification that describes the shape of an endpoint and the accepted/provided types) based on your Django Serializers (or a manually written spec string, but that's irrelevant to this issue).

I think what's being asked here is for a way to specify that all subclasses of a certain type have all their method annotations not be picked up by `TCH001`/`TCH002`/`TCH003`. That way one could say that if the type is used as an annotation in a `Serializer` subclass, to not remove it.

Even without multifile analysis this could be beneficial since Django Serializers are often all grouped in a single module (due to circular import issues).


---

_Comment by @MichaReiser on 2024-10-27 13:54_

@AlexWaygood would you mind taking a look at this. This is definitely beyond my Python knowledge 😅

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-10-27 14:09_

---

_Comment by @last-partizan on 2024-10-28 10:11_

> I think what's being asked here is for a way to specify that all subclasses of a certain type have all their method annotations not be picked up by TCH001/TCH002/TCH003. That way one could say that if the type is used as an annotation in a Serializer subclass, to not remove it.

Yes, that's exactly what's needed.

---

_Comment by @MichaReiser on 2024-10-28 12:40_

Somewhat related #13713

---

_Comment by @stiangrim on 2025-04-09 07:11_

We are struggling with this as well (Ruff telling me to move stuff into TYPE_CHECKING blocks, which breaks drf-spectacular schema). Any updates? 🙏 

---

_Comment by @johannesloibl on 2025-07-15 10:45_

This is sooo annoying...

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-07-15 10:46_

---

_Referenced in [jupyter-server/jupyter_server#1558](../../jupyter-server/jupyter_server/pulls/1558.md) on 2025-09-01 23:59_

---
