```yaml
number: 10748
title: "\"uv add\" vs \"uv pip install\""
type: issue
state: closed
author: AGitUser111
labels:
  - question
assignees: []
created_at: 2025-01-19T02:32:46Z
updated_at: 2025-01-19T04:14:21Z
url: https://github.com/astral-sh/uv/issues/10748
synced_at: 2026-01-12T16:00:20Z
```

# "uv add" vs "uv pip install"

---

_@AGitUser111_

When trying to install fastapi-users with `uv add`, the module appears to be installed and intellisense will work while navigating through the module tree, but I cannot successfully import any of the classes. 

for example, the intellisense works for:
    ```from fastapi_users.db import SQLAlchemyBaseUserTableUUID```

But running will result in:
   ``` ImportError: cannot import name 'SQLAlchemyBaseUserTableUUID' from 'fastapi_users.db'```

This is not the case when installed with `uv pip install`

Is this to do with how it has been packaged? What makes something (in)compatible with `uv add` vs `uv pip install`?

---

_Comment by @charliermarsh on 2025-01-19 02:36_

I assume that they are installing into different virtual environments. `uv add` will always install into the `.venv` directory located at the project root. 

---

_Label `question` added by @charliermarsh on 2025-01-19 02:40_

---

_Comment by @AGitUser111 on 2025-01-19 03:35_

I thought that installing with `uv pip install` had fixed the issue but I was incorrect. 
I did figure out the issue. I am just dumb and hadn't used `--extra sqlalchemy`. 

Thanks for your quick reply and sorry to waste your time. 



---

_Closed by @AGitUser111 on 2025-01-19 03:35_

---

_Comment by @charliermarsh on 2025-01-19 03:43_

No worries, I'm glad you were able to resolve it!

---
