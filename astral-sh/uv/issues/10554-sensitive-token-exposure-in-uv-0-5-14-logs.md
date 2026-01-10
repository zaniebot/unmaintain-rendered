---
number: 10554
title: Sensitive token exposure in uv 0.5.14 logs
type: issue
state: closed
author: rivershah
labels:
  - duplicate
  - security
assignees: []
created_at: 2025-01-13T07:11:13Z
updated_at: 2025-01-14T08:49:40Z
url: https://github.com/astral-sh/uv/issues/10554
synced_at: 2026-01-10T01:24:54Z
---

# Sensitive token exposure in uv 0.5.14 logs

---

_Issue opened by @rivershah on 2025-01-13 07:11_

Private indices use private token based access with the pattern:

`https://<token>@><domain>/pypi/simple`

When installing packages from such indices, uv 0.5.14 logs the token in plain text to stdout. This behavior exposes sensitive credentials, whereas pip avoids this issue.

Request: Can uv adopt a logging approach similar to pip to avoid exposing sensitive tokens in plain text?




---

_Comment by @FishAlchemist on 2025-01-13 07:33_

* HTTP authentication
https://docs.astral.sh/uv/configuration/authentication/#http-authentication
* Package indexes authentication
https://docs.astral.sh/uv/configuration/indexes/#providing-credentials

You can choose to use one of the above two methods.

-----
As for pip's configuration file, uv does not read it.
https://docs.astral.sh/uv/pip/compatibility/#configuration-files-and-environment-variables

---

_Comment by @rivershah on 2025-01-13 08:24_

uv logs credentials to stdout in several scenarios:
	1.	`uv pip install --system "package @ https://$TOKEN@<domain>/pypi/simple"`
	2.	`uv pip install --system "package @ git+https://$TOKEN@<domain>/package.git"`

Example output:

```Installed 1 package in 1ms
-package==x.x.x (from https://token_in_plain_text@<domain>/pypi/simple)
+package==x.x.x (from git+https://token_in_plain_text@<domain>/package.git)
```

---

_Comment by @charliermarsh on 2025-01-13 15:35_

This is a duplicate of #1714, but thanks for the added information. I'll merge with that issue.

---

_Closed by @charliermarsh on 2025-01-13 15:35_

---

_Label `duplicate` added by @charliermarsh on 2025-01-13 15:35_

---

_Label `security` added by @charliermarsh on 2025-01-13 15:35_

---

_Comment by @rivershah on 2025-01-14 08:49_

Once again @charliermarsh I am really sorry about creating extra work and noise when an issue already existed.
github issues search is ridiculously bad. let me see if llm tools do a better job than built in search

---
