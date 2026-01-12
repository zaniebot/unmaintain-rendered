```yaml
number: 17428
title: uv and system installed Python installations
type: issue
state: open
author: zoof
labels:
  - question
assignees: []
created_at: 2026-01-12T23:07:20Z
updated_at: 2026-01-12T23:11:59Z
url: https://github.com/astral-sh/uv/issues/17428
synced_at: 2026-01-12T23:24:19Z
```

# uv and system installed Python installations

---

_@zoof_

### Question

My organization is considering adopting uv but I have a question.  For security reasons, we'd like to have uv use a version of Python that has been centrally installed so that it is kept up-to-date.  If an environment has been set up to use this Python installation, and Python gets updated by system administrators, say from version 3.12 to 3.13, what are the consequences for existing venv's?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @zoof on 2026-01-12 23:07_

---

_Comment by @zanieb on 2026-01-12 23:11_

When you update Python do you delete the old interpreter / installation?

---
