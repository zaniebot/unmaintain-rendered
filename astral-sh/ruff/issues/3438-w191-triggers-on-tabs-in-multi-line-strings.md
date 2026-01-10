```yaml
number: 3438
title: "`W191` triggers on tabs in multi-line strings"
type: issue
state: closed
author: stinodego
labels:
  - bug
assignees: []
created_at: 2023-03-10T12:59:31Z
updated_at: 2023-04-07T02:26:09Z
url: https://github.com/astral-sh/ruff/issues/3438
synced_at: 2026-01-10T11:09:46Z
```

# `W191` triggers on tabs in multi-line strings

---

_Issue opened by @stinodego on 2023-03-10 12:59_

The following file gets flagged. 

```python
greeting = """hello
	world
"""
```

The lint should not trigger here, as this is not Python program indentation, it's simply part of a string. It should be no different from the following example, which is NOT flagged:


```python
greeting = """hello
world	:)
"""
```

This is also a known issue in pycodestyle:
https://github.com/PyCQA/pycodestyle/issues/376

---

_Label `bug` added by @charliermarsh on 2023-03-10 16:07_

---

_Comment by @charliermarsh on 2023-03-10 16:07_

Ah yeah, kind of a "known" bug which was deemed correct because of pycodestyle's behavior. If it's considered a bug in pycodestyle, though, we can definitely fix.

---

_Comment by @charliermarsh on 2023-04-07 02:26_

Closed by #3837.

---

_Closed by @charliermarsh on 2023-04-07 02:26_

---
