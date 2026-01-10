```yaml
number: 11604
title: respect color settings for log messages
type: pull_request
state: merged
author: Gankra
labels:
  - bug
  - tracing
assignees: []
merged: true
base: main
head: gankra/colr
created_at: 2025-02-18T20:23:14Z
updated_at: 2025-02-18T20:39:31Z
url: https://github.com/astral-sh/uv/pull/11604
synced_at: 2026-01-10T11:10:38Z
```

# respect color settings for log messages

---

_Pull request opened by @Gankra on 2025-02-18 20:23_

fixes #11560 

---

_Label `bug` added by @Gankra on 2025-02-18 20:23_

---

_Label `tracing` added by @Gankra on 2025-02-18 20:23_

---

_Comment by @Gankra on 2025-02-18 20:26_

> Using anstream inside tracing would be great, I ran into trait typing problems last time I tried that.

I have a feeling I attempted the exact same fix konsti did and ran into exactly this issue and then I realized I was solving the problem at the wrong abstraction layer and it all worked totally fine.

---

_@zanieb approved on 2025-02-18 20:37_

---

_Merged by @Gankra on 2025-02-18 20:39_

---

_Closed by @Gankra on 2025-02-18 20:39_

---

_Branch deleted on 2025-02-18 20:39_

---
