```yaml
number: 10684
title: Getting no warning if uv gets 403 on Private Repository
type: issue
state: closed
author: dvonessen
labels:
  - question
assignees: []
created_at: 2025-01-16T16:26:36Z
updated_at: 2025-03-21T15:19:22Z
url: https://github.com/astral-sh/uv/issues/10684
synced_at: 2026-01-10T03:50:30Z
```

# Getting no warning if uv gets 403 on Private Repository

---

_Issue opened by @dvonessen on 2025-01-16 16:26_

Hello,

First of all, thank you for this excellent piece of software as well as Ruff. My team and I are loving it!

I believe we may have encountered a bug.

We are using private JFrog/Artifactory PyPI repositories. When new users want to access them, they need permission to do so. If they do not have permissions on one of our repositories, and there is a package with the same name on PyPI.org, UV does not issue a warning that the client received a 403 Access Denied. Instead, it installs the package from PyPI.org.

We run UV with the --verbose flag, but nothing in the debug logs indicates that UV attempted to connect to the private repository and received a 403 error.

Maybe I misunderstand something, or it could be a bug?

Thank you for your time and effort.

---

_Comment by @zanieb on 2025-01-16 17:09_

I believe it's a JFrog behavior to pass-through to PyPI when credentials are not provided — that's not a uv feature. We're probably not even receiving a 403.

If you provide a username on the index URL, that will force uv to use an authenticated request.

---

_Label `question` added by @zanieb on 2025-01-16 17:09_

---

_Comment by @zanieb on 2025-03-21 15:19_

Let me know if you have more questions here.

---

_Closed by @zanieb on 2025-03-21 15:19_

---
