```yaml
number: 20945
title: "`FAST001` false positive for undefined names"
type: issue
state: open
author: dylwil3
labels:
  - bug
  - rule
assignees: []
created_at: 2025-10-17T18:28:26Z
updated_at: 2025-10-22T17:23:48Z
url: https://github.com/astral-sh/ruff/issues/20945
synced_at: 2026-01-10T11:10:00Z
```

# `FAST001` false positive for undefined names

---

_Issue opened by @dylwil3 on 2025-10-17 18:28_

### Summary

We give a lint here for FAST001 here when probably we should abort because neither `Item` nor `Foo` are defined. 

Example:

```python
from __future__ import annotations
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

@app.post("/items/", response_model=Foo) # false positive FAST001
async def create_item(item: Item) -> Item:
    return item
```

[Playground](https://play.ruff.rs/edd92ea7-8e60-4d36-98e3-4577f00d7e0b)

This is because we check that two resolved names are equal - but `.resolve_name` returns an `Option` and in this case resolving both `Foo` and `Item` gives `None`:

https://github.com/astral-sh/ruff/blob/6e7ff0706564cb938c154b5a5f9993c1118085e0/crates/ruff_linter/src/rules/fastapi/rules/fastapi_redundant_response_model.rs#L124-L125

### Version

_No response_

---

_Label `bug` added by @dylwil3 on 2025-10-17 18:28_

---

_Label `rule` added by @dylwil3 on 2025-10-17 18:28_

---

_Comment by @11happy on 2025-10-22 17:23_

Hii @dylwil3  
Does this approach look correct?

```
if let (Some(response_model_id), Some(return_value_id)) = (
    semantic.resolve_name(response_mode_name_expr),
    semantic.resolve_name(return_value_name_expr),
) {
    return response_model_id == return_value_id;
}
return false;

```
only returns true if both names resolve successfully ,. If either is undefined, it returns false

---
