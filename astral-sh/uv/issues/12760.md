```yaml
number: 12760
title: Avoid GitHub queries for immutable references
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-04-08T20:08:00Z
updated_at: 2025-04-09T12:33:14Z
url: https://github.com/astral-sh/uv/issues/12760
synced_at: 2026-01-10T03:41:47Z
```

# Avoid GitHub queries for immutable references

---

_Issue opened by @zanieb on 2025-04-08 20:08_

per https://github.com/astral-sh/uv/issues/12746#issuecomment-2787524690 it looks like we're not doing this as intended. This may require some investigation.

---

_Label `bug` added by @zanieb on 2025-04-08 20:08_

---

_Comment by @charliermarsh on 2025-04-08 20:37_

I can take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-08 20:51_

---

_Comment by @charliermarsh on 2025-04-08 20:51_

Ah ok, I see the problem here.

---

_Closed by @charliermarsh on 2025-04-09 02:00_

---

_Closed by @charliermarsh on 2025-04-09 02:00_

---

_Comment by @sbidoul on 2025-04-09 09:40_

I confirm the issue is resolved. 

Issue opened in the evening, fixed when you wake up 😍 

---

_Comment by @charliermarsh on 2025-04-09 12:33_

Thank you for filing, it was a really good catch (and my mistake)!

---
