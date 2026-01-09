---
number: 5639
title: RUF012 warns for subclasses of Pydantic classes
type: issue
state: open
author: roikoren755
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-07-10T10:19:56Z
updated_at: 2025-11-25T20:01:29Z
url: https://github.com/astral-sh/ruff/issues/5639
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF012 warns for subclasses of Pydantic classes

---

_Issue opened by @roikoren755 on 2023-07-10 10:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
First of all, once again, I'd like to thank you for a great library.

The recently added RUF012 rule warns with false positives for classes that extend a Pydantic model.
```python
from pydantic import BaseModel


class A(BaseModel):
    lst = []


class B(A):
    lst2 = []  # RUF012

```

Ruff version: `ruff 0.0.277`

---

_Label `bug` added by @charliermarsh on 2023-07-10 13:40_

---

_Referenced in [astral-sh/ruff#5273](../../astral-sh/ruff/pulls/5273.md) on 2023-10-25 08:43_

---

_Comment by @nhtgl on 2023-12-02 15:34_

Hi @charliermarsh 👋 Any eta on this issue?

---

_Comment by @mikeleppane on 2024-01-30 06:51_

I think this can be closed? At least the given example cannot be reproduced unless there's something more to it.

---

_Comment by @danieleades on 2024-02-27 12:38_

> I think this can be closed? At least the given example cannot be reproduced unless there's something more to it.

the comment on the linked pull request suggests the fix only works for direct inheritance, and not transitive inheritance

---

_Comment by @henrybetts on 2024-03-22 13:15_

Perhaps it would be worth having an option to specify the parent classes to ignore when applying this rule?

For example, I'm using [Beanie](https://beanie-odm.dev) and running into a similar issue whereby this rule should be ignored for beanie.Document subclasses.

Something like;
```
[tool.ruff.lint.ruff]
ignore-ruf012-classes = ["pydantic.BaseModel", "beanie.Document"]
```

---

_Comment by @JonathanPlasse on 2024-04-21 16:41_

Should the tag `multifile-analysis` be added, as base classes could be in other files?

---

_Label `multifile-analysis` added by @MichaReiser on 2024-04-21 20:08_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:32_

---

_Referenced in [astral-sh/ruff#13630](../../astral-sh/ruff/issues/13630.md) on 2025-02-07 17:04_

---

_Referenced in [astral-sh/ruff#16174](../../astral-sh/ruff/issues/16174.md) on 2025-02-15 03:11_

---

_Comment by @gstrauss-zscaler on 2025-10-16 07:53_

Hey, is this issue still hasn't been resolved?

---

_Comment by @MichaReiser on 2025-10-16 08:13_

Yeah sorry. Adding accurate mulitifile analysis isn't a small undertaking. We plan to bring it to Ruff, but it might be quite some time before it completes.

---

_Comment by @takeda on 2025-11-25 19:53_

I think the code still isn't acceptable even with pydantic, shouldn't:

```py
class A(BaseModel):
    lst = []
```

be

```py
class A(BaseModel):
    lst = Field(default_factory=list)
```

?

Am I missing something?

Edit: ok, I found the answer: https://docs.pydantic.dev/latest/concepts/fields/#validate-default-values looks like pydantic added a workaround for a common bug, but this workaround became standard. I still think deepcopy which pydantic is doing will be slower than creating structure from scratch.

---
