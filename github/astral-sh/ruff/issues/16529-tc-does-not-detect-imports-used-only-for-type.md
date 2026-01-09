---
number: 16529
title: TC does not detect imports used only for type annotations
type: issue
state: closed
author: devon-research
labels:
  - question
assignees: []
created_at: 2025-03-06T08:12:12Z
updated_at: 2025-03-06T09:45:45Z
url: https://github.com/astral-sh/ruff/issues/16529
synced_at: 2026-01-07T13:12:16-06:00
---

# TC does not detect imports used only for type annotations

---

_Issue opened by @devon-research on 2025-03-06 08:12_

### Summary

It seems that some of the `TC` rules are not working correctly. For example, if I have this file:

```python
from typing import NamedTuple

t: NamedTuple | None = None
```

and I run

```bash
ruff check --isolated --select=TC
```

then I get that all checks pass.

My understanding is that this should yield a `TC003` error. Am I misunderstanding the intended behavior?

### Version

0.9.4

---

_Comment by @MichaReiser on 2025-03-06 08:29_

Types imported from the `typing` module are excempt from moving into `TYPE_CHECKING` by default because you still have to import the `typing` module to get the `TYPE_CHECKING` variable at runtime. So the benefit of moving types imported from `typing` is marginal. See Charlie's response here https://github.com/astral-sh/ruff/issues/9197#issuecomment-1863808227


However, you can remove `typing` from the `excempt-module` list and ruff will start enforcing moving `typing` imports in the `TYPE_CHECKING` block

```toml
[lint]
select = [ "TC" ]

[lint.flake8-type-checking]
exempt-modules = []
```

See https://play.ruff.rs/9ccf9a06-534c-441a-8101-02056076b8a6

---

_Label `question` added by @MichaReiser on 2025-03-06 08:29_

---

_Comment by @devon-research on 2025-03-06 09:45_

Ah, I see. Thanks!

I additionally had this confusion come up outside the `typing` module, but I realize now from the issue you linked that the behavior I was seeing is explained by the `strict = false` default.

---

_Closed by @devon-research on 2025-03-06 09:45_

---
