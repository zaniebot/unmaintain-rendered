```yaml
number: 2530
title: "Support `respect-type-ignore-comments` in file overrides"
type: issue
state: closed
author: strokirk
labels:
  - configuration
  - suppression
assignees: []
created_at: 2026-01-16T08:45:32Z
updated_at: 2026-01-19T08:09:44Z
url: https://github.com/astral-sh/ty/issues/2530
synced_at: 2026-01-19T08:24:55Z
```

# Support `respect-type-ignore-comments` in file overrides

---

_@strokirk_

Hello!

https://github.com/astral-sh/ty/issues/1422 and https://github.com/astral-sh/ruff/pull/22137, which added `respect-type-ignore-comments` as a global configuration option, have been very useful for me to avoid warnings in ty where it doesn't agree with mypy. I'd love if that could be applied on a file-by-file basis using overrides. 

A common example is Pydantic's `@computed_field` decorator, which requires `# type: ignore[prop-decorator]` for mypy but doesn't need any ignore comment for ty.

Since a lot of those models are usually defined in a single file, or file matching a common pattern, an override would be super useful.

## Example

```python
from pydantic import BaseModel, computed_field

class MyModel(BaseModel):
    @computed_field  # type: ignore[prop-decorator]
    @property
    def my_prop(self) -> dict:
        ...
```

Thanks for a great tool!

---

_Label `configuration` added by @AlexWaygood on 2026-01-16 08:46_

---

_Label `suppression` added by @AlexWaygood on 2026-01-16 08:46_

---

_Comment by @MichaReiser on 2026-01-16 08:51_

Thanks for opening this issue. This shouldn't be too hard to add. We only waited with adding support for it because we first wanted to hear from users what use cases they have for it. Now we know! Thank you

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-16 08:51_

---

_Closed by @MichaReiser on 2026-01-19 08:09_

---
