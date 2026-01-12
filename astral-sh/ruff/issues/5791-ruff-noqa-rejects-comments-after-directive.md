```yaml
number: 5791
title: "`# ruff: noqa` rejects comments after directive"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-07-16T00:38:09Z
updated_at: 2023-07-16T05:07:04Z
url: https://github.com/astral-sh/ruff/issues/5791
synced_at: 2026-01-12T15:54:45Z
```

# `# ruff: noqa` rejects comments after directive

---

_@charliermarsh_

_No description provided._

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-16 00:38_

---

_Label `bug` added by @charliermarsh on 2023-07-16 00:38_

---

_Comment by @farmio on 2023-07-16 04:57_

Hi 👋! 
You asked me to paste the exact code raising the warnings. So here it is
https://github.com/XKNX/xknx/blob/ef92f03db5bdbf163f6010bd545c1ba20007318d/xknx/dpt/dpt_hvac_mode.py#L15

The comment-comment text does not seem to matter - even 
```py
# ruff: noqa: RUF012  # d
``` 
would raise warnings
```
lint: commands[0]> ruff check .
warning: Invalid code provided to `# ruff: noqa`: #
warning: Invalid code provided to `# ruff: noqa`: d
```

---

_Comment by @charliermarsh on 2023-07-16 05:05_

Thanks so much!

---

_Comment by @charliermarsh on 2023-07-16 05:06_

Ok, I just confirmed that I see these warnings on 0.0.277, but not on 0.0.278. So, hopefully fixed by upgrading Ruff, but let me know if you see otherwise.

---

_Closed by @charliermarsh on 2023-07-16 05:06_

---
