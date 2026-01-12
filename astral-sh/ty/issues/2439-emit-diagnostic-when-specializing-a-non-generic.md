```yaml
number: 2439
title: Emit diagnostic when specializing a non-generic class
type: issue
state: open
author: tamireinhorn
labels:
  - diagnostics
  - generics
assignees: []
created_at: 2026-01-11T03:56:05Z
updated_at: 2026-01-12T09:35:18Z
url: https://github.com/astral-sh/ty/issues/2439
synced_at: 2026-01-12T09:56:38Z
```

# Emit diagnostic when specializing a non-generic class

---

_Issue opened by @tamireinhorn on 2026-01-11 03:56_

I wrote a function like:
```py
def fetch_user_countries_count(
    source: str, source_ids: list[int]
) -> sa.Sequence[sa.RowMapping[str, Any]]:
    user_locations = user_author_locations_cte(source=source, source_ids=source_ids)
    stmt = select_countries_with_author_count(user_locations)
    with SESSION() as sess:
        return sess.execute(stmt).mappings().all()
```

And yet, when I call it anywhere else, I get:
```py
def fetch_user_countries_count(
    source: str,
    source_ids: list[int]
) -> @Todo
```

I don't understand what else I should be doing. I tried changing the inner types but to no avail. I saw nothing mentioning said Todo type in the docs as well.

---

_Comment by @AlexWaygood on 2026-01-11 14:16_

Hey, thanks for the report, and sorry for the confusing UX :-)

The `@Todo` type doesn't mean that you've done anything wrong -- it means that there's some missing logic in ty itself and we don't know how to infer a good type here. To know exactly what the `Todo` is about here, we'll need a way to reproduce this -- how is the variable `sa` defined in your code? Is that a third-party module?

---

_Label `question` added by @AlexWaygood on 2026-01-11 14:16_

---

_Label `needs-info` added by @AlexWaygood on 2026-01-11 17:44_

---

_Comment by @tamireinhorn on 2026-01-11 18:21_

> Hey, thanks for the report, and sorry for the confusing UX :-)
> 
> The `@Todo` type doesn't mean that you've done anything wrong -- it means that there's some missing logic in ty itself and we don't know how to infer a good type here. To know exactly what the `Todo` is about here, we'll need a way to reproduce this -- how is the variable `sa` defined in your code? Is that a third-party module?


yeah, SA is defined as `import sqlalchemy as sa`.



---

_Comment by @dhruvmanila on 2026-01-12 05:47_

In debug build, it gives me the `@Todo(specialized non-generic class)` type:

```py
import typing as t

import sqlalchemy as sa

def _(x: sa.Sequence[sa.RowMapping[str, t.Any]]):
    # revealed: @Todo(specialized non-generic class)
    reveal_type(x)
```

mypy and Pyright both error on the annotation:
```
mypy: "Sequence" expects no type arguments, but 1 given  [type-arg]
mypy: "RowMapping" expects no type arguments, but 2 given  [type-arg]
Pyright: Expected no type arguments for class "RowMapping" [reportInvalidTypeArguments]
```

ty doesn't raise this error and this suggests that these classes is non-generic, so I think we just need to raise a diagnostic and infer `Unknown` for these cases. In the code, it's marked as TODO:

https://github.com/astral-sh/ruff/blob/09ff3e705639e4773f185994744390fdeb518e8e/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L730-L732

https://github.com/astral-sh/ruff/blob/09ff3e705639e4773f185994744390fdeb518e8e/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L1066-L1068

I don't see any open issues for this so we can use this.

---

_Renamed from "@Todo type" to "Emit diagnostic when specializing a non-generic class" by @dhruvmanila on 2026-01-12 05:47_

---

_Label `question` removed by @dhruvmanila on 2026-01-12 05:47_

---

_Label `needs-info` removed by @dhruvmanila on 2026-01-12 05:47_

---

_Label `generics` added by @dhruvmanila on 2026-01-12 05:48_

---

_Label `diagnostics` added by @dhruvmanila on 2026-01-12 05:48_

---

_Added to milestone `Stable` by @dhruvmanila on 2026-01-12 05:48_

---

_Comment by @tamireinhorn on 2026-01-12 06:56_

> In debug build, it gives me the `@Todo(specialized non-generic class)` type:
> 
> ```py
> import typing as t
> 
> import sqlalchemy as sa
> 
> def _(x: sa.Sequence[sa.RowMapping[str, t.Any]]):
>     # revealed: @Todo(specialized non-generic class)
>     reveal_type(x)
> ```
> 
> mypy and Pyright both error on the annotation:
> ```
> mypy: "Sequence" expects no type arguments, but 1 given  [type-arg]
> mypy: "RowMapping" expects no type arguments, but 2 given  [type-arg]
> Pyright: Expected no type arguments for class "RowMapping" [reportInvalidTypeArguments]
> ```
> 
> ty doesn't raise this error and this suggests that these classes is non-generic, so I think we just need to raise a diagnostic and infer `Unknown` for these cases. In the code, it's marked as TODO:
> 
> https://github.com/astral-sh/ruff/blob/09ff3e705639e4773f185994744390fdeb518e8e/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L730-L732
> 
> https://github.com/astral-sh/ruff/blob/09ff3e705639e4773f185994744390fdeb518e8e/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs#L1066-L1068
> 
> I don't see any open issues for this so we can use this.

I forgot to mention, but on later testing, I removed the wrong type arguments for RowMapping, and ty still spat out the same type. I put them in just to check what happened and did not remove them from the example posted here. Can edit if preferred.


---

_Comment by @dhruvmanila on 2026-01-12 09:34_

> I forgot to mention, but on later testing, I removed the wrong type arguments for RowMapping, and ty still spat out the same type. I put them in just to check what happened and did not remove them from the example posted here. Can edit if preferred.

Yeah, I suspect that's because `sa.Sequence` itself is a non-generic class (as highlighted by mypy)?

---
