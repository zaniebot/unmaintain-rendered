---
number: 6572
title: "support setting `SSL_CERT_FILE` in `pyproject.toml`"
type: issue
state: open
author: DetachHead
labels:
  - configuration
assignees: []
created_at: 2024-08-24T07:33:06Z
updated_at: 2025-01-15T23:51:09Z
url: https://github.com/astral-sh/uv/issues/6572
synced_at: 2026-01-10T01:24:02Z
---

# support setting `SSL_CERT_FILE` in `pyproject.toml`

---

_Issue opened by @DetachHead on 2024-08-24 07:33_

as far as i can tell, the cert file can only be configured with the environment variable, but because in my case the file is stored in git, it would be more convenient if it could also be configured in `pyproject.toml`

---

_Label `configuration` added by @charliermarsh on 2024-08-25 02:27_

---

_Comment by @samypr100 on 2024-08-25 20:42_

Yea, it was briefly discussed  in https://github.com/astral-sh/uv/pull/4171 adding support in a future PR for both `SSL_CERT_FILE` and `SSL_CLIENT_CERT`

---

_Comment by @zanieb on 2024-08-26 17:09_

It seems like bad practice to check-in an SSL certificate — can you share more about why you're doing that?

---

_Comment by @DetachHead on 2024-08-26 22:28_

it's a corporate environment with custom internal certificates that need to be known about by the libraries the project uses. it's much easier to just commit the certificate than getting everyone in the team to manually configure their tools to use it

---

_Comment by @samypr100 on 2024-08-26 22:31_

> Yea, it was briefly discussed in #4171 adding support in a future PR for both `SSL_CERT_FILE` and `SSL_CLIENT_CERT`

I should clarify what I was thinking originally
**Supporting CLI equivalent and global settings (e.g. pip.conf style), not at project level pyproject.toml

---

_Comment by @DetachHead on 2024-08-26 22:56_

it might also be worth noting that my team uses [pyprojectx](https://pyprojectx.github.io/) to manage the uv installation locally in our project instead of installing it globally. we prefer to minimize the amount of stuff we have to install/configure globally

---

_Referenced in [astral-sh/uv#6715](../../astral-sh/uv/issues/6715.md) on 2024-08-27 19:18_

---

_Comment by @DetachHead on 2025-01-15 23:51_

just found out about the [`native-tls`](https://docs.astral.sh/uv/reference/settings/#native-tls) option, which works for my use case.

i'll leave this issue open in case anybody needs to be able to specify the path to the cert file

---

_Referenced in [servo/book#53](../../servo/book/issues/53.md) on 2025-02-21 05:46_

---

_Referenced in [astral-sh/uv#11728](../../astral-sh/uv/issues/11728.md) on 2025-02-23 21:10_

---
