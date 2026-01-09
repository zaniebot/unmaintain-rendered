---
number: 13453
title: native-tls=true option in user uv.toml?
type: issue
state: closed
author: tazr
labels:
  - question
assignees: []
created_at: 2025-05-14T16:05:02Z
updated_at: 2025-05-14T18:44:48Z
url: https://github.com/astral-sh/uv/issues/13453
synced_at: 2026-01-07T13:12:18-06:00
---

# native-tls=true option in user uv.toml?

---

_Issue opened by @tazr on 2025-05-14 16:05_

### Question

I know I can set the environment varialbe UV_NATIVE_TLS to true, however I would prefer to have all my global settings in one place. I was wondering if this option could be set in the user uv.toml file? If not, would that be a reasonable option?

### Platform

Linux 6.8.0-59-generic x86_64 GNU/Linux

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

---

_Label `question` added by @tazr on 2025-05-14 16:05_

---

_Comment by @charliermarsh on 2025-05-14 18:44_

Yeah, you should be able to set it at the top-level of your `uv.toml` (`native-tls = true`). See: https://docs.astral.sh/uv/reference/settings/#native-tls.

---

_Closed by @charliermarsh on 2025-05-14 18:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-14 18:44_

---
