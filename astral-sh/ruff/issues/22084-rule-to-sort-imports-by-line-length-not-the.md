```yaml
number: 22084
title: Rule to sort imports by line length, not the module name length
type: issue
state: closed
author: kuyugama
labels:
  - question
  - isort
assignees: []
created_at: 2025-12-19T14:12:00Z
updated_at: 2025-12-19T16:52:49Z
url: https://github.com/astral-sh/ruff/issues/22084
synced_at: 2026-01-12T15:54:58Z
```

# Rule to sort imports by line length, not the module name length

---

_@kuyugama_

### Question

Ruff has two lint options to sort imports by length `length-sort` and `length-sort-straight`, but they are sorting imports by module name (e.g. `os.path`). This doesn't work well for `from` imports that may import multiple items at one line, so this line may be bigger than `ruff` may assume and this will break code beautifulness.

Here is the example:

I have such imports:

```python
from fundi.resolve import resolve
from fundi.logging import get_logger
from fundi.types import CacheKey, CallableInfo
from fundi.util import call_sync, call_async, add_injection_trace
```

And ruff makes them look completely reversed:

```python
from fundi.util import call_sync, call_async, add_injection_trace
from fundi.types import CacheKey, CallableInfo
from fundi.logging import get_logger
from fundi.resolve import resolve
```

Although, other imports in the same module are perfectly ordered:
```python
import typing
import contextlib
import collections.abc
```

### Version

0.14.10

---

_Label `question` added by @kuyugama on 2025-12-19 14:12_

---

_Comment by @ntBre on 2025-12-19 16:52_

Thanks for the report! I think this is a duplicate of https://github.com/astral-sh/ruff/issues/9870 and the related https://github.com/astral-sh/ruff/issues/14136.

---

_Closed by @ntBre on 2025-12-19 16:52_

---

_Label `isort` added by @ntBre on 2025-12-19 16:52_

---
