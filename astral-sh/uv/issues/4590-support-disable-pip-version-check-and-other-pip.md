```yaml
number: 4590
title: "Support `--disable-pip-version-check` and other pip flags"
type: issue
state: closed
author: jeffgeee
labels:
  - question
assignees: []
created_at: 2024-06-27T16:44:04Z
updated_at: 2024-06-30T23:43:25Z
url: https://github.com/astral-sh/uv/issues/4590
synced_at: 2026-01-12T15:58:51Z
```

# Support `--disable-pip-version-check` and other pip flags

---

_@jeffgeee_

`uv` is supposed to be a "Drop-in replacement for common pip, pip-tools, and virtualenv commands."

Recently testing with some python scripts which used `--disable-pip-version-check` I found this to not be the case, along with a few other flags. Was this being tracked anywhere?

---

_Comment by @zanieb on 2024-06-27 17:00_

Please consult our compatibility guide: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md

If there are things that aren't documented there, feel free to let us know or add in a pull request.

---

_Label `question` added by @zanieb on 2024-06-27 17:00_

---

_Comment by @charliermarsh on 2024-06-27 17:38_

I guess we could add that to the compatibility args but it's obviously a weird one.

---

_Closed by @charliermarsh on 2024-06-30 23:43_

---

_Closed by @charliermarsh on 2024-06-30 23:43_

---
