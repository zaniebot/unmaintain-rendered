---
number: 1775
title: Any way to install a package from private repository with deploy key?
type: issue
state: closed
author: kolkre
labels:
  - enhancement
assignees: []
created_at: 2024-02-20T19:11:15Z
updated_at: 2024-02-21T01:31:17Z
url: https://github.com/astral-sh/uv/issues/1775
synced_at: 2026-01-07T13:12:16-06:00
---

# Any way to install a package from private repository with deploy key?

---

_Issue opened by @kolkre on 2024-02-20 19:11_

I'm attempting to install a package from our Git instance using the command uv pip install `git+https://<DEPLOY_KEY>@<HOST>/<REPO>`, but it appears that it's not implemented in uv. It works when I use the regular pip. 

Are there any plans to implement this?

---

_Comment by @charliermarsh on 2024-02-20 19:11_

Yup! We're working on it right now. Hopefully this week.

---

_Assigned to @zanieb by @zanieb on 2024-02-20 19:25_

---

_Referenced in [astral-sh/uv#1781](../../astral-sh/uv/pulls/1781.md) on 2024-02-20 20:08_

---

_Comment by @zanieb on 2024-02-20 20:25_

Hey @kolkre, just want to clarify: how are you specifying your deploy key?

---

_Label `enhancement` added by @zanieb on 2024-02-21 00:19_

---

_Comment by @kolkre on 2024-02-21 01:24_

> Hey @kolkre, just want to clarify: how are you specifying your deploy key?

It's not an SSH key. It's a text-format token that should be specified in the `Authorization` header for HTTP requests to Git.

---

_Comment by @zanieb on 2024-02-21 01:25_

I see thanks. GitHub uses the term "Deploy keys" for SSH keys. Your issue was resolved in #1717 and will be out in the next release.

---

_Closed by @zanieb on 2024-02-21 01:25_

---

_Comment by @kolkre on 2024-02-21 01:28_

I just noticed a similar issue has already been opened, and the related PR has been merged (https://github.com/astral-sh/uv/pull/1717). Perhaps it would be good to mention this in the documentation/examples.

---
