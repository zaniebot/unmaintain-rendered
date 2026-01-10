```yaml
number: 9432
title: Include explicit separator when constructing path to Windows global config file
type: pull_request
state: closed
author: zanieb
labels:
  - bug
  - windows
assignees: []
draft: true
base: main
head: zb/system-drive
created_at: 2024-11-25T23:19:09Z
updated_at: 2024-11-28T00:32:40Z
url: https://github.com/astral-sh/uv/pull/9432
synced_at: 2026-01-10T12:00:00Z
```

# Include explicit separator when constructing path to Windows global config file

---

_Pull request opened by @zanieb on 2024-11-25 23:19_

Closes https://github.com/astral-sh/uv/issues/9416

---

_Label `bug` added by @zanieb on 2024-11-25 23:19_

---

_Label `windows` added by @zanieb on 2024-11-25 23:19_

---

_Comment by @zanieb on 2024-11-25 23:50_

😭 

https://github.com/astral-sh/uv/blob/9fd6958a16f56c1e3fd4d40cb82df41ff1737e60/crates/uv-settings/src/lib.rs#L369-L378

---

_@zanieb reviewed on 2024-11-26 18:32_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:220 on 2024-11-26 18:32_

So... `prefix` is private. I'm not sure what to do here. I think we need to change the way we're testing this?

---

_Review requested from @charliermarsh by @zanieb on 2024-11-26 18:32_

---

_Converted to draft by @zanieb on 2024-11-27 01:43_

---

_Comment by @charliermarsh on 2024-11-27 22:07_

Taking a look on my Windows machine.

---

_Comment by @zanieb on 2024-11-28 00:32_

Replaced by https://github.com/astral-sh/uv/pull/9488

---

_Closed by @zanieb on 2024-11-28 00:32_

---
