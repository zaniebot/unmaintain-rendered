---
number: 2133
title: "pydantic BaseModel with aliases triggers \"No argument provided for required parameter\""
type: issue
state: closed
author: masc-it
labels: []
assignees: []
created_at: 2025-12-20T19:20:22Z
updated_at: 2025-12-20T22:59:25Z
url: https://github.com/astral-sh/ty/issues/2133
synced_at: 2026-01-10T01:52:52Z
---

# pydantic BaseModel with aliases triggers "No argument provided for required parameter"

---

_Issue opened by @masc-it on 2025-12-20 19:20_

### Summary

Let's say I have a basemodel like this one:

```python
class PostInput(BaseModel):
    title: str
    slug: str
    markdown_content: str = Field(alias="markdownContent")
    category_slug: str | None = Field(default=None, alias="categorySlug")
    category_id: int | None = Field(default=None, alias="categoryId")
```

then if I try to instantiate it, by using the base model param names (the snake case ones) I get: "No argument provided for required parameter `markdownContent`"

but that's just arbitrary. If I try to fix it, by using the camelcase alias, I get the same error.


### Version

0.0.3

---

_Comment by @sharkdp on 2025-12-20 22:59_

Thank you for reporting this.

This is probably the same as https://github.com/astral-sh/ty/issues/1159

---

_Closed by @sharkdp on 2025-12-20 22:59_

---
