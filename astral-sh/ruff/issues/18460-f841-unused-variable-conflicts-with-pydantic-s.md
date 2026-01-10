```yaml
number: 18460
title: "F841: `unused-variable` conflicts with Pydantic's ForwardRef"
type: issue
state: closed
author: Konfus-ius
labels:
  - question
assignees: []
created_at: 2025-06-04T13:32:53Z
updated_at: 2025-07-24T12:02:53Z
url: https://github.com/astral-sh/ruff/issues/18460
synced_at: 2026-01-10T11:09:58Z
```

# F841: `unused-variable` conflicts with Pydantic's ForwardRef

---

_Issue opened by @Konfus-ius on 2025-06-04 13:32_

### Summary

Taking the following class definition

```python
from pydantic import BaseModel, ConfigDict, field_validator

class ClassName(BaseModel):
    output: ForwardRef("unused_variable")

    @classmethod
    def initialize(cls, db_repository: DatabaseRepository) -> "WhatEver":
        unused_variable = List[Literal[*output]] 
        cls.model_rebuild()
        return cls
```
ruff check shows a finding for F841 unused variable. This is theroretically correct but actually wrong. As the pydantic ForwardRef makes use of the `unused_variable`.

Request:
 - Either "flexibilize" F841 such that pydantic ForwardRef is properly taken into account or
 - add rule to enforce getting rid of ForwardRef within pydantic (if wanted)

Thanks for guidance


---

_Comment by @Viicos on 2025-06-04 14:38_

Not a Ruff maintainer but I don't think special casing this is necessary. `model_rebuild()` fetches the current namespace where it is called to resolve forward annotations, which is really uncommon behavior (and only works with some Python implementations). Instead, we recommend explicitly providing the rebuild namespace with `model_rebuild(_types_namespace={'unused_variable': unused_variable})`.

---

_Label `question` added by @ntBre on 2025-06-04 17:51_

---

_Comment by @Konfus-ius on 2025-06-06 08:10_

Hi @Viicos 

thanks for the feedback, we will check this approach. In best case, we get fully rid of the ForwardRefs ...

But your approach is much better and is in line with linting.

---

_Closed by @MichaReiser on 2025-07-24 12:02_

---
