---
number: 15624
title: "`PLW3201` false positive for Pydantic dunders like `__get_pydantic_core_schema__`"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
assignees: []
created_at: 2025-01-20T23:04:57Z
updated_at: 2025-01-21T07:28:56Z
url: https://github.com/astral-sh/ruff/issues/15624
synced_at: 2026-01-07T13:12:16-06:00
---

# `PLW3201` false positive for Pydantic dunders like `__get_pydantic_core_schema__`

---

_Issue opened by @jamesbraza on 2025-01-20 23:04_

With the following dependencies and Python 3.12.7:

```none
pydantic                  2.10.1
pydantic-core             2.27.1
pylint                    3.3.3
pylint-plugin-utils       0.8.2
pylint-pydantic           0.3.5
ruff                      0.9.2
```

Running Ruff on the snippet from https://docs.pydantic.dev/latest/concepts/types/#as-a-method-on-a-custom-type

```python
from collections.abc import Callable
from dataclasses import dataclass
from typing import Any

from pydantic import GetCoreSchemaHandler
from pydantic_core import CoreSchema, core_schema


@dataclass(frozen=True)
class MyAfterValidator:
    func: Callable[[Any], Any]

    def __get_pydantic_core_schema__(
        self, source_type: Any, handler: GetCoreSchemaHandler
    ) -> CoreSchema:
        return core_schema.no_info_after_validator_function(
            self.func, handler(source_type)
        )
```

I get the following `PLW3201` error:

```
a.py: PLW3201 Dunder method `__get_pydantic_core_schema__` has no special meaning in Python 3
```

I know I can allowlist this name via Ruff configuration:

```toml
[tool.ruff.lint.pylint]
allow-dunder-method-names = ["__get_pydantic_core_schema__"]
```

However, I guess the question is similar to https://github.com/astral-sh/ruff/issues/9716, but instead of built-ins now for Pydantic. Should Pydantic dunders be ignored by default?

---

_Label `rule` added by @MichaReiser on 2025-01-21 07:20_

---

_Comment by @MichaReiser on 2025-01-21 07:28_

Thanks for the detailed write up and linking to the other relevant issue. 

Adding external libraries is always a slippery slope because once you add one, you have to add all others. But we have made exceptions for libraries that can be considered community standards as which pydantic qualifies. 

But I think it ultimately comes back to what Alex said in his [comment](https://github.com/astral-sh/ruff/issues/9716#issuecomment-1917855645) and if allowing any non-stdlib dunder is in the indent of the rule, because the rule very explicitly states that it checks for dunder methods that have no special meaning in Python 3, which is true for all pydantic dunders (it's the framework that adds the special meaning). 

Either way, I'll merge this with https://github.com/astral-sh/ruff/issues/9716#issuecomment-1917855645 as it's basically the same question but for pydandic instead of rich. 

---

_Closed by @MichaReiser on 2025-01-21 07:28_

---
