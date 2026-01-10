---
number: 3017
title: "Allow sha256-<hash> (for Gitea/Forgejo compatibility)"
type: issue
state: closed
author: Zottelchen
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-04-12T16:26:31Z
updated_at: 2024-04-13T01:09:22Z
url: https://github.com/astral-sh/uv/issues/3017
synced_at: 2026-01-10T01:23:24Z
---

# Allow sha256-<hash> (for Gitea/Forgejo compatibility)

---

_Issue opened by @Zottelchen on 2024-04-12 16:26_

When trying to install a package from a private Forgejo/Gitea instance with rye 0.32.0 (`rye add private_package`)  it fails, because of the following uv error:

```
error: Failed to run uv compile error: Received some unexpected HTML from https://git.example.com/api/packages/Zottelchen/pypi/simple/private_package/
  Caused by: Unexpected fragment (expected `#sha256=...` or similar) on URL: sha256-2dad1cbbf357953b94b0c57a240e5217eb59300f303e4d35215fe7d07478f089
. uv exited with status: exit code: 2
``` 

[The issue lies in the way the Forgejo/Gitea template formats the link.](https://codeberg.org/forgejo/forgejo/src/commit/0c42e3c7557ff03614c99e14487a0844a649f393/templates/api/packages/pypi/simple.tmpl#L11) So currently the package links look like this: `https://git.example.com/api/packages/Zottelchen/pypi/files/private_package/0.1.0/private_package-0.1.0-py3-none-any.whl#sha256-2dad1cbbf357953b94b0c57a240e5217eb59300f303e4d35215fe7d07478f089`

A quick fix for this would be to also allow the minus  in the  `sha256-`fragment.





---

_Comment by @Zottelchen on 2024-04-12 16:47_

Also opened a issue/PR with Forgejo: https://codeberg.org/forgejo/forgejo/issues/3189

---

_Comment by @charliermarsh on 2024-04-12 17:35_

Ah yeah, that's interesting. It looks like a bug in the registry, given the spec? My guess is that pip drops those hashes altogether and doesn't enforce them, as if they didn't exist, but I'd have to look back at the pip code.

---

_Label `compatibility` added by @charliermarsh on 2024-04-12 17:35_

---

_Label `needs-decision` added by @charliermarsh on 2024-04-12 17:35_

---

_Comment by @notatallshaw on 2024-04-13 00:14_

It seems like Forgejo has already fix this https://codeberg.org/forgejo/forgejo/pulls/3197 ?

---

_Comment by @charliermarsh on 2024-04-13 00:58_

Oh great! Thanks.

---

_Closed by @charliermarsh on 2024-04-13 00:58_

---

_Comment by @notatallshaw on 2024-04-13 01:09_

Or specifically they accepted @Zottelchen PR and backported it already 🎉

---

_Referenced in [go-gitea/gitea#30594](../../go-gitea/gitea/pulls/30594.md) on 2024-04-19 21:58_

---

_Referenced in [go-gitea/gitea#30612](../../go-gitea/gitea/pulls/30612.md) on 2024-04-20 01:15_

---
