```yaml
number: 2497
title: Spaces/tabs being rendered as HTML code in Markdown code blocks
type: issue
state: open
author: gabriel-abn
labels:
  - server
assignees: []
created_at: 2026-01-14T17:09:41Z
updated_at: 2026-01-14T18:30:45Z
url: https://github.com/astral-sh/ty/issues/2497
synced_at: 2026-01-14T18:47:49Z
```

# Spaces/tabs being rendered as HTML code in Markdown code blocks

---

_@gabriel-abn_

### Summary

hi guys

markdown code blocks rendering good but still with some invalid characters

<img width="1028" height="749" alt="Image" src="https://github.com/user-attachments/assets/73572085-9f74-43a1-a0ca-95f370237bbf" />

these html codes are my tabs... if i remove them, all codes disappear

strings also being rendered with a "\\" before all underscores

just updated `ty` before trying creating the issue and the error persists

the example from the image:

```python
class IProductsRepository[T: Product](Protocol):
	async def get(self, id: int) -> T | None: ...

	class _SearchFilterOptions(TypedDict, total=False):
		category: Literal["name", "acronym", "exhibition_name"]
		name: str
		visible: bool
		sellable: bool
		product_type_id: int
		product_list: list[int]
		page: int
		page_limit: int

	async def search(self, options: _SearchFilterOptions) -> list[T]:
		"""
		Search/list products based on the provided filter options.

		Supports both specific search (by category and name) and general listing with filters.
		If category and name are provided, performs specific search.
		Otherwise, lists all products matching the filter criteria.

		### Args:
		- options (_SearchFilterOptions): The search filter options:
		```python
		{
			"category": "name" | "acronym" | "exhibition_name" # (optional for listing),
			"name": "Product Name" # (optional for listing, required if category is provided),
			"visible": True | False,
			"sellable": True | False,
			"product_type_id": int,
			"product_list": list[int],
			"page": int,
			"page_limit": int,
		}
		```

		### Returns:
			list[T]: A list of products that match the search/filter criteria.
		"""
		...

```

### Version

0.0.11

---

_Renamed from "Spaces/tabs being rendered as HTML code" to "Spaces/tabs being rendered as HTML code in Markdown code blocks" by @gabriel-abn on 2026-01-14 17:11_

---

_Comment by @MichaReiser on 2026-01-14 17:39_

Would you mind pasting the code as text so that I don't have to type it from your screenshot 😅

---

_Comment by @gabriel-abn on 2026-01-14 18:02_

> Would you mind pasting the code as text so that I don't have to type it from your screenshot 😅

just updated the description haha sorry

---

_Comment by @MichaReiser on 2026-01-14 18:19_

Awesome, thank you. Very much appreciated

---

_Label `server` added by @AlexWaygood on 2026-01-14 18:30_

---
