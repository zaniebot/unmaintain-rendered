```yaml
number: 2439
title: "@Todo type"
type: issue
state: open
author: tamireinhorn
labels:
  - question
  - needs-info
assignees: []
created_at: 2026-01-11T03:56:05Z
updated_at: 2026-01-11T18:21:35Z
url: https://github.com/astral-sh/ty/issues/2439
synced_at: 2026-01-12T02:26:12Z
```

# @Todo type

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
