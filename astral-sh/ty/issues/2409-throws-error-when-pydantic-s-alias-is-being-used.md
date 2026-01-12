```yaml
number: 2409
title: "Throws error when pydantic's alias is being used"
type: issue
state: closed
author: viliusgudziunas
labels: []
assignees: []
created_at: 2026-01-09T08:56:28Z
updated_at: 2026-01-09T09:03:35Z
url: https://github.com/astral-sh/ty/issues/2409
synced_at: 2026-01-12T15:54:26Z
```

# Throws error when pydantic's alias is being used

---

_@viliusgudziunas_

### Summary

```
# pyproject.toml

pydantic = "^2.3.0"
ty = "^0.0.10"
```

`ty` seems to break when` pydantic` `alias` is being used

Example code:

```
from typing import Literal
from pydantic import BaseModel

class LabelConfig(BaseModel):
    background_color: Literal["000000"] | None = Field(None, alias="backgroundColor")
    text_color: Literal["000000"] | None = Field(None, alias="textColor")


c = LabelConfig(background_color="000000", text_color="000000")
```

Running `ty check` I get the following errors

```
error[unknown-argument]: Argument `background_color` does not match any known parameter
   --> src...:426:17
    |
426 | c = LabelConfig(background_color="000000", text_color="000000")
    |                 ^^^^^^^^^^^^^^^^^^^^^^^^^
    |
info: rule `unknown-argument` is enabled by default

error[unknown-argument]: Argument `text_color` does not match any known parameter
   --> src...:426:44
    |
426 | c = LabelConfig(background_color="000000", text_color="000000")
    |                                            ^^^^^^^^^^^^^^^^^^^
    |
info: rule `unknown-argument` is enabled by default
```

It seems to want me to always use the alias, but if I understand pydantic's docs, then alias is just an alternative name for the field.

### Version

ty 0.0.10 (d18902cdc 2026-01-07)

---

_Comment by @MichaReiser on 2026-01-09 09:03_

Thank you 

---

_Closed by @MichaReiser on 2026-01-09 09:03_

---
