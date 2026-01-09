---
number: 7731
title: Syntax error while parsing variables containing unicode? character
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
created_at: 2023-10-01T02:30:47Z
updated_at: 2023-10-01T03:30:09Z
url: https://github.com/astral-sh/ruff/issues/7731
synced_at: 2026-01-07T13:12:15-06:00
---

# Syntax error while parsing variables containing unicode? character

---

_Issue opened by @dhruvmanila on 2023-10-01 02:30_

Surfaced in #7632. Maybe it's related to the encoding comment?

```python
# -*- coding: iso-8859-15 -*-
¸uite = 1
```

```python
# -*- coding: latin-1 -*-
cu÷t_ie = 1
``` 

```python
# -*- coding: iso-8859-1 -*-
macro.÷equest.getText = 1
``` 

---

_Label `bug` added by @dhruvmanila on 2023-10-01 02:30_

---

_Label `parser` added by @dhruvmanila on 2023-10-01 02:30_

---

_Comment by @charliermarsh on 2023-10-01 03:30_

I confirmed that all of these are due to the alternative encoding (if you remove the coding comment, Python fails to parse these files), which we don't currently support. Merging into https://github.com/astral-sh/ruff/issues/6791.

---

_Closed by @charliermarsh on 2023-10-01 03:30_

---
