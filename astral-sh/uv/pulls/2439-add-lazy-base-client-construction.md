```yaml
number: 2439
title: Add lazy base client construction
type: pull_request
state: closed
author: zanieb
labels:
  - performance
assignees: []
draft: true
base: main
head: zb/base-client-lazy
created_at: 2024-03-14T00:21:55Z
updated_at: 2024-03-21T19:10:31Z
url: https://github.com/astral-sh/uv/pull/2439
synced_at: 2026-01-10T14:49:08Z
```

# Add lazy base client construction

---

_Pull request opened by @zanieb on 2024-03-14 00:21_

I'm basically sure I'm doing this wrong... but it's a start

---

_Label `performance` added by @zanieb on 2024-03-14 00:22_

---

_Comment by @charliermarsh on 2024-03-21 18:49_

One way to think about this is: instead of a lazy builder, can we have a client that is lazy internally...

---

_Comment by @zanieb on 2024-03-21 18:50_

Yeah I agree having a separate type seems overkill. I've opened an issue for this at #2597 — I do not think I will pursue this right now.

---

_Closed by @zanieb on 2024-03-21 18:50_

---

_Comment by @charliermarsh on 2024-03-21 18:57_

It feels like it needs @BurntSushi touch...

---

_Comment by @zanieb on 2024-03-21 19:10_

lol agree

---
