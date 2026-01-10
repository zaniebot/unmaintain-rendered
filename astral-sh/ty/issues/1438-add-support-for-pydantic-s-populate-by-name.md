---
number: 1438
title: "Add support for Pydantic's `populate_by_name`, `validate_by_name`, etc."
type: issue
state: open
author: moredatarequired
labels:
  - library
assignees: []
created_at: 2025-10-27T01:12:36Z
updated_at: 2026-01-09T03:12:03Z
url: https://github.com/astral-sh/ty/issues/1438
synced_at: 2026-01-10T01:48:23Z
---

# Add support for Pydantic's `populate_by_name`, `validate_by_name`, etc.

---

_Issue opened by @moredatarequired on 2025-10-27 01:12_

### Summary

Prior to 0.0.1a24 Pydantic models using aliases and either `populate_by_name` or `validate_by_name` were correctly assessed by ty when passed the field name (rather than the alias name).

Repro:
```python
from pydantic import BaseModel, ConfigDict, Field

class A(BaseModel):
    model_config = ConfigDict(validate_by_name=True)
    a: int = Field(alias="x")

a = A(a=5)
```

previously `ty` understood that `a` was a valid keyword argument.
```
❯ uvx --with 'pydantic==2.12.3' --with 'ty==0.0.1a23' ty check a.py
Checking ------------------------------------------------------------ 1/1 files                                                                                                                          All checks passed!
```

now it no longer does:
```
❯ uvx --with 'pydantic==2.12.3' --with 'ty==0.0.1a24' ty check a.py
Checking ------------------------------------------------------------ 1/1 files
error[missing-argument]: No argument provided for required parameter `x`
 --> a.py:7:5
  |
5 |     a: int = Field(alias="x")
6 |
7 | a = A(a=5)
  |     ^^^^^^
8 | print(a.model_dump())
  |
info: rule `missing-argument` is enabled by default

error[unknown-argument]: Argument `a` does not match any known parameter
 --> a.py:7:7
  |
5 |     a: int = Field(alias="x")
6 |
7 | a = A(a=5)
  |       ^^^
8 | print(a.model_dump())
  |
info: rule `unknown-argument` is enabled by default

Found 2 diagnostics
```

### Version

_No response_

---

_Label `dataclasses` added by @AlexWaygood on 2025-10-27 06:42_

---

_Comment by @Viicos on 2025-10-27 07:33_

`populate_by_name`/`validate_by_name` was never understood by ty, but instead aliases where _not_ supported until https://github.com/astral-sh/ruff/pull/20961, problaby leading to this behavior.

---

_Comment by @sharkdp on 2025-10-27 12:42_

> `populate_by_name`/`validate_by_name` was never understood by ty, but instead aliases where _not_ supported until [astral-sh/ruff#20961](https://github.com/astral-sh/ruff/pull/20961), problaby leading to this behavior.

Exactly, thank you for the explanation.

It looks like solving this would require ty to add dedicated pydantic support that would include "parsing" the `model_config` and then adding/removing the alias and/or the field name as keyword parameters to `__init__`, depending on settings like `validate_by_name`, `validate_by_alias`, etc.

---

_Label `library` added by @sharkdp on 2025-10-27 12:42_

---

_Renamed from "0.0.1a24 regression with Pydantic models using populate_by_name=True" to "Add support for Pydantic's `populate_by_name`, `validate_by_name`, etc." by @sharkdp on 2025-10-27 12:44_

---

_Label `dataclasses` removed by @AlexWaygood on 2025-10-27 13:00_

---

_Added to milestone `Stable` by @carljm on 2026-01-09 03:12_

---
