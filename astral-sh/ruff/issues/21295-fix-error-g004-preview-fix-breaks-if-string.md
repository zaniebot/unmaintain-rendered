```yaml
number: 21295
title: "[Fix error] G004 preview fix breaks if string contains newline"
type: issue
state: closed
author: mkj-perfusiontech
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-11-06T14:48:54Z
updated_at: 2025-11-10T22:26:51Z
url: https://github.com/astral-sh/ruff/issues/21295
synced_at: 2026-01-12T15:54:57Z
```

# [Fix error] G004 preview fix breaks if string contains newline

---

_@mkj-perfusiontech_

Minimal example:

(file: dummy.py)
```python
import logging

bar = "bar"
logging.getLogger().error(f"Lorem ipsum \n {bar}")
```

Run command `ruff check path/to/dummy.py --select G004 --fix --preview` (with nothing in config files). It seemingly tries to refactor the log line to

```python
logging.getLogger().error("Lorem ipsum 
%s", foo)
```
 and gets a syntax error.

Ruff version: 0.14.3

---

_Comment by @ntBre on 2025-11-06 15:27_

Thanks for the report!

---

_Label `bug` added by @ntBre on 2025-11-06 15:27_

---

_Label `fixes` added by @ntBre on 2025-11-06 15:27_

---

_Label `preview` added by @ntBre on 2025-11-06 15:27_

---

_Comment by @ntBre on 2025-11-10 22:26_

I didn't realize it last week, but I think this is a duplicate of https://github.com/astral-sh/ruff/issues/20151. The second example there includes a case with a newline and resulting syntax error.

---

_Closed by @ntBre on 2025-11-10 22:26_

---
