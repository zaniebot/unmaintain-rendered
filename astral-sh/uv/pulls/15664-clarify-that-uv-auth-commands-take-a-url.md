```yaml
number: 15664
title: "Clarify that `uv auth` commands take a URL"
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/lookup-url
created_at: 2025-09-03T13:50:54Z
updated_at: 2025-09-03T14:16:17Z
url: https://github.com/astral-sh/uv/pull/15664
synced_at: 2026-01-12T16:11:53Z
```

# Clarify that `uv auth` commands take a URL

---

_@konstin_

From the previous description I tried `uv auth token pyx`, which didn't work.

---

_Label `documentation` added by @konstin on 2025-09-03 13:50_

---

_Comment by @zanieb on 2025-09-03 13:59_

It's not technically a URL because we don't require a scheme. Maybe can just say a bit more about what's supported here instead?

I'd actually be okay special casing "pyx" as well.

---

_Comment by @konstin on 2025-09-03 14:01_

I'm happy with whatever here, a description of what it accepts would be best here, though I don't have the context of what it's supposed to accept.

---

_Comment by @zanieb on 2025-09-03 14:01_

I think it's "a domain or URL" right now

---

_@zanieb approved on 2025-09-03 14:03_

---

_Renamed from "Clarify that `uv auth token` takes a URL" to "Clarify that `uv auth` commands take a URL" by @zanieb on 2025-09-03 14:04_

---

_Comment by @konstin on 2025-09-03 14:06_

Thanks!

---

_Merged by @zanieb on 2025-09-03 14:16_

---

_Closed by @zanieb on 2025-09-03 14:16_

---

_Branch deleted on 2025-09-03 14:16_

---
