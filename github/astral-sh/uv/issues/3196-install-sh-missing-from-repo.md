---
number: 3196
title: Install.sh Missing from Repo?
type: issue
state: closed
author: blakete
labels: []
assignees: []
created_at: 2024-04-22T19:50:06Z
updated_at: 2024-04-22T22:06:46Z
url: https://github.com/astral-sh/uv/issues/3196
synced_at: 2026-01-07T13:12:17-06:00
---

# Install.sh Missing from Repo?

---

_Issue opened by @blakete on 2024-04-22 19:50_

```console
$ curl -LsSf https://astral.sh/uv/install.sh
curl: (22) The requested URL returned error: 404
```
Therefore, the [Getting Started instructions here](https://github.com/astral-sh/uv#getting-started) do not work. 
```console
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```
And this, in my Dockerfile, also now no longer works because of this issue:
```Dockerfile
# Install UV
ADD https://astral.sh/uv/install.sh /install.sh
RUN chmod -R 655 /install.sh && /install.sh && rm /install.sh
```
Please fix this!!!

---

_Referenced in [astral-sh/uv#3197](../../astral-sh/uv/issues/3197.md) on 2024-04-22 19:53_

---

_Comment by @zanieb on 2024-04-22 19:53_

Investigating...

---

_Comment by @zanieb on 2024-04-22 20:02_

We've rolled the latest release back to 0.1.35. We're troubleshooting permissions errors in the publish for 0.1.36. Thanks for the report!

---

_Comment by @rodolfolottin on 2024-04-22 20:04_

Thank you @zanieb !

---

_Comment by @blakete on 2024-04-22 20:07_

Thanks @zanieb and @rodolfolottin 

---

_Referenced in [astral-sh/uv#3198](../../astral-sh/uv/issues/3198.md) on 2024-04-22 20:07_

---

_Comment by @zanieb on 2024-04-22 20:10_

We'll track the long term fix for this in #3198.

We'll either fix 0.1.36 or publish 0.1.37 today.

---

_Closed by @zanieb on 2024-04-22 20:10_

---

_Comment by @zanieb on 2024-04-22 20:13_

Note that there's nothing wrong with the 0.1.36 release it just didn't make it to PyPI and the installer script is missing.

---

_Comment by @blakete on 2024-04-22 21:43_

@zanieb understood. Thanks for your quick response and resolution of the issue!

---

_Comment by @zanieb on 2024-04-22 22:06_

That release is fixed now too.

---
