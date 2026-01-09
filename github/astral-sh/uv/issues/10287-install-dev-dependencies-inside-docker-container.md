---
number: 10287
title: "Install dev dependencies inside docker container's system python"
type: issue
state: closed
author: rivershah
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-01-03T14:19:37Z
updated_at: 2025-01-03T15:36:34Z
url: https://github.com/astral-sh/uv/issues/10287
synced_at: 2026-01-07T13:12:18-06:00
---

# Install dev dependencies inside docker container's system python

---

_Issue opened by @rivershah on 2025-01-03 14:19_

This installs dependencies only:
`uv pip install --system --all-extras -r pyproject.toml`

How do we install specific groups please to system python, for example defined like this:

```
[dependency-groups]
    dev = ["pytest>=8.3.4"]
```

---

_Comment by @rivershah on 2025-01-03 14:24_

I restructured `pyproject.toml`
```
...
[project.optional-dependencies]
    dev = [
        "pytest==8.3.4",
...
    ]
```

`uv pip install --system -r pyproject.toml --extra dev` works fine

I can't get dependency groups to install though. Thanks for any guidance


---

_Comment by @charliermarsh on 2025-01-03 15:29_

I think you should track #8590 for that. We haven't added the group syntax to `uv pip` yet.

---

_Closed by @charliermarsh on 2025-01-03 15:29_

---

_Label `duplicate` added by @charliermarsh on 2025-01-03 15:29_

---

_Label `enhancement` added by @charliermarsh on 2025-01-03 15:29_

---

_Comment by @charliermarsh on 2025-01-03 15:29_

(We definitely want to support this, just doesn't exist yet.)

---

_Comment by @rivershah on 2025-01-03 15:35_

Thanks for closing this duplicate. Apologies I missed the existing issue

---

_Comment by @charliermarsh on 2025-01-03 15:36_

No worries at all, hard to find.

---
