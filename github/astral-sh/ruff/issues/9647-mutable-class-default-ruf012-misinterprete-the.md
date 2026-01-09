---
number: 9647
title: mutable-class-default (RUF012) misinterprete the code block
type: issue
state: closed
author: ArazHeydarov
labels:
  - bug
assignees: []
created_at: 2024-01-26T14:09:58Z
updated_at: 2024-01-26T14:54:50Z
url: https://github.com/astral-sh/ruff/issues/9647
synced_at: 2026-01-07T13:12:15-06:00
---

# mutable-class-default (RUF012) misinterprete the code block

---

_Issue opened by @ArazHeydarov on 2024-01-26 14:09_

Hello, 

RUF012 working principle seems to be broken. I'm defining a Pydantic [json_encoder](https://docs.pydantic.dev/1.10/usage/exporting_models/#json_encoders) config however Ruff detects it as typing annotation and raises an error when I check. 

Code:
```Python
from bson import ObjectId
from pydantic import BaseConfig, BaseModel


class MongoModel(BaseModel):
    class Config(BaseConfig):
        allow_population_by_field_name = True
        json_encoders = {ObjectId: str}
```

Command: `ruff check .`

Result:
```
src/models/mongo_model.py:8:25: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
Found 1 error.
```

Current settings:
```toml
[tool.ruff.lint]
extend-select = ["I", "C90", 'N', 'UP', 'ASYNC', 'RUF']
```  

Current Ruff version:
`ruff 0.1.14`
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2024-01-26 14:11_

It looks like `BaseConfig` is missing from our allowlist.

---

_Label `bug` added by @charliermarsh on 2024-01-26 14:11_

---

_Comment by @charliermarsh on 2024-01-26 14:11_

What is `BaseConfig`? I can't find it in the Pydantic docs.

---

_Comment by @trag1c on 2024-01-26 14:27_

> What is `BaseConfig`? I can't find it in the Pydantic docs.

It's part of Pydantic v1: https://docs.pydantic.dev/2.0/api/config/#pydantic.config.BaseConfig

---

_Comment by @ArazHeydarov on 2024-01-26 14:28_

It's a class that is deprecated as of Pydantic 2.0. The result is the same without inheriting from that class.
I know that the Config class definition itself is deprecated. 

---

_Comment by @charliermarsh on 2024-01-26 14:28_

Thank you! Will add.

---

_Comment by @ArazHeydarov on 2024-01-26 14:31_

Thank you for your quick response. 

---

_Referenced in [astral-sh/ruff#9650](../../astral-sh/ruff/pulls/9650.md) on 2024-01-26 14:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 14:49_

---

_Closed by @charliermarsh on 2024-01-26 14:54_

---
