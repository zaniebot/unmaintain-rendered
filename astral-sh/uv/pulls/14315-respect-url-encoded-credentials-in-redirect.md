```yaml
number: 14315
title: Respect URL-encoded credentials in redirect location
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: jtfm/redirect-url-credentials
created_at: 2025-06-27T13:56:52Z
updated_at: 2025-06-27T14:41:16Z
url: https://github.com/astral-sh/uv/pull/14315
synced_at: 2026-01-12T16:11:09Z
```

# Respect URL-encoded credentials in redirect location

---

_@jtfmumm_

uv currently ignores URL-encoded credentials in a redirect location. This PR adds a check for these credentials to the redirect handling logic. If found, they are moved to the Authorization header in the redirect request.

Closes #11097 


---

_Label `bug` added by @jtfmumm on 2025-06-27 13:56_

---

_Label `network` added by @jtfmumm on 2025-06-27 13:56_

---

_Renamed from "Handle URL-encoded credentials in redirect location" to "Respect URL-encoded credentials in redirect location" by @jtfmumm on 2025-06-27 13:57_

---

_@charliermarsh approved on 2025-06-27 14:31_

This seems reasonable to me, though with auth it's always hard to predict whether some obscure setup will break :sob:

---

_Comment by @jtfmumm on 2025-06-27 14:40_

> This seems reasonable to me, though with auth it's always hard to predict whether some obscure setup will break 😭

I think this is a pretty rare case in general, but we'll see!

---

_Merged by @jtfmumm on 2025-06-27 14:41_

---

_Closed by @jtfmumm on 2025-06-27 14:41_

---

_Branch deleted on 2025-06-27 14:41_

---
