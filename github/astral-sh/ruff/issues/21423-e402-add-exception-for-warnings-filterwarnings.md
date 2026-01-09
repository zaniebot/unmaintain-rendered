---
number: 21423
title: "[E402] Add exception for warnings.filterwarnings() before imports"
type: issue
state: open
author: tsvikas
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-13T11:13:50Z
updated_at: 2025-11-16T11:41:43Z
url: https://github.com/astral-sh/ruff/issues/21423
synced_at: 2026-01-07T13:12:16-06:00
---

# [E402] Add exception for warnings.filterwarnings() before imports

---

_Issue opened by @tsvikas on 2025-11-13 11:13_

E402 currently allows `sys.path` and `os.environ` modifications before imports. I'd like to propose adding `warnings.filterwarnings()` (and related warning module functions) to this exception list.

**Rationale:**
- It's a common pattern to filter warnings before importing modules that emit warnings during import
- Like `sys.path` modifications, warning filters affect import-time behavior
- Currently requires verbose `# noqa: E402` comments on every subsequent import

**Example:**
```python
import warnings

warnings.filterwarnings("ignore", category=DeprecationWarning)

import some_module  # Currently triggers E402
```

**Proposed:** Treat `warnings.filterwarnings()`, `warnings.simplefilter()`, and `warnings.resetwarnings()` the same as `sys.path` modifications.

---

_Comment by @ntBre on 2025-11-13 15:29_

This is related to some other recent issues around E402, such as https://github.com/astral-sh/ruff/issues/19928.

I think it makes sense to me to expand the known exceptions for cases in the standard library. I'm pretty sure I've run into this one myself before. But I'm curious what others think too.

---

_Label `rule` added by @ntBre on 2025-11-13 15:29_

---

_Label `needs-decision` added by @ntBre on 2025-11-13 15:29_

---

_Comment by @tsvikas on 2025-11-16 11:34_

Following your comment, i found out that several similar cases exist: https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20E402

Might I suggest, that rather than hard-coding exceptions for each case, we will add a directive that allows marking non-import statements as intentionally placed within the import block:

```python
import sys
sys.path.append("../lib")  # ruff: import-setup
import some_module

import warnings
warnings.filterwarnings("ignore", category=DeprecationWarning)  # ruff: import-setup
import some_other_module

import logging
logging.basicConfig(level=logging.DEBUG)  # ruff: import-setup
import yet_another_module
```

This would:
- Signal explicit intent, making code more self-documenting
- Avoid the need to suppress E402 on all subsequent imports
- Allow any statement to be marked as "import setup" without needing hard-coded exceptions
- Be more maintainable than case-by-case `# noqa: E402` on every import that follows

The directive would tell Ruff to treat the marked line as if it were part of the import block preamble, so subsequent imports don't trigger E402.

---

_Comment by @MichaReiser on 2025-11-16 11:41_

Thanks for the suggestion. I like the idea of making it easier to suppress the rule.

I don't think we should introduce rule-specific pragma comments. Simply because it adds complexity for users, they need to learn a new pragma comment, and it adds a lot of complexity to ruff, it needs to understand an extra pragma comment.


I think #3711 will greatly help here as it allows to disable the rule for an entire block of code

---
