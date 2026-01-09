---
number: 13777
title: "Feature Request: Prevent useless import aliases"
type: issue
state: closed
author: Diaoul
labels:
  - question
assignees: []
created_at: 2024-10-16T12:29:31Z
updated_at: 2024-10-16T18:04:47Z
url: https://github.com/astral-sh/ruff/issues/13777
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature Request: Prevent useless import aliases

---

_Issue opened by @Diaoul on 2024-10-16 12:29_

Following a refactor renaming an alias, I ended up with this kind of import statement

```python
from myapplication.queuing import Queue as Queue
```

**Current behavior:** This passes ruff linting and formatting
**Expected behavior:** Autocorrected

Do you think this could be implemented in ruff?

---

_Comment by @charliermarsh on 2024-10-16 13:04_

Unfortunately I don't think we can lint against that -- it's actually a semantically meaningful pattern in Python. A "redundant re-export" will cause analyzers to treat it as an intentional re-export of the variable.

---

_Comment by @charliermarsh on 2024-10-16 13:07_

You can read more about it here: https://github.com/astral-sh/ruff/issues/717

---

_Label `question` added by @MichaReiser on 2024-10-16 13:08_

---

_Comment by @sbrugman on 2024-10-16 13:09_

Isn't this case already being covered by [useless-import-alias (PLC0414)](https://docs.astral.sh/ruff/rules/useless-import-alias/#useless-import-alias-plc0414) (opt-in via pylint)

---

_Comment by @charliermarsh on 2024-10-16 14:17_

Oh good call — thanks!

---

_Closed by @charliermarsh on 2024-10-16 14:17_

---

_Comment by @Diaoul on 2024-10-16 18:04_

Oops, can't believe I missed this... Sorry for the noise 🙈 

---
