```yaml
number: 4425
title: Add option to disable fetching managed toolchains
type: pull_request
state: closed
author: zanieb
labels:
  - preview
assignees: []
draft: true
base: main
head: zb/toolchain-only-installed
created_at: 2024-06-20T15:45:51Z
updated_at: 2024-07-02T01:59:15Z
url: https://github.com/astral-sh/uv/pull/4425
synced_at: 2026-01-10T13:48:28Z
```

# Add option to disable fetching managed toolchains

---

_Pull request opened by @zanieb on 2024-06-20 15:45_

Adds a toolchain preference (introduced in #4424) to only allow installed managed toolchains.

This might not actually belong in the toolchain preferences? i.e. the way it's currently spelled there's no option for "prefer system but allow installed managed toolchains". Enabling or disabling fetches could be totally independent of the toolchain preference. I'm on the fence on what the correct approach is. Regardless, this functionality is important and this is a good starting point for discussion on how to expose it.

---

_Converted to draft by @zanieb on 2024-06-20 15:46_

---

_Label `preview` added by @zanieb on 2024-06-20 15:46_

---

_Comment by @zanieb on 2024-07-02 01:59_

Replaced by #4601 

---

_Closed by @zanieb on 2024-07-02 01:59_

---
