```yaml
number: 1009
title: Combine dependency clauses with the same root
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-01-19T17:15:07Z
updated_at: 2024-06-27T11:25:24Z
url: https://github.com/astral-sh/uv/issues/1009
synced_at: 2026-01-12T15:58:25Z
```

# Combine dependency clauses with the same root

---

_@zanieb_

e.g. instead of "you require a and you require b" we should say "you require a and b"

_Originally posted by @charliermarsh in https://github.com/astral-sh/puffin/pull/982#discussion_r1458258923_
            

---

_Label `error messages` added by @zanieb on 2024-01-19 17:15_

---

_Renamed from "Combine clauses with the same root" to "Combine dependency clauses with the same root" by @zanieb on 2024-01-19 17:15_

---

_Closed by @ibraheemdev on 2024-04-24 16:34_

---

_Reopened by @ibraheemdev on 2024-04-24 16:34_

---

_Comment by @ibraheemdev on 2024-04-24 16:35_

#3225 does this for requirement errors, but we might be able to do this for more types of errors. e.g. "because foo is unavailable and bar is unavailable" could become "because foo and bar are unavailable" (https://github.com/astral-sh/uv/pull/3225#issuecomment-2073608295).

---

_Comment by @konstin on 2024-06-27 11:25_

Closing as a special case of #1901

---

_Closed by @konstin on 2024-06-27 11:25_

---
