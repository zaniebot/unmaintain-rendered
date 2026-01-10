```yaml
number: 842
title: Comments among import statements flagged as incorrect
type: issue
state: closed
author: brettcannon
labels:
  - question
assignees: []
created_at: 2022-11-20T23:44:44Z
updated_at: 2022-11-22T01:39:32Z
url: https://github.com/astral-sh/ruff/issues/842
synced_at: 2026-01-10T12:09:58Z
```

# Comments among import statements flagged as incorrect

---

_Issue opened by @brettcannon on 2022-11-20 23:44_

For instance, having a comment at the end of an import line gets flagged:

```python
from __future__ import annotations

import html
import html.parser
import urllib.parse
from typing import Any, Dict, List, Optional, Union

import packaging.specifiers
import packaging.utils
from typing_extensions import Literal, TypeAlias, TypedDict  # Python 3.8+ only.
```

---

_Comment by @brettcannon on 2022-11-20 23:46_

It also fails on the original version that Black and isort produce:
```python
from __future__ import annotations

import html
import html.parser
import urllib.parse
from typing import Any, Dict, List, Optional, Union

import packaging.specifiers
import packaging.utils

# Python 3.8+ only.
from typing_extensions import Literal, TypeAlias, TypedDict
```

---

_Comment by @charliermarsh on 2022-11-20 23:52_

@brettcannon -- Just trying to reproduce this - is there any chance you're on a version earlier than v0.0.122?

---

_Label `question` added by @charliermarsh on 2022-11-21 03:08_

---

_Comment by @brettcannon on 2022-11-21 23:55_

I just got the VS Code extension update that comes with 0.0.132 and that fixed it! Sorry about the noise.

---

_Closed by @brettcannon on 2022-11-21 23:55_

---

_Comment by @charliermarsh on 2022-11-22 01:39_

No problem!

---
