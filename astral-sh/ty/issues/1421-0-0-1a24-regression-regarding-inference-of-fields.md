```yaml
number: 1421
title: 0.0.1a24 regression regarding inference of fields for pydantic models
type: issue
state: closed
author: theelderbeever
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-10-23T15:53:24Z
updated_at: 2025-10-27T16:07:24Z
url: https://github.com/astral-sh/ty/issues/1421
synced_at: 2026-01-10T02:06:25Z
```

# 0.0.1a24 regression regarding inference of fields for pydantic models

---

_Issue opened by @theelderbeever on 2025-10-23 15:53_

### Summary

Using `ty` as the LSP in Zed I now see this error for all of my `pydantic` models. This doesn't happen in the previous version.

> No arguments provided for required parameters `__pydantic_extra__`, `__pydantic_fields_set__`, `__pydantic_private__` (ty missing-argument)

### Version

ty==0.0.1a24

### Example
```python
from pydantic import BaseModel


class Example(BaseModel):
    value: int


example = Example(value=1)
```

---

_Label `dataclasses` added by @AlexWaygood on 2025-10-23 17:19_

---

_Comment by @sharkdp on 2025-10-23 18:55_

Thank you for reporting this. Which version of Pydantic do you use?

---

_Label `bug` added by @sharkdp on 2025-10-23 18:55_

---

_Renamed from "`ty==0.0.1a24` regression with required arguments analysis" to "0.0.1a24 regression regarding inference of fields for pydantic models" by @AlexWaygood on 2025-10-23 19:03_

---

_Comment by @theelderbeever on 2025-10-23 19:18_

@sharkdp 

```bash
❯ uv pip freeze | grep pydantic
pydantic==2.5.3
pydantic-core==2.14.6
pydantic-settings==2.2.1
```

---

_Comment by @sharkdp on 2025-10-27 12:16_

Oh, interesting. I was testing with a more recent version. I can indeed confirm that this fails with 2.5.3, but it works for pydantic 2.6.0 and later.

2.6.0 includes [this bugfix](https://github.com/pydantic/pydantic/pull/8163) to pydantic's `dataclass_transform` setup. I haven't completely tracked down if ty *should* work with this previous setup, but I'm not sure if it's worth investing time in, given that there seems to have been a bug in pydantic before that.

---

_Comment by @sharkdp on 2025-10-27 12:17_

(this looks like a regression in ty 0.0.1a24 because we didn't support "pydantic style" dataclass-transformers before that, at all)

---

_Comment by @theelderbeever on 2025-10-27 15:14_

Ah okay good find on the bug. Honestly I will just bump pydantic versions in my app.

---

_Comment by @sharkdp on 2025-10-27 16:07_

Ok, I'm going to close this for now. Anyone, feel free to re-open this if you think this is not a bug in pydantic, but rather in ty.

---

_Closed by @sharkdp on 2025-10-27 16:07_

---
