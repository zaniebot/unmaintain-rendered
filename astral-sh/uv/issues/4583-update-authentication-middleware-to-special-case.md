```yaml
number: 4583
title: Update authentication middleware to special-case index urls
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - network
assignees: []
created_at: 2024-06-27T11:03:00Z
updated_at: 2025-04-29T21:37:04Z
url: https://github.com/astral-sh/uv/issues/4583
synced_at: 2026-01-10T03:41:46Z
```

# Update authentication middleware to special-case index urls

---

_Issue opened by @zanieb on 2024-06-27 11:03_

As discussed in https://github.com/astral-sh/uv/issues/4056, pip makes a keyring request for credentials for the _index url_ but we make a request for the _package url_ and the _index url host name_ which for some plugins (e.g. Azure #3542) does not seem to work.

We should update the authentication handling to be aware of the user-provided index urls and, when we see a url that is a child of an index url, attempt to fetch credentials for the index url either after or instead of the package url.

---

_Label `help wanted` added by @zanieb on 2024-06-27 11:03_

---

_Label `network` added by @zanieb on 2024-06-27 11:03_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-02 17:33_

---

_Closed by @zanieb on 2025-04-29 21:37_

---
