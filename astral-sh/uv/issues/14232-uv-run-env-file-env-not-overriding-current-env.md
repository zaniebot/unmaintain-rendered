---
number: 14232
title: "`uv run --env-file=.env` not overriding current env"
type: issue
state: closed
author: Hellzed
labels:
  - question
assignees: []
created_at: 2025-06-24T08:36:14Z
updated_at: 2025-06-24T15:50:46Z
url: https://github.com/astral-sh/uv/issues/14232
synced_at: 2026-01-10T01:25:43Z
---

# `uv run --env-file=.env` not overriding current env

---

_Issue opened by @Hellzed on 2025-06-24 08:36_

### Question

Currently, when running `uv run --env-file=.env`, current env is not overridden.

Given the following **.env** file:
```shell
GREETING=hello
```

Running `GREETING=bonjour uv run --env-file=.env` will display 

> bonjour

Is this the expected behaviour? Doc doesn't make it explicitly cleat that the env is not overridden, and this can be surprising behaviour when comparing with similar tools (as using `--env-file` will not grant you a clean slate if you've previously `export`ed env vars and forgot about it).

### Platform

Linux 6.14.0-22-generic x86_64 GNU/Linux

### Version

uv 0.7.14+2 (aa2448ef8 2025-06-23)

---

_Label `question` added by @Hellzed on 2025-06-24 08:36_

---

_Comment by @charliermarsh on 2025-06-24 13:37_

I believe this is by design and consistent with how most tools treat `.env` files, e.g., https://www.npmjs.com/package/dotenv among others.

---

_Comment by @loris-mayday on 2025-06-24 15:46_

Thank you for the reply, that makes sense!

---

_Closed by @charliermarsh on 2025-06-24 15:50_

---
