---
number: 21132
title: "`module-import-not-at-top-of-file` (`E402`) - false negative on `if TYPE_CHECKING` imports"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-30T04:00:46Z
updated_at: 2025-10-30T16:50:57Z
url: https://github.com/astral-sh/ruff/issues/21132
synced_at: 2026-01-07T13:12:16-06:00
---

# `module-import-not-at-top-of-file` (`E402`) - false negative on `if TYPE_CHECKING` imports

---

_Issue opened by @DetachHead on 2025-10-30 04:00_

> It was unexpected to me, that, e.g., the following order is not flagged by isort (or any other rule like TCH or E402):
> 
> 
> 
> ```python
> 
> import os
> 
> from typing import TYPE_CHECKING
> 
> 
> 
> if TYPE_CHECKING:
> 
>     from my_package import my_function
> 
> 
> 
> import pydantic
> 
> 
> 
> import my_package
> 
> ``` 

 _Originally posted by @kdebrab in [#13976](https://github.com/astral-sh/ruff/issues/13976#issuecomment-2449187735)_

(opening a separate issue for this since the original issue is specifically for the isort import order)

[playground](https://play.ruff.rs/d67f1f6a-3d99-4459-8555-4f44a411c09c)

---

_Comment by @ntBre on 2025-10-30 14:05_

This does make some sense to me as a better fit than adding it to the isort rule. One downside with E402 in particular is that it doesn't have an autofix, though.

---

_Label `rule` added by @ntBre on 2025-10-30 14:05_

---

_Label `needs-decision` added by @ntBre on 2025-10-30 14:05_

---

_Comment by @Avasam on 2025-10-30 16:50_

I'll also quote my own comment under the same issue: https://github.com/astral-sh/ruff/issues/13976#issuecomment-2453504019
> Looking at this from a different angle: Could this be related to `E402` instead ? "Module level import not at top of file" could trigger for imports found after top-level `if TYPE_CHECKING` blocks.



---
