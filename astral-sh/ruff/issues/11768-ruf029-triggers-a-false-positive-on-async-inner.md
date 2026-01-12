```yaml
number: 11768
title: RUF029 triggers a false positive on async inner function
type: issue
state: closed
author: garyj
labels:
  - question
assignees: []
created_at: 2024-06-06T00:35:58Z
updated_at: 2024-06-06T13:09:57Z
url: https://github.com/astral-sh/ruff/issues/11768
synced_at: 2026-01-12T15:54:51Z
```

# RUF029 triggers a false positive on async inner function

---

_@garyj_

Hi Team,

Similar to #11043 and #11218, I am getting a false positive when using an async inner function. Code snippet below (from Django project). Might be somewhat convoluted case, but I thought it's worth reporting. 

```python
async def export_as_json(queryset: QuerySet, fields: list[str] | None = None) -> StreamingHttpResponse: # RUF029 Triggers here
    async def json_generator(queryset: QuerySet) -> AsyncGenerator:
        yield '['
        first = True
        async for product in queryset.aiterator():
            if not first:
                yield ','
            else:
                first = False
            yield json.dumps(await serialize_product(product, fields))
        yield ']'

    response = StreamingHttpResponse(json_generator(queryset), content_type='application/json')
    response['Content-Disposition'] = 'attachment; filename="feed.json"'
    return response
```

---

_Label `question` added by @charliermarsh on 2024-06-06 04:00_

---

_Comment by @charliermarsh on 2024-06-06 04:02_

Does the outer function need to be `async` though? It returns an `async` generator, but doesn't actually `await` it. Wouldn't some caller eventually need to be `async` to `await` it?

---

_Comment by @garyj on 2024-06-06 08:30_

Thank you Charlie, you are 100% right, the outer function does not need to be `async`. 

Apology for the noise. 

---

_Closed by @charliermarsh on 2024-06-06 13:09_

---

_Comment by @charliermarsh on 2024-06-06 13:09_

No problem!

---
