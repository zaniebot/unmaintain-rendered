```yaml
number: 12239
title: "lint isort I001: forbid multiple imports on single line?"
type: issue
state: open
author: fschulze
labels:
  - question
  - isort
assignees: []
created_at: 2024-07-08T08:28:16Z
updated_at: 2025-01-13T07:43:14Z
url: https://github.com/astral-sh/ruff/issues/12239
synced_at: 2026-01-12T15:54:51Z
```

# lint isort I001: forbid multiple imports on single line?

---

_@fschulze_

I tried to configure lint.isort to block multiple imports on single lines without enforcing the way multiple imports are split.

This should be reported as I001:
```python
from typing import Any, Callable, ContextManager
```

Both of these variants should not be reported:
```python
from typing import Any
from typing import Callable
from typing import ContextManager
```

```python
from typing import (
    Any,
    Callable,
    ContextManager,
)
```

The only setting which comes close is ``force-single-line=true``, but then only the first variant is allowed and the last variant is reported.

---

_Label `isort` added by @AlexWaygood on 2024-07-08 08:52_

---

_Comment by @MichaReiser on 2024-07-08 09:21_

This is interesting. Could you explain why you want to allow both variants?

The goal of `isort` is to have consistent import formatting. That's why it only allows exactly one variant.

---

_Comment by @fschulze on 2024-07-08 09:43_

For longer import lists the bracketed version is more readable, for a few imports the non-bracketed version is more concise without preventing nice diffs like the multiple imports on one line would.

---

_Label `question` added by @MichaReiser on 2025-01-13 07:43_

---
