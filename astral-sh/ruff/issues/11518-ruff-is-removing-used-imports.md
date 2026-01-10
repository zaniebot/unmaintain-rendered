```yaml
number: 11518
title: Ruff is removing used imports
type: issue
state: closed
author: lcsvcn
labels:
  - bug
assignees: []
created_at: 2024-05-23T17:46:21Z
updated_at: 2024-05-28T18:47:07Z
url: https://github.com/astral-sh/ruff/issues/11518
synced_at: 2026-01-10T11:09:53Z
```

# Ruff is removing used imports

---

_Issue opened by @lcsvcn on 2024-05-23 17:46_

Removing used imports (probably wrong classified as "unused"

```
ruff check . --fix --verbose
ruff format --verbose
```
Here is autoformat configured with commands above:
<img width="1274" alt="Screenshot 2024-05-23 at 14 36 51" src="https://github.com/astral-sh/ruff/assets/6011385/92f917b5-08e1-4c3b-a228-f61df9ed3689">

here is a function that is using the import:
<img width="424" alt="Screenshot 2024-05-23 at 14 37 29" src="https://github.com/astral-sh/ruff/assets/6011385/e7d1ca0a-1a17-4570-bbe8-e93c4ca0ee56">

The ruff format insists in remove used import, it was not first time, but now it is first time it was doing several times in a row.

---

_Comment by @charliermarsh on 2024-05-23 17:59_

Can you please include a full reproducible example, like a Python file that demonstrates the bug? That snippet alone isn’t enough (and can’t be copy-pasted) since it doesn’t contain any imports.

---

_Comment by @zanieb on 2024-05-23 18:04_

Might be a duplicate of https://github.com/astral-sh/ruff/issues/8691

---

_Label `needs-info` added by @dhruvmanila on 2024-05-24 06:43_

---

_Comment by @charliermarsh on 2024-05-26 17:17_

Closing as a dupe, looks that way.

---

_Closed by @charliermarsh on 2024-05-26 17:17_

---

_Reopened by @charliermarsh on 2024-05-28 01:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 01:52_

---

_Label `needs-info` removed by @charliermarsh on 2024-05-28 01:52_

---

_Label `bug` added by @charliermarsh on 2024-05-28 01:52_

---

_Comment by @charliermarsh on 2024-05-28 01:53_

I think it's actually slightly different than #8691.

---

_Closed by @charliermarsh on 2024-05-28 18:47_

---
